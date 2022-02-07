---
title: Django：Postgresql Expresssion索引
date: 2021-03-19 17:46:18
tags: [django, python, postgresql]
---

在Django中无法实现Expression索引，至少正常途径是做不到的：

```python
# django.db.models.indexes.Index
class Index:
    def create_sql(self, model, schema_editor, using='', **kwargs):
        fields = [model._meta.get_field(field_name) for field_name, _ in self.fields_orders]
        col_suffixes = [order[1] for order in self.fields_orders]
        condition = self._get_condition_sql(model, schema_editor)
        return schema_editor._create_index_sql(
            model, fields, name=self.name, using=using, db_tablespace=self.db_tablespace,
            col_suffixes=col_suffixes, opclasses=self.opclasses, condition=condition,
            **kwargs,
        )

# django.db.backends.base.schema.BaseDatabaseSchemaEditor
class BaseDatabaseSchemaEditor:
    def _create_index_sql(self, model, fields, *, name=None, suffix='', using='',
                          db_tablespace=None, col_suffixes=(), sql=None, opclasses=(),
                          condition=None):
        """
        Return the SQL statement to create the index for one or several fields.
        `sql` can be specified if the syntax differs from the standard (GIS
        indexes, ...).
        """
        tablespace_sql = self._get_index_tablespace_sql(model, fields, db_tablespace=db_tablespace)
        columns = [field.column for field in fields]
        sql_create_index = sql or self.sql_create_index
        table = model._meta.db_table

        def create_index_name(*args, **kwargs):
            nonlocal name
            if name is None:
                name = self._create_index_name(*args, **kwargs)
            return self.quote_name(name)

        return Statement(
            sql_create_index,
            table=Table(table, self.quote_name),
            name=IndexName(table, columns, suffix, create_index_name),
            using=using,
            columns=self._index_columns(table, columns, col_suffixes, opclasses),
            extra=tablespace_sql,
            condition=self._index_condition_sql(condition),
        )
```

<!--more-->

不过，[django-expression-index](https://github.com/kmierzeje/django-expression-index)实现了一个hacking的做法，
那便是在创建Statement对象之后，强制修改columns属性，以达到能够正常生成sql语句的目的，下面是完整代码：

```python
from django.db import models
from django.db.backends.utils import names_digest, split_identifier
from functools import partial

class ExpressionIndex(models.Index):
    def __init__(self, *, expressions=(), name=None, db_tablespace=None, opclasses=(), condition=None):
        super().__init__(fields=[str(e) for e in expressions],
                         name=name, db_tablespace=db_tablespace,
                         opclasses=opclasses, condition=condition)
        self.expressions=expressions

    def deconstruct(self):
        path, args, kwargs = super().deconstruct()
        kwargs.pop('fields')
        kwargs['expressions'] = self.expressions
        return path, args, kwargs

    def set_name_with_model(self, model):
        self.fields_orders=[(model._meta.pk.name,'')]
        _, table_name = split_identifier(model._meta.db_table)
        digest=names_digest(table_name, *self.fields, length=6)
        self.name=f"{table_name[:19]}_{digest}_{self.suffix}"

    def create_sql(self, model, schema_editor, using='', **kwargs):

        class Descriptor:
            db_tablespace=''
            def __init__(self, expression):
                self.column=str(expression)

        col_suffixes = [''] * len(self.expressions)
        condition = self._get_condition_sql(model, schema_editor)
        statement= schema_editor._create_index_sql(
            model, [Descriptor(e) for e in self.expressions],
            name=self.name, using=using, db_tablespace=self.db_tablespace,
            col_suffixes=col_suffixes, opclasses=self.opclasses, condition=condition,
            **kwargs,
        )

        compiler=model._meta.default_manager.all().query.get_compiler(connection=schema_editor.connection)
        statement.parts['columns'] = ", ".join(
            [self.compile_expression(e, compiler) for e in self.expressions])
        return statement

    def compile_expression(self, expression, compiler):
        def resolve_ref(original, name, allow_joins=True, reuse=None, summarize=False, simple_col=False):
            return original(name, allow_joins, reuse, summarize, True)

        query=compiler.query
        query.resolve_ref=partial(resolve_ref, query.resolve_ref)
        expression=expression.resolve_expression(query, allow_joins=False)
        sql, params=expression.as_sql(compiler, compiler.connection)
        return sql % params
```

由于在Django3.1.x中移除了参数`simple_col`，`query.resolve_ref`执行失败。将代码修改如下：

```python
from django.db.backends.utils import split_identifier, names_digest
from django.db.models import Index


class ExpressionIndex(Index):
    def __init__(self, *, fields=(), expressions=(), name=None, db_tablespace=None, opclasses=(), condition=None):
        super().__init__(fields=fields,
                         name=name, db_tablespace=db_tablespace,
                         opclasses=opclasses, condition=condition)
        self.expressions = expressions

    def deconstruct(self):
        path, args, kwargs = super().deconstruct()
        kwargs['expressions'] = self.expressions
        return path, args, kwargs

    def set_name_with_model(self, model):
        _, table_name = split_identifier(model._meta.db_table)
        digest = names_digest(table_name, *self.fields, length=6)
        self.name = f'{table_name[:19]}_{digest}_{self.suffix}'

    def create_sql(self, model, schema_editor, using='', **kwargs):
        class FieldDescriptor:
            db_tablespace = ''

            def __init__(self, expression):
                self.column = str(expression)

        condition = self._get_condition_sql(model, schema_editor)
        statement = schema_editor._create_index_sql(
            model, [FieldDescriptor(e) for e in self.expressions],
            name=self.name, using=using, db_tablespace=self.db_tablespace,
            col_suffixes=[''] * len(self.expressions), opclasses=self.opclasses, condition=condition,
            **kwargs,
        )

        compiler = model._meta.default_manager.all().query.get_compiler(connection=schema_editor.connection)
        statement.parts['columns'] = ','.join([self.compile_expression(e, compiler) for e in self.expressions])
        return statement

    @classmethod
    def compile_expression(cls, expression, compiler):
        expression = expression.resolve_expression(compiler.query, allow_joins=False)
        sql, params = expression.as_sql(compiler, compiler.connection)
        return sql % params
```

另外一种更值得推荐做法是{% post_link django-run-rawsql-in-migration Django：在Migrations中执行原生SQL %}。

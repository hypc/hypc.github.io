---
title: Django：在Migrations中执行原生SQL
date: 2021-03-19 16:36:23
tags: [django]
---

First, generate a new empty migration:

```bash
$ python manage.py makemigrations app --empty --name add_index_runsql
Migrations for 'app':
  app/migrations/0002_add_index_runsql.py
```

Next, edit the migration file and add a RunSQL operation:

```python
# migrations/0002_add_index_runsql.py

from django.db import migrations, models

class Migration(migrations.Migration):
    atomic = False

    dependencies = [
        ('app', '0001_initial'),
    ]

    operations = [
        migrations.RunSQL(
            'CREATE INDEX app_sale_sold_at_b9438ae4 ON app_sale (sold_at);',
            reverse_sql='DROP INDEX "app_sale_sold_at_b9438ae4";',
        ),
    ]
```

<!--more-->

When you run the migration, you will get the following output:

```bash
$ python manage.py migrate
Operations to perform:
  Apply all migrations: app
Running migrations:
  Applying app.0002_add_index_runsql... OK
```

migrate back to the initial migration:

```bash
$ python manage.py migrate 0001
Operations to perform:
  Target specific migration: 0001_initial, from app
Running migrations:
  Rendering model states... DONE
  Unapplying app.0002_add_index_fake... OK
```

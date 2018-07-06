---
title: Django多对多关联字段查询
date: 2018-07-06 13:32:46
tags: [django, python]
---

```python
class Author(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=40)
    email - models.EmailField()

class Book(models.Model):
    title = models.CharField(max_length=200)
    authors = models.ManyToManyField(Author)
```

### 从Book角度查询Author：

```python
book = Book.objects.get(id=1)
book.authors.all()  # 查询id为1的Book的所有Author
book.authors.filter(first_name='jack')  # 查询id为的Author中first_name为jack的Author
```

### 从Author角度查询Book：

```python
author = Author.objects.get(id=1)
author.book_set.all()   # 查询id为1的Author的所有Book
```

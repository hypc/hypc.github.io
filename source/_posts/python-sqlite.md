---
title: 在Python中使用SQLite
date: 2018-10-21 09:10:12
tags: [python, sqlite]
---

## SQLite简介

[SQLite][]是一个软件库，实现了自给自足的、无服务器的、零配置的、事务性的SQL数据库引擎。[SQLite][]是在世界上最广泛部署的SQL数据库引擎。

SQLite优点：

* 不需要一个单独的服务器进程或操作的系统（无服务器的）。
* SQLite不需要配置，这意味着不需要安装或管理。
* 一个完整的SQLite数据库是存储在一个单一的跨平台的磁盘文件。
* SQLite是非常小的，是轻量级的，完全配置时小于 400KiB，省略可选功能配置时小于250KiB。
* SQLite是自给自足的，这意味着不需要任何外部的依赖。
* SQLite事务是完全兼容ACID的，允许从多个进程或线程安全访问。
* SQLite支持SQL92（SQL2）标准的大多数查询语言的功能。
* SQLite使用ANSI-C编写的，并提供了简单和易于使用的 API。
* SQLite可在UNIX（Linux, Mac OS-X, Android, iOS）和Windows（Win32, WinCE, WinRT）中运行。

<!--more-->

## Python - SQLite

[SQLite3][SQLite]可使用[sqlite3][python sqlite3]模块与Python进行集成，Python2.5.x以上版本默认自带了该模块。

### 连接数据库

使用下面代码连接数据库：

```python
import sqlite3

conn = sqlite3.connect('test.db')
```

上面代码表示连接一个`test.db`（在当前目录下）的数据库，如果不存在，则创建。
数据库名也可以使用特殊的名字**`:memory:`**，它表示数据库只在内存中创建。

### CREATE TABLE

```python
import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()
sql = '''CREATE TABLE COMPANY (
    ID INT PRIMARY KEY     NOT NULL,
    NAME           TEXT    NOT NULL,
    AGE            INT     NOT NULL,
    ADDRESS        CHAR(50),
    SALARY         REAL
);'''
c.execute(sql)
conn.commit()
conn.close()
```

### INSERT

```python
import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()

c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) VALUES (1, 'Paul', 32, 'California', 20000.00);")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) VALUES (2, 'Allen', 25, 'Texas', 15000.00);")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) VALUES (3, 'Teddy', 23, 'Norway', 20000.00);")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00);")

conn.commit()
conn.close()
```

### SELECT

```python
import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()

cursor = c.execute("SELECT ID, NAME, ADDRESS, SALARY FROM COMPANY;")
for row in cursor:
    print({
        'ID': row[0],
        'NAME': row[1],
        'ADDRESS': row[2],
        'SALARY': row[3],
    })

conn.close()
```

### UPDATE

```python
import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()

cursor = c.execute("UPDATE COMPANY SET SALARY=25000.00 WHERE ID=1;")
conn.commit()

cursor = c.execute("SELECT ID, NAME, ADDRESS, SALARY FROM COMPANY;")
for row in cursor:
    print({
        'ID': row[0],
        'NAME': row[1],
        'ADDRESS': row[2],
        'SALARY': row[3],
    })

conn.close()
```

### DELETE

```python
import sqlite3

conn = sqlite3.connect('test.db')
c = conn.cursor()

cursor = c.execute("DELETE FROM COMPANY WHERE ID=2;")
conn.commit()

cursor = c.execute("SELECT ID, NAME, ADDRESS, SALARY FROM COMPANY;")
for row in cursor:
    print({
        'ID': row[0],
        'NAME': row[1],
        'ADDRESS': row[2],
        'SALARY': row[3],
    })

conn.close()
```

[SQLite]: https://sqlite.org/
[python sqlite3]: https://docs.python.org/3/library/sqlite3.html

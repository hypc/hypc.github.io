---
title: Python解包
date: 2019-01-18 09:05:08
tags: [python]
---

昨天有个Python初学者问了一个问题：

```python
def test(a, b, c): pass
# 怎么用一个元组作为参数直接传递给test函数？
```

这里用到了一个解包的概念，可以直接使用`test(*(1, 2, 3))`。

## 解包可迭代对象

在Python中，可迭代对象都可以解包，如str、list、tuple、set、生成器、迭代器...

```python
>>> a0, a1, *args, a_1 = "Hello world!"     # 解包str
>>> a0
'H'
>>> a_1
'!'
>>> a0, a1, *args, a_1 = [0, 1, 2, 3, 4, 5]     # 解包list
>>> a0
0
>>> a_1
5
>>> a0, a1, *args, a_1 = range(10)      # 解包生成器
>>> a0
1
>>> a_1
9
>>> a0, a1, (a2_0, a2_1, *a2_args), *args = ["0", "a", (0, 1, 2, 3, 4)]     # 双重解包
>>> a0
'0'
>>> a1
'a'
>>> a2_0
0
>>> a2_args
[2, 3, 4]
```

<!--more-->

## 解包dict

正常情况下，dict也是可以迭代的，只不过只能迭代key值，不能迭代value，
也就是说，dict是可以正常解包的。由于dict是无序的，解包也是无序的。

```python
>>> a, b, c = {'a': 1, 'b': 2, 'c': 3}
>>> a
'a'
>>> b
'c'
```

## 函数调用中的解包操作

普通解包，可以在前面添加一个`*`号：

```python
>>> print(*[1, 2, 3])       # 解包list
1 2 3
>>> print(*{1, 2, 3, 4})    # 解包set
1 2 3 4
>>> print(*'Hello world!')  # 解包str
H e l l o   w o r l d !
>>> print(*{'a': 1, 'b': 2})    # 解包dict的key值
a b
```

dict解包还可以用于关键字参数上，在前面添加两个`*`号，如：

```python
def test(a=1, b=2, c=3):
    print(a, b, c)

test(**{'a': 10, 'b': 20, 'c': 30})
# ==> 10 20 30
```

在Python3.5之后，Python还支持同时对多个值进行解包：

```python
>>> print(*(1, 2), *[3], *'Hello world!')
1 2 3 H e l l o   w o r l d !
```

也可以将解包用在表达式中：

```python
>>> *range(3), *[3]
(0, 1, 2, 3)
>>> {*range(4), 4}
{0, 1, 2, 3, 4}
>>> {'a': 1, **{'b': 2}}
{'a': 1, 'b': 2}
>>> {**{'a': 1, 'b': 2}, **{'c': 3, 'd': 4}}    # 合并两个dict
{'d': 4, 'a': 1, 'c': 3, 'b': 2}
```

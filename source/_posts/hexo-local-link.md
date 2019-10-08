---
title: Hexo引用自己的文章
date: 2019-10-08 14:01:19
tags: [hexo]
---

在用Hexo写文章时，经常需要引用自己的文章，而文章在发布之后地址为`/年/月/日/文章名`，
这样的话，使用markdown格式（`[]()`）引用的话会变得很不方便。

万幸的是，hexo提供了内置的语法来引用文章：

```hexo
{% post_link 文章文件名（不要后缀） 文章标题（可选） %}
```

例如：

```hexo
{% post_link Hello-World %}
{% post_link Hello-World 你好世界 %}
```

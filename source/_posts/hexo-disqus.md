---
title: Hexo集成Disqus
date: 2019-08-16 13:31:37
tags: [hexo, disqus]
---

[Disqus][]是一个非常流行的为网站集成评论系统的工具，同样，[Hexo][]也可以集成[Disqus][]以便可以和读者交流。

[Disqus]: https://disqus.com/
[Hexo]: https://hexo.io/

## 创建站点

登录Disqus后，创建一个站点：

![](/images/disqus-1.png)

根据提示填写表单，注意这里生成的`shortname`，在后面配置Hexo时会用到。

<!--more-->

## 配置Hexo

修改主题配置：

```yaml
disqus:
  enable: true
  shortname: your_shortname
  count: true
  lazyload: true
```

执行`hexo serve`命令，打开文章，效果如下：

![](/images/disqus-2.png)

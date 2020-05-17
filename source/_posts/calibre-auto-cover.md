---
title: Calibre一键生成封面
date: 2020-01-21 11:06:25
tags: [calibre, shell]
---

刚刚写了个爬虫，抓取了infoq上的电子书：

```
minibooks/
    |-- AI前线特刊：AI领域2017进展总结/
    |   |-- AI前线特刊：AI领域2017进展总结.pdf
    |   |-- cover.jpg
    |   |-- metadata.json
    |   |-- metadata.opf
    |-- AI商业化下的技术演进/
    |   |-- AI商业化下的技术演进.pdf
    |   |-- cover.jpg
    |   |-- metadata.json
    |   |-- metadata.opf
    |-- CNUTCon特刊：智能时代运维最佳实践/
    |   |-- CNUTCon特刊：智能时代运维最佳实践.pdf
    |   |-- cover.jpg
    |   |-- metadata.json
    |   |-- metadata.opf
    |-- Cloud 2.0时代，华为云的技术探秘与实践/
    |   |-- Cloud 2.0时代，华为云的技术探秘与实践.pdf
    |   |-- cover.jpg
    |   |-- metadata.json
    |   |-- metadata.opf
```

<!--more-->

在使用calibre导入书籍的时候发现，没有正常导入封面，
然后网上各种查找，没有发现类似问题的解决方案，后来一想，写个命令重新生成一下封面吧：

```bash
cd $calibre_library/

for book in */*; do
    if [[ -d "$book" ]]; then
        pushd "$book" >> /dev/null
        if [[ ! -f cover.jpg ]]; then
            ebook-meta *.pdf --get-cover cover.jpg >> /dev/null
        fi
        popd >> /dev/null
    fi
done
```

重新打开calibre，效果如下：

![](/images/calibre-auto-cover-1.png)

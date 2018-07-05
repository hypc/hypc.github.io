---
title: gitbook
date: 2018-07-05 13:38:36
tags: [gitbook, nodejs]
---

## 安装

```shell
npm install -g gitbook-cli
```

## 配置

修改文件`book.json`:

```json
{
    "title": "$title$",
    "author": "$author$",
    "description": "$description$",
    "repository": {
        "type": "git",
        "url": "$url$"
    },
    "plugins": [
        "theme-opendocs", "advanced-emoji", "bg-nest",
        "ace", "codeblock-label",
        "simpletabs",
        "include",
        "katex",
        "-highlight", "prism",
        "-lunr", "-search", "search-pro",
        "-sharing",
        "-fontsettings"
    ],
    "gitbook": ">=3.0.0",
    "pluginsConfig": {}
}
```

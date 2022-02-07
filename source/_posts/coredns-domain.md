---
title: CoreDNS泛域名解析
date: 2021-08-07 14:19:00
tags: [coredns]
---

借助 `template` 插件可以实现泛域名解析，语法如下：

```
template CLASS TYPE [ZONE...] {
    match REGEX...
    answer RR
    additional RR
    authority RR
    rcode CODE
    fallthrough [ZONE...]
}
```

```
.:53 {
    errors
    health
    ready
    template IN A domain.local {
        answer "{{ .Name }} 60 IN A 192.168.23.7"
        fallthrough
    }
    forward . 8.8.8.8
    cache 30
    loop
    reload
    loadbalance
}
```

**References**

* [CoreDNS plugin: template](https://coredns.io/plugins/template/)

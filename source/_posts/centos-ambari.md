---
title: Centos7安装Ambari服务
date: 2018-12-25 08:42:00
tags: [centos, hadoop, ambari, bigdata]
---

Centos安装Ambari服务非常简单，执行下面命令即可：

```bash
# Ambari Server
wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.7.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo
yum install -y ambari-server
# 配置初始化信息，都默认即可
ambari-server setup
```

Ambari Server安装完成之后可使用浏览器访问[http://your-host:8080]()，使用`admin/admin`登录。

**注意**：在配置集群之前需要先设置后各主机的hostname，否则后面添加agent有可能会找不到server。

更详细的信息可参考：[Deploy_and_Configure_a_HDP_Cluster][]

[Deploy_and_Configure_a_HDP_Cluster]: https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.0.0/bk_ambari-installation/content/ch_Deploy_and_Configure_a_HDP_Cluster.html

---
title: 使用Vagrant一键部署Ambari服务
date: 2018-12-25 15:48:57
tags: [vagrant, hadoop, ambari, bigdata]
---

`Vagrantfile`文件内容如下：

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

$num_instances = 2
$instance_name_prefix = "ambari"
$root_domain = "example.com"

Vagrant.configure("2") do |config|
    config.vm.box = "centos/7"
    config.vm.box_check_update = false

    config.vm.define vm_name = "ser" do |ser|
        ser.vm.hostname = "#{$instance_name_prefix}.#{$root_domain}"
        ser.vm.network "private_network", ip: "192.168.56.20"
        ser.vm.provider "virtualbox" do |vb|
            vb.memory = "1024"
        end
        ser.vm.provision :shell, inline: <<-SHELL
            yum install -y git vim tree cmatrix curl wget net-tools
            wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.7.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo
            yum install -y ambari-server
        SHELL
    end

    (1..$num_instances).each do |i|
        config.vm.define vm_name = "agent-%02d" % i do |agent|
            agent.vm.hostname = "%s-%02d.%s" % [$instance_name_prefix, i, $root_domain]
            agent.vm.network "private_network", ip: "192.168.56.#{20 + i}"
            agent.vm.provider "virtualbox" do |vb|
                vb.memory = "4096"
            end
        end
    end

end
```

`vagrant up`之后，使用`vagrant ssh ser`连接到ser主机，
使用root身份运行`ambari-server setup`命令配置ambari服务信息，
完成之后使用浏览器访问[http://your-host:8080]()，使用`admin/admin`登录。

更详细的信息可参考：[Deploy_and_Configure_a_HDP_Cluster][]

[Deploy_and_Configure_a_HDP_Cluster]: https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.0.0/bk_ambari-installation/content/ch_Deploy_and_Configure_a_HDP_Cluster.html

---
title: GreenPlum
date: 2020-01-02 10:29:41
tags: [postgresql, greenplum]
---

[Greenplum][]数据库，一个用于分析、机器学习和人工智能的开源大规模并行数据平台。

* **一次打包到处运行的平台**：不管是裸机、私有云还是公有云。不受限于硬件环境和平台，用户可以灵活的选择最适合自己的方案，迁移代价低。
  硬件环境的普适性，提供了极大的灵活性，解放了硬件平台的制约和绑定，为客户解决了后顾之忧。
  使用 Greenplum 可以在其间无缝迁移，开发、运维人员不需要学习新的数据库处理技术。
* **处理和分析各种数据源的数据的平台**：支持各种数据源，包括 Hadoop、HIVE、HBase、S3、Gemfire、各种数据库和文件等，不需要移动数据，
  避免了数据加载的复杂性，和其带来的数据不一致的问题。
* **支持各种数据格式的平台**：不管是结构化、半结构化（XML、JSON、KV）还是非结构化，譬如文本数据、GIS数据、图数据等。
* **具有强大内核的平台**：Greenplum 具有强大的内核技术，包括数据水平分布、并行查询执行、专业优化器、线性扩展能力、多态存储、资源管理、高可用、高速数据加载等。
* **具备强大灵活性和可扩展性的平台**： 支持扩展（Extension）、自定义类型和函数、PXF和外部表技术。
  可以使用多种语言实现用户自定义函数和聚集，包括 PL/Python、PL/R、PL/Java、PL/Perl、PL/PGSQL 和 C 等。
* **支持标准的平台**：支持SQL、JDBC和ODBC等行业标准。经过半个多世纪的发展，SQL 成为了数据平台的万向头，
  向上可以连接各种 BI工具、可视化工具和数据分析工具，向下可以连接各种ETL工具、各种数据源和各种格式的数据等。
* **集成数据分析平台**：支持商业智能（BI）、文本、GIS、图、图像、流式数据处理等。
  通过Pivotal开源的 Apache 顶级项目 MADlib，Greenplum 可以在数据内部运行 50 多种数据分析和机器学习算法。
  MADlib 提供 SQL 接口进行数据分析，大大降低了数据分析的门槛；MADlib 内建于数据库内，使用 MPP 的优势，提高了分析的效率；
  MADlib可以在全量数据而不是抽样数据上进行分析，提高了精度。
* **开放源代码且持续大力投入的平台**： 2017年 Pivotal 在 github 的开源贡献列表中全球排名第四左右。采用开源方案，不担心后门问题和被锁定问题。开源还可以构建更好的生态。
* **采用敏捷软件开发方法开发的平台**：Greenplum 采用敏捷方法开发，实现了快速迭代、持续发布和质量内建。
  2017年 Greenplum 发布了10个版本，以前发布一个版本需要1个月左右，现在只需要十几个小时。
* **具备企业级稳定性的平台**：Greenplum 经过十多年发展，有大量活跃客户，大量数百节点集群为全球2000强企业生产系统提供服务，稳定性非常高。
* **具备成熟生态系统的平台**：Greenplum 生态非常完善，有大量的合作伙伴。

[Greenplum]: https://greenplum.org/

<!--more-->

## 单节点安装

基于Ubuntu18.04：

```bash
## install greenplum
sudo add-apt-repository -y ppa:greenplum/db
sudo apt update -y && sudo apt install -y greenplum-db git tree vim
echo "export GPHOME=/opt/greenplum-db-6.2.1" >> ~/.bashrc
echo '. $GPHOME/greenplum_path.sh' >> ~/.bashrc
source ~/.bashrc

## init greenplum & start greenplum
cp $GPHOME/docs/cli_help/gpconfigs/gpinitsystem_singlenode .
sed -i '/^#/d' gpinitsystem_singlenode
sed -i '/^$/d' gpinitsystem_singlenode
echo "$(hostname)" > hostlist_singlenode
sed -i "s/^MASTER_HOSTNAME.*$/MASTER_HOSTNAME=$(hostname)/g" gpinitsystem_singlenode
primary_dir=$(pwd)/primary
mkdir -p ${primary_dir}
sed -i '/^declare -a DATA_DIRECTORY/d' gpinitsystem_singlenode
echo "declare -a DATA_DIRECTORY=(${primary_dir} ${primary_dir})" >> gpinitsystem_singlenode
master_dir=$(pwd)/master
mkdir -p ${master_dir}
sed -i '/^MASTER_DIRECTORY/d' gpinitsystem_singlenode
echo "MASTER_DIRECTORY=${master_dir}" >> gpinitsystem_singlenode

gpssh-exkeys -f hostlist_singlenode
gpinitsystem -c gpinitsystem_singlenode

## config client authorization
echo 'host all all 192.168.0.0/16 trust' >> ${master_dir}/gpsne-1/pg_hba.conf

## restart greenplum
gpstop -d ${master_dir}/gpsne-1/
gpstart -d ${master_dir}/gpsne-1/
```

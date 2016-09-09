title: mysql-index
date: 2016-09-05 20:49:22
tags: mysql
categories: mysql
---

# mysql索引
![](http://oc9orpe44.bkt.clouddn.com/16-9-9/21794403.jpg)

聚簇索引的叶节点就是数据节点，而非聚簇索引的页节点仍然是索引检点，并保留一个链接指向对应数据块。

* myisan引擎

MyISAM 为非聚簇索引（NonCluster Index）
MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录

* innodb引擎

InnoDB的主索引是聚簇索引（Cluster Index），数据和Key本身都会存储在B+Tree的叶子节点
InnoDB的辅助索引本身主要是记录主索引的Key，最终查找数据还是从辅助索引再去主索引查找纯读模式下，性能对比MyISAM引擎会稍有影响（辅助索引需要3次io，主索引需要3次io，MyISAM可能只需要4次IO）

**如果非特殊原因，强烈建议使用  InnoDB（XtraDB）存储引擎！**
原因：（对比MyISAM or Aria）更稳定可靠的数据存储和索引结构；整个存储引擎设计思想更可靠先进，接近于Oracle、SQL Server级别的数据库（有兴趣可以去阅读源码了解细节）更多可靠特性支持，比如事务、外键等支持（支付等关键领域事务非常重要）运行更稳定，不论读写数据的量级，都能够保证比较稳定的性能响应更好地崩溃恢复机制，特别利用一些Percona的一些工具，更有效运维InnoDBMySQL 5.5+ 对比 MySQL 5.1.x 总体功能和性能提升太多，改进太多

###mysql优化
* 硬件
    1. CPU：使用多核CPU，能够充分发挥新版MySQL多核下的效果，建议4核以上    2. 内存：如果数据量比较大，建议使用不要低于20G内存的服务器；    3. 硬盘：如果条件允许使用SSD硬盘，TPS能够达到2000或者更高；SAS硬盘的TPS极限也可能在1000左右，对性能的提升是非常显著；为保证数据可靠性，建议适合的RAID方案    4. 网卡：保证网卡不是瓶颈，保证足够的吞吐量，建议千兆网卡
* linux服务器关键配置
    1. 文件打开描述符：/etc/security/limits.conf 中 nofile、/proc/sys/fs/nr_open    2. 可分配文件句柄数：/etc/sysctl.conf 中 fs.file-max、/proc/sys/fs/file-max    3. 进程数：/etc/security/limits.conf 中的 nproc    4. 线程数：/proc/sys/kernel/thread-max    5. 其他TCP和网络相关选项：/etc/sysctl.conf    6. 这些都可以通过ulimit 或 直接调整 /proc 中变量进行临时修改    6. 建议关闭SWAP分区（内存要足够大才行）；如果数据太大，为了防止夯死主机，可以设置2G左右的SWAP分区
* msyql 配置等


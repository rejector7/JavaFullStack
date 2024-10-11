# 1. Hbase概述

## 1.3 Hbase体系结构

主从模型：master-regionServer
* hbaseClient：客户端读写入口
  * meta表：定位行所在regionServer
* Zk：协助master管理。zk主要做一些轻量级的工作。
  * 选举master
  * 保存&管理核心元数据。定位meta表所在的regionServer。
  * 心跳regionServer。出问题通知master进行恢复。
* master：一个active Master。宕机后Zk会选举出新的master。负责管理hbase集群。
  * 表级别的CRUD，Compaction
  * 管理RegionServer：负载均衡、宕机恢复等。
  * 清理HLog、HFile
* regionServer：数据行级别的CRUD
  * HLog：WAL，用于恢复、主从复制。保存在hdfs中。
  * Region：regionServer包含多个region。Log-Structured Merged Tree（LSM）结构。按rowKey水平划分为不同region。
    * store：按照列簇分为多个store
      * memStore：并发有序跳表。每个store两个memStore（mutable和immutable用于互换）定期会flush成Hfile
  * blockCache：读缓存。多个region公用一个
* dfsClient：regionServer调用dfsClient
* hdfs dn：实际存储Hfile、Hlog数据。通常regionServer和dn会尽量在同一台物理机上。

# 2. 基础数据结构与算法

* 一个列簇 = 一个LSM树
  * 内存部分 + 磁盘部分：memStore + HFile
    * 内存部分：SkipList：并发性能好。CRUD都是LogN
    * 磁盘部分：多个有序KV的HFile文件。
    * 每一条数据是一个KV，存储的是操作记录（通过type标识操作类型），而不仅是数据。所以才叫做LSM，存储日志而不仅是数据。
    * 写优化：随机写通过memStore改为顺序写HFile。
    * 读优化：Compaction，布隆过滤器
  * 布隆过滤器：假阳真阴

* Compaction
  * 大量HFile，影响读性能，定期Compaction
    * major compaction：全局合并
    * minor compaction：局部合并

# 3. Hbase依赖服务

# 4. Hbase客户端

# 5. RegionServer的核心模块

# 6. Hbase读写流程

# 7. Compaction实现

# 8. 负载均衡实现

# 9. 宕机恢复原理

# 10. 复制

# 11. 备份与恢复

# 12. Hbase运维

# 13. Hbase系统调优

# 14. Hbase运维案例分析

# 15. Hbase 2.x核心计数

# 16. 高级话题

# 附录A Hbase热门问题集锦

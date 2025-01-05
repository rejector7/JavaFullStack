
# 第6章 深入kafka

## 6.1 集群间的成员关系

旧版：
* zk
* broker
* topic/partition/副本

新版：
* Kraft controller
* broker
* topic/partition/副本

## 6.2 控制器

旧版zk有两个功能：
* 选举控制器
* 保存元数据

控制器主要有两个功能：
* 选举分区首领
* 管理元数据

新版Kraft控制器将zk和原控制器功能统一到一起。不再使用zk。

旧版zk vs Kraft：
* 使用难度：zk是个独立的分布式系统，需要了解两个系统。Kraft内嵌进kafka。
* 功能设计：zk和controller功能重合。Kraft将所有管理功能整合到controller。
* 重启运维性能：zk方式，控制器重启需要从zk获取所有元数据，容易有瓶颈。Kraft主从控制器，随时热切。

业务上。Kraft当broker数量较多（超过400），会产生cpu瓶颈和带宽瓶颈。

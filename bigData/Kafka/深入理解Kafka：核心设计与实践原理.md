## 第五章 日志存储

### 5.2 日志格式的演变

#### 5.2.3 消息压缩
生产者设置压缩格式。
一般是对批量消息集进行压缩。

## 第七章 深入客户端

### 7.1 分区分配策略

* RangeAssigner
* RoundRobinAssigner
* StickyAssigner
* 自定义Assigner
* 组内广播消费：笛卡尔积。
  * 消费位移提交会失效，因为在broker端不同消费者提交的位移会互相覆盖。因此需要客户端自己保存消费位移。
  * 只能用assign模式（手动分配分区），而不是依赖消费者协调器

 分区分配分成两类：
 * assign手动分配
 * 消费者协调器分配

消费模式：
* poll/pull：消费者拉取消息，自动管理偏移量
* push：broker主动push（Kafka不支持真正的push）

### 7.2 消费者协调器和组协调器

如何协调分配策略和决定如何分配？

* 旧版：zk watcher
* 新版：
  * broker：GroupCoordinator
    * 负责管理消费组元信息
    * 负责协调分区分配
  * consumer：ConsumerCoordinator
    * 心跳检测
  * 二者交互决定分配。

### 7.3 __consumer_offsets剖析

* 每个集群都有一个__consumer_offsets topic
* 不同消费组的消费位移提交到不同的__consumer_offsets broker中，该broker同时也是GroupCoordinator所在的节点。

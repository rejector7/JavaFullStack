# Disruptor Java Concurrency Programming Framework

doc website:
https://lmax-exchange.github.io/disruptor/

**基本介绍**

高性能线程间通信库。
主要解决队列通信中存在的两个问题：

* CPU级别的缓存失效（cache-miss）
* 锁机制造成的延迟

disruptor框架实现了：
* 硬件识别/同理机制，避免缓存失效？
* 无锁

## User Guide

**disruptor vs BlockingQueue**


**核心概念**

* multicast event：多播事件
  * 即事件会被发送给多个消费者
* 消费者依赖图
  * 用以实现实际业务中不同线程的依赖情况
  * Sequencer？
* 事件空间预分配
  * 降低gc造成的CPU时间
  * 通过在ringbuffer中预分配空间
* 无锁机制（可选）
  * 内存可见性和正确性由内存屏障（memory barrier）和CAS操作保证。
* 
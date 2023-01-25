# Hadoop权威指南（第四版）

## 第二部分 第七章 MapReduce的工作机制

### 7.1 MapReduce作业的运行机制

主要涉及作业和任务两个粒度的提交，执行，完成以及过程中的状态更新。

**MR作业的重要实体**

* client
* RM
* NM
* AM
* Common Task Containers
* HDFS

**作业提交**

client：提交作业到RM，waitForCompletion -> submit

1. get Application ID
2. check output dir：作业运行前需保证输出目录不存在
3. getSplits：调用inputformat，获取所有输入数据的分片信息。因此分片/inputformat是在client端运行的。
4. copy resources to hdfs：将jar包、分片信息、配置文件等从client拷贝到hdfs。之后的am/task将从这里获取相关信息。

RM：submitApplication，提交作业

**作业初始化**
RM：submitApplication -> Yarn scheduler：get AM container -> launch AM

AM主要作用以及其中的初始化工作：
* 接收进度跟踪，完成报告：初始化跟踪实体？簿记对象？
* 建立task
  * 从hdfs中获取input splits，建立task对象：此时还没分配container
  * 分配task ID
* setupJob：
  * OutputCommitter：建立输出目录，建立任务临时目录。output_dir/_temporary/1/_temporary/attempt_xxx_task_id_xx

**任务分配&执行**

1. AM：为任务请求资源/容器
2. RM：分配资源（节点/容器）给指定task
3. AM：通过指定节点的NM，start container
4. NM：launch task container/JVM
5. Task Container：run YarnChild
   1. copy resources：hdfs -> task container：任务jar包，配置信息等？
   2. setup：？
   3. commit：OutputCommit将输出数据从任务临时目录移动到job目录，task完成不会清理map临时文件，在job完成时才会进行清理工作


      






**各实体运行的作业部分**

* client：

### 7.2 失败

失败实体：
* 任务
* am
* nm
* rm
一般情况仅考虑任务、am、client失败的情况。

**任务失败**
1. 用户代码失败
map/reduce JVM报错 -> am -> map/reduce中的用户日志(user log)
-> 任务failed，释放资源
2. JVM本身问题导致退出
JVM退出 -> NM -> am标记为failed
3. 任务挂起
长时间没有进度更新到am -> am标记任务为failed -> 杀死JVM进程
可能是IO/计算等问题导致处理速度慢。


### 7.3 shuffle和排序

配置调优！

* 合并因子
* reduce复制线程、reduce分区线程
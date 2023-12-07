# 第一章大数据处理框架概览
## 大数据处理框架的四层结构

# 第二章 应用运行的基本流程
## Spark应用例子


# Spark逻辑处理流程（逻辑计划）
## Logic Plan生成方法
### RDD之间的数据依赖关系
* 窄依赖
* 宽依赖

## 常用transformation数据操作
* map & mapValues
  * 多分区之间无序，分区内部有序（输入有序的情况下）
* filter & filterByRange
* flatMap & flatMapValues
  * 只会展平一层，如有多层需全部展平，需重复调用flatMap
* sample & sampleByKey
* mapPartitions & mapPartitionsWithIndex
* partitionBy
  * 水平划分 （parallelize默认） 根据元素位置（索引）
  * HashPartitioner 
  * RangePartitioner 根据元素大小关系
* groupByKey
* reduceByKey
* aggregateByKey
* combineByKey
* foldByKey
* coGroup/groupWith
* join
  * 如join、intersection、distinct等涉及对多个rdd中相同key数据处理的操作，都会产生shuffleDependency
* cartesian
* sortByKey
* coalesce
* repartition
* intersection
* distinct
* union
* zip & zipPartitions
* zipWithIndex & zipWithUniqueId
* subtractByKey
* subtract
* sortBy
* glom
  
## 常用action()数据操作
transformation和action的区别：
* 返回类型：RDD & 数值、数据结构
* 数据收集：分布式 & 大部分要收集到driver（数据量较大时内存可能有较大瓶颈）

* count/countByKey/countByValue
* collect&collectAsMap
* foreach&foreachPartitions
* fold/reduce/aggregate
  * 和funcByKey的区别是方法针对的数据范围不同：全量和部分数据
* treeAggregate&treeReduce
* reduceByKeyLocality
* take/first/takeOrdered/top
* max/min
* isEmpty
* lookup
* saveAsTextFile/saveAsObjectFile/saveAsSequenceFile/saveAsHadoopFile
  

# Spark物理执行计划（Physical plan）
## 物理计划生成方法
* action -> 划分job
* shuffleDependency -> 划分stage
* 分区 -> 划分task

## 常用数据操作生成的物理执行计划
根据依赖方式划分stage

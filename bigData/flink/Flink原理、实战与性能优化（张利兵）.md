# 第四章
## 4.3 窗口计算
### 4.3.1 Windows Assigner
定义窗口分配方式：
* 滚动窗口（TumblingWindows）
  * 时间类型
  * 窗口时间
  * 窗口起始时间偏移量
* 滑动窗口（SlidingWindows）
  * 滑动时间
* 会话窗口（SessionWindows）
  * session gap
  * 动态Gap
* 全局窗口（GlobalWindows）
  * 没有窗口边界
  * 需要借助trigger触发
 
### 4.3.2 Windows Function
定义窗口内数据的计算方式：
* ReduceFunction
  * 增量计算
* AggregationFunction
  * 增量计算
  * 更通用
* ProcessWindowFunction
  * 全量计算
  * 复杂逻辑
  * 操作窗口的状态数据、元数据
* 增量 + ProcessWindowFunction
  * 增量用于计算，ProcessWindowFunction用于获取窗口元数据/状态
 
**ProcessWindowFunction状态操作**
状态类型分类（根据存储值）：
* 值状态（ValueState)：存储单一值。
* 列表状态（ListState）：存储一个元素列表。

状态类型分类（根据生命周期）：
* windowState
  * 独立窗口状态
  * 处理迟到元素等多次触发的情况
  * 处理复杂逻辑，需要中间状态管理
* globalState
  * 作业级别的全局状态
  * 没有显式的跨窗口全局状态定义
  * 通过BroadcastState进行广播来实现
 


### 4.3.3 Trigger窗口触发器
触发窗口计算，可以实现多次触发
* EventTimeTrigger：事件时间 & 水印
* ProcessTimeTrigger
* ContinuousEventTimeTrigger：间隔时间周期性触发
  * 这里的周期起始时间调整为整除间隔时间的
* ContinuousProcessTimeTrigger
* CountTrigger：按照数据量
* DeltaTrigger：按某种方式计算Delta

自定义Trigger需要实现的方法
* OnElement：逐个元素回调函数，一般会在这里面设置触发定时器registerEventTimeTimer
* OnEventTime：窗口触发后的回调函数，一般这里也会设置触发定时器registerEventTimeTimer，比如按照周期间隔设置下次触发时间。
* OnProcessTime
* OnMerge：SessionWindows需要
* Clear

间隔周期触发器可以通过registerEventTimeTimer和OnElement，OnEventTime的返回状态来实现。


### 4.3.4 Evictors 数据剔除器
自定义Evictors接口：
* evictBefore
* evictAfter：主要是性能考虑，剔除后续/下游不再需要的数据
* 
evict操作不保证顺序

### 4.3.5 延迟数据处理

**延迟到达、水印生成、窗口触发**
* 延迟到达
  * 窗口触发+延迟容忍内到达，触发更新或新数据
  * 超过延迟容忍，可以丢弃或旁路输出
* 乱序容忍，水位根据事件时间和乱序容忍（事件时间 - 乱序容忍）生成
* 窗口根据水位触发
  
举例：
如果计算十分钟窗口 08:00 到 08:10的数据，水印乱序设置为1分钟，allowedLateness设置为3分钟。
窗口08:00到08:10是在事件时间08:11时（此时会生成08:10的水印）触发计算，因为水印乱序设置为1分钟。
从08:11到08:14，窗口接受和处理属于该时段的延迟到达数据，因为设置的允许延迟为3分钟。
在08:14之后，标记为08:00到08:10时间窗口的更晚到达的数据将不会被这个窗口的计算考虑。



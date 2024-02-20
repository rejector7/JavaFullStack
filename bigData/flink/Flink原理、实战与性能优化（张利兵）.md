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
 
**延迟到达、水位生成、窗口触发**
* 延迟到达
  * 延迟容忍内到达，触发更新或新数据
  * 超过延迟容忍，可以丢弃或旁路输出
* 乱序容忍，水位根据事件时间和乱序容忍（事件时间 - 乱序容忍）生成
* 窗口根据水位触发
  
举例：如果是5秒乱序，那应该是在10:10:05的事件到达后，生成一个10:10:00的watermark，然后数据流根据这个watermark触发10:00到10:10的窗口计算。


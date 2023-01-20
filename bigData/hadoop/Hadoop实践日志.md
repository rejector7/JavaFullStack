# Hadoop实践日志

**hadoop windows本地运行**

安装过程参考：
* https://blog.csdn.net/Axel_Fran/article/details/121781347
* https://blog.csdn.net/yang_shibiao/article/details/122620656

主要步骤：
1. 安装hadoop
2. 下载winutils，注意版本对应
3. 复制到bin目录，将winutils & hadoop.dll复制到system目录
4. 新建项目：https://zhuanlan.zhihu.com/p/89617163
设置gradle.build文件，引入hadoop-common、hadoop-mapreduce-client-core依赖。

遇到问题：
ERROR StatusLogger No log4j2 configuration file found. Using default configuration: logging only errors to the console. Set system property 'log4j2.debug' to show Log4j2 internal initialization logging.
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument

问题原因：
https://juejin.cn/post/6844904095338201096
版本冲突

解决方式：
暂时删除hive依赖。

遇到问题：
Exception in thread "main" java.io.IOException: Cannot initialize Cluster. Please check your configuration for mapreduce.framework.name and the correspond server addresses. at org.apache.hadoop.mapreduce.Cluster.initialize(Cluster.java:116)

参考：https://stackoverflow.com/questions/33011134/cannot-initialize-cluster-please-check-your-configuration-for-mapreduce-framewo

解决方式：添加 implementation("org.apache.hadoop:hadoop-mapreduce-client-jobclient:$hadoopVersion")

运行成功。

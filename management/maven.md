### maven 构建流程

https://www.liaoxuefeng.com/wiki/1252599548343744/1309301196980257
https://www.runoob.com/maven/maven-plugins.html

### 各种命令的含义

* clean：清理
  * 清理target目录，重建新的空目录
  * 当代码修改时，清理后执行其他命令，保证代码生效
  * 一般来说可以不clean，除非不修改源代码，但是通过其他方式改动了jar包
  * 但是为了保险起见，一般都会加clean
* compile：编译
  * 将源代码转为，可被jvm执行的字节码文件
  * 见《深入理解java虚拟机》：
    * 词法分析、语义分析、语法分析等
* package：打包
  * 将源代码打包成可执行的jar包
* install：安装
  * 编译、打包、安装到本地仓库
## 第二章容器技术基础
核心功能：
* Cgroups：制造约束，各个进程组所能使用的资源限制，比如cpu、内存、磁盘等
* Namespace：隔离环境，修改进程视图，比如进程空间（PID）、文件系统（Mount）、网络设备（Network）等

容器只是一种特殊的进程。

**cgroups**
各种资源通过文件目录的方式组织在特定路径下，每个容器都会新建一组Cgroup，并配置相应的资源。

**mount namespace & chroot**
* mount namespace可以让容器只看到自己的挂载点，不会影响/访问其他进程的挂载文件系统。
* chroot修改进程的根目录。
结合这两者，可以为容器进程创建一个独立隔离的文件系统环境。

**容器镜像**
挂载在根目录上的文件系统，隔离环境，即为容器镜像：rootfs。

镜像升级：增量式构建。
镜像的所有分层存放在特定目录下，通过UnionFS进行组织，联合挂载到容器ID目录下。
这个挂载点就是一个完整的rootfs。

镜像分层：
* 可读写层：所有修改都记录在这层
* init层：临时修改，不会commit，比如/etc/host
* 只读层：原有镜像


**Volume**
通过挂载使用宿主机文件系统。

**Dockerfile命令底层原理**


**核心三功能**
* namespace：进程视图隔离
* cGroup：资源限制
* rootfs：独立根目录

## 第三章 Kubernetes设计与架构

容器：
* 容器镜像：静态
* 容器运行时：动态，包括namespace、cGroups。

架构：
* Master：控制节点
  * ApiServer
    * etcd
  * scheduler
  * controller-manager
* Node：计算节点
  * kubelet：负责和容器运行时交互
 
核心能力：容器编排
核心设计理念：声明式API

主要从两个方便考虑：
* 容器间关系：使用各种API对象来处理各种关系，比如访问关系、代理关系。
  * Pod：紧密协作的单实例（里面可能有多个服务容器）
  * Deployment：多实例管理
  * Service：Pod代理/负载均衡
* 容器运行形态：定时任务、守护进程、单次job等。
 
  

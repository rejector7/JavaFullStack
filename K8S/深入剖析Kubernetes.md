# 第二章容器技术基础
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

# 第三章 Kubernetes设计与架构

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

# 第五章编排原理

## Pod对象

源码：staging/src/k8s.io/api/core/v1/types.go:3537
* Metadata
* Spec
* Status

**详细说明和例子**
```
在Kubernetes（k8s）的Pod中，metadata字段用于存储与Pod相关的元数据信息，而spec（规范）字段则包含定义Pod的规格和配置信息。

metadata字段通常包含以下信息：
1. 名称（name）：Pod的名称用于唯一标识一个Pod实例。
2. 命名空间（namespace）：用于组织和隔离资源的逻辑区域。
3. 标签（labels）：一组键值对，用于对Pod进行分类和标记。
4. 注解（annotations）：键值对形式的元数据，用于存储关于Pod的其他辅助信息。

spec字段通常包含以下信息：
1. 容器和镜像（containers和images）：在spec中可以定义一个或多个容器，并指定使用的镜像、容器的资源需求等。
2. 重启策略（restartPolicy）：指定容器的重启行为，如总是重启、从不重启或根据容器退出原因进行重启。
3. 节点选择器（nodeSelector）：定义Pod所在节点的选择标准，控制Pod在哪些节点上运行。
4. 网络配置（networking）：包括Pod的网络模式、端口映射，以及与其他资源的网络连接等。

通过区分metadata和spec字段，可以理解metadata字段存储关于Pod的基本描述信息和用于分类、标记和注释的元数据，而spec字段定义了实际运行和配置Pod的规格和详情信息。
```

下面是一个比较复杂/完整的Pod对象的例子：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
  labels:
    app: my-app
    tier: frontend
  annotations:
    description: This pod runs the frontend of My App
spec:
  restartPolicy: Always
  nodeSelector:
    environment: production
  containers:
    - name: frontend
      image: my-app:latest
      ports:
        - containerPort: 80
      resources:
        limits:
          cpu: "1"
          memory: "512Mi"
        requests:
          cpu: "0.5"
          memory: "256Mi"
    - name: database
      image: mysql:latest
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: password123
        - name: MYSQL_DATABASE
          value: myappdb
        - name: MYSQL_USER
          value: myappuser
        - name: MYSQL_PASSWORD
          value: myapppassword
      ports:
        - containerPort: 3306
```

在上述例子中，Pod对象具有以下特征：
- 元数据（metadata）字段中指定了Pod的名称为"my-app-pod"，标签（labels）为"app: my-app"和"tier: frontend"，并添加了一个注解（annotations）用于描述Pod的用途。
- 规范（spec）字段中设置了重启策略为"Always"，节点选择器（nodeSelector）为"environment: production"，以确保Pod在标记为"environment: production"的节点上运行。
- Pod中包含了两个容器：一个名为"frontend"的容器，使用"my-app:latest"镜像，映射了容器端口80，并定义了资源需求和限制；另一个名为"database"的容器，使用"mysql:latest"镜像，并设置了一些环境变量和容器端口3306。
希望这个例子能够帮助你理解一个比较复杂/完整的Pod对象的结构和配置。


#                              k8s基本使用

##说明：

​        本文档旨在描述k8s的基本使用，帮助用户快速上手k8s部署项目。概况图如下：

<img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/k8sBasic.png">

## 1. 资源对象

​      kubenetes中的对象都可以在yaml文件中作为一种API类型来配置。简单分类如下

|  类别  |                    名称                    |
| :--: | :--------------------------------------: |
| 资源对象 | Pod、ReplicaSet、ReplicationController、Deployment、StatefulSet、DaemonSet、Job、CronJob、HorizontalPodAutoscaling |
| 配置对象 | Node、Namespace、Service、Secret、ConfigMap、Ingress、Label、ThirdPartyResource、 ServiceAccount |
| 存储对象 |         Volume、Persistent Volume         |
| 策略对象 | SecurityContext、ResourceQuota、LimitRange |



在这里，我们主要介绍下面几种资源对象。

<img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/k8sResource.png">

### 1. 1  Pod



***1.1.1 什么是pod?***

pod是kubernetes创建或部署的最小/最简单的基本单位，一个Pod代表集群上正在运行的一个进程。





***1.1.2 pod的使用***

​     kubenetes中pod的使用可分为2种主要方式：

- **pod中运行一个容器。**“one-container-per-Pod”模式是kubenetes最常见的用法，在这种情况下你可以将Pod视为单个封装的容器，但是kubenetes是直接管理Pod而不是容器。

  ​

- **Pods中运行多个需要一起工作的容器。** pod可以封装紧密耦合的应用，他们需要由多个容器组成，他们之间能够共享资源，这些容器可形成一个单一的内部Service单位。



**注意：**

你很少会直接在kubenetes中创建单个Pod。因为Pod的生命周期是很短暂的。当Pod被创建后（不论是由你还是由其他Controller创建），都会被kubenetes调度到集群中的Node上。直到Pod的进程终止、被删掉、因为缺少资源而被驱逐、或Node故障之前，这个Pod都会一直保持在那个Node上。

Pod不会自愈，如果Pod运行的Node故障，或是调度器本身故障，这个Pod就会被删除。同样，如果Pod所在Node缺少资源或Pod处于维护状态，Pod也会被驱逐。kubenetes使用Controller这个抽象层来管理Pod实例。虽然可以直接使用Pod，但是kubenetes中通常是使用Controller来管理Pod的。





***1.1.3 Pod提供2种共享资源：网络和存储***

- **网络**

   每个Pod被分配一个独立的Ip地址，Pod中的每个容器共享网络命名空间，包括Ip地址和网络端口。Pod内的容器可以使用localhost相互通信。当Pod中的容器与Pod外部通信时，他们必须协调如何使用共享网络资源（如端口）。


- **存储**

​       Pod可以指定一组共享存储Volumes。Pod中的所有容器都可以访问共享Volumes，允许这些容器共享数据。Volumes还可以用于Pod中的数据持久化，以防其中一个容器需要重新启动而丢失数据。





***1.1.4 Pod和Controller***

​         Controller可以创建和管理多个Pod,提供副本管理、滚动升级和集群级别的自愈能力。例如：一个Node故障，Controller就能自动将该节点上的Pod调度到其他健康的Node上。

​         Controller示例：

- ​         Deployment
- ​         StatefulSet
- ​         DaemonSet





### 1.  2   Deployment

​           Deployment为Pod和Replication Set（下一代Replication Controller） 提供声明式更新。

​           您只需要在Deployment中描述您想要的目标状态是什么，Deployment Controller就会帮您将Pod和ReplicaSet的实际状态改变到您的目标状态。



###1.3 ReplicationController和ReplicaSet

​      ReplicationController用来确保容器应用的副本数始终保持在用户定义的副本数，即如果有容器异常退出，会自动创建新的Pod来替代，而如果异常多出来的容器也会自动回收。

​      在新版本中建议使用ReplicaSet来取代ReplicationController。两者本质上是相同的，只是名字不太一样，并且ReplicaSet支持集合式的selector。

​      ***VS   Deployment***

​       虽然ReplicaSet可以独立使用，但一般还是建议使用Deployment来自动管理ReplicaSet，这样无需担心跟其他机制不兼容问题（比如ReplicaSet不支持rolling-update但deployment支持）



### 1.4  DemonSet

​       DaemonSet确保全部（或一些）Node上运行一个Pod的副本。当有Node加入集群时，也会为他们新增一个Pod，当有Node从集群移除时，这些Pod也会被回收，删除DaemonSet将会删除它创建的所有Pod。例如：在每个Node上运行日志收集Daemon，例如fluentd、logstash。

​       ***VS Replication Controller***

​       

| Replication Controller                   | Daemon Set                               |
| ---------------------------------------- | ---------------------------------------- |
| 为无状态的Service使用Replication Controller，像Frontend，实现对副本的数量进行扩缩容、平滑升级，比精确控制Pod运行在某个主机上要重要的多。 | 需要Pod副本总是运行在全部或特定主机上，并需要先于其他Pod启动，当这被认为非常重要的时候，应该用Daemon Set。 |



### 1.5  Job

​         Job负责批处理任务，即仅执行一次的任务，它保证批处理任务的一个或多个Pod成功结束。

​         ***Cron Job***用来管理基于时间的Job，即：

​         在给定时间点只运行一次、周期性的在给定时间点运行。



### 1.6  StatefulSet

​       StatefulSet作为Controller为Pod提供唯一的标识，它可以保证部署和scale的顺序。是为了解决有状态服务的问题。（对应Deployment是为无状态服务而设计），应用场景举例：

​        稳定的持久化存储、稳定的网络标识、有序部署与扩展。



## 2. yaml

​       yaml这种语言是以数据作为中心，而不是以置标语言为重点，是一种直观的能被电脑识别的数据序列化格式，是一个可读性高并且容易被人类阅读，容易和脚本语言交互，用来表达资料序列的编程语言。

​      Yaml语法规则：

- ​      大小写敏感
- ​      使用缩进表示层级关系
- ​      缩进时不允许使用Tal键，只允许使用空格
- ​      缩进的空格数目不重要，只要相同层级的元素左对齐即可
- ​      “#”表示注释

​    在kubenetes中只需要知道两种结构类型即可：

-    Lists
-    Map

​     使用Yaml用于k8s的定义带来的好处包括：

- ​     便捷性：不必添加大量的参数到命令行中执行命令
- ​     可维护性：Yaml文件可以通过源头控制，跟踪每次操作
- ​     灵活性：Yaml可以创建比命令行更加复杂的结果



## 3. 操作

###           3.1 dashbord

<img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/dashbord.png">

​       

常用操作：

​        将创建好的yaml文件通过Create按钮创建所需资源项目。

###           3.2  kubectl

##          

| 描述   | 语法                                       | 举例                                     |
| ---- | ---------------------------------------- | -------------------------------------- |
| 创建   | $ create -f FILENAME                     | kubectl create -f my.yaml              |
| 删除   | $ delete ([-f FILENAME] \| TYPE [(NAME \| -l label \| --all)]) | 删除所有pods：kubectl delete pods --all     |
| 修改   | $ edit (RESOURCE/NAME \| -f FILENAME)    | 修改service:  kubectl edit svc/myservice |
| 查询   | $ get [(-o\|--output=)json\|yaml\|wide\|custom-columns=...\|custom-columns-file=...\|go-template=...\|go-template-file=...\|jsonpath=...\|jsonpath-file=...] (TYPE [NAME \| -l label] \| TYPE/NAME ...) [flags] | 查询所有pod:  kubectl get pods             |



## 4. Grafana

​       Grafana是一个可视化面板，有着非常漂亮的图表和布局展示，功能齐全的度量仪表盘和图形编辑器，支持Graphite、Zabbix、InfluxDB、Elasticsearch、Prometheus和OpenTSDB作为数据源。

​       ***Grafana主要特性：***

- ​        灵活丰富的图形化选项
- ​        可以混合多种风格
- ​        支持白天和夜间模式
- ​        多个数据源

​    

​        ***Dashbord：***



​       <img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/Grafana.png">

​       可以通过Dashbord查看集群详情：cpu、memory、filesystemm、network等



## 5. Demo

​       接下面将通过一个demo来介绍如何部署一个项目。

​      一、业务场景

​             部署ITOO的基础项目（后端），部署之后将可以访问基础系统的swagger。

​      二、所需资料

​             1、yaml文件：

​                  basicInfo-deployment.yaml

​                  basicInfo-service.yaml

​              2、后端war包

​                    basicInfo-web.war

​                    basicInfo-service.war

​               3、镜像

​                    reg.dynamicharbor.com/web/othertomcat:2.0

​          三、详解 

​                  1、basicInfo-deployment.yaml

​                         用来将后端war包部署在tomcat中，部署为一个Deployment类型的资源。

<img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/deployment.png">



​                     将war包放在最下方的Path对应的路径（服务器的目录）下即可。



​                  2、basicInfo-service.yaml

​                      <img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/service.png">



​                     

​                     在Dashbord上点击右上角的+Create，选中 “basicInfo-deployment.yaml”文件， 此时Dashbord上就会出现如下资源：

​                     <img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/1.png">

​                       

​                在Dashbord上点击右上角的+Create，选中 “basicInfo-service.yaml”文件，  此时Dashbord上就会出现service如下：

​               <img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/2.png">



​               

​         

​               此时访问：IP：31116即可访问基础服务的swagger。



##**6**. 参考

kubenetes官网：

kubenetes中文社区：http://docs.kubernetes.org.cn/

宋净超：https://jimmysong.io/kubernetes-handbook/

kubenetes之yaml文件：https://blog.csdn.net/phantom_111/article/details/79427144

Grafana安装配置介绍：http://www.ywnds.com/?p=5903
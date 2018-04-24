*Docker入门文档**

# 一、Docker基本认识

## 1、什么是Docker？

1、什么是Docker？

&emsp;&emsp;Docker是全球领先的软件容器化平台。官方解释，docker是一个开源项目，可以作为轻量级容器来打包、部署、运行应用。Node.js解释，docker运行用标准单位及所有依赖打包一个应用。但是这些有些抽象，所以我们用现有技术来类比。

&emsp;&emsp;Docker 可以粗糙的理解为轻量级的虚拟机，但是Docker不是虚拟机。
<div align=center>
<img src="https://raw.githubusercontent.com/12wanghongwei/KubernetesLearning-RoadMap/master/images/Docker VS VM.png">
</div>

&emsp;&emsp;虚拟机VM上有Hypervisor（虚拟层，虚拟处理硬件），在硬件上安装独立的OS，然后再运行各种各样的应用程序。Docker利用Docker Engine运行应用程序，比虚拟机少了虚拟层，使得程序的启动速度，内存等小很多，所以更轻量。

## 2、为什么用Docker？

&emsp;&emsp;Docker平台是唯一的容器平台，用于构建，保护和管理从开发到生产的最广泛的应用程序。其优点有：

1. 敏捷
2. 可移植性
3. 安全
4. 节约成本  

&emsp;&emsp;Docker赋予开发者自由，创造力和更快运送更多软件的权力。从所有容器化应用和服务的通用控制平面获得可视性，管理和安全性。另外，Docker是一个开放和可扩展的平台，可轻松集成到现有环境中，并在全球范围内培育充满活力的技术生态系统。

## 3、Docker解决的问题？

&emsp;&emsp;传统的开发过程中，开发、测试、运维是三个独立运作的团队，团队之间沟通不畅，开发运维之间冲突时有发生，导致协作效率低下，产品交付延迟， 影响了企业的业务运行。

&emsp;&emsp;传统的开发过程中，开发、测试、运维是三个独立运作的团队，团队之间沟通不畅，开发运维之间冲突时有发生，导致协作效率低下，产品交付延迟， 影响了企业的业务运行。	

&emsp;&emsp;Docker技术将应用以集装箱的方式打包交付，使应用在不同的团队中共享，通过镜像的方式应用可以部署于任何环境中。以容器方式交付的Docker技术支持不断地开发迭代，大大提升了产品开发和交付速度。

## 4、架构

Docker采用C/S架构模式。

<div align=center>
<img src="https://raw.githubusercontent.com/12wanghongwei/KubernetesLearning-RoadMap/master/images/docker架构.png">
</div>

### 4.1Docker容器（Container）

容器是独立运行的一个或一组应用。

### 4.2Docker镜像(Images)

Docker 镜像是用于创建 Docker 容器的模板。



容器和镜像的关系类似于面向对象编程中的对象与类。

| Docker | 面向对象 |
| ------ | -------- |
| 容器   | 对象     |
| 镜像   | 类       |

### 4.3Dockerfile

&emsp;&emsp;创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

# 二、Docker安装与使用

## 1、Docker安装：


推荐使用在linux上安装Docker，如果没有环境可以尝试虚拟机安装：https://blog.csdn.net/hui_2016/article/details/68927487
以CentOS为例：https://www.w3cschool.cn/docker/centos-docker-install.html


## 2、Docker命令：

常用命令：https://www.w3cschool.cn/docker/docker-nx3g2gxn.html

## 3、docker实例：

以安装tomcat为例：

1.拉取tomcat镜像：

[root@localhost mysql]# docker pull tomcat

2.运行镜像：

[root@localhost tomcat]# docker run --name tomcat -p 8080:8080 -v $PWD/test:/usr/local/tomcat/webapps/test -d tomcat

3.查看容器启动情况：

[root@localhost tomcat]# docker ps

4.浏览器中输入：http://ip:8080/ （ip为虚拟机ip）

<div align=center>
<img src="https://raw.githubusercontent.com/12wanghongwei/KubernetesLearning-RoadMap/master/images/tomcat.png">
</div>                                                             

# 三、资源

Docker官网：<https://www.docker.com/>

Docker中文社区：<http://www.docker.org.cn/>

GitBook：https://yeasy.gitbooks.io/docker_practice/content/

Docker菜鸟教程，资源汇总：<http://www.runoob.com/docker/docker-resources.html>

Docker入门教程-慕课网：<https://www.imooc.com/learn/867>

 

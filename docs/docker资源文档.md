*Docker入门文档**

# 一、Docker基本认识

## 1、什么是Docker？

&nbsp;1、什么是Docker？

​	Docker是全球领先的软件容器化平台。官方解释，docker是一个开源项目，可以作为轻量级容器来打包、部署、运行应用。Node.js解释，docker运行用标准单位及所有依赖打包一个应用。但是这些有些抽象，所以我们用现有技术来类比。

​	Docker 可以粗糙的理解为轻量级的虚拟机，但是Docker不是虚拟机。

![Docker VS VM](C:\Users\王红伟\Desktop\Docker VS VM.png)

​	虚拟机VM上有Hypervisor（虚拟层，虚拟处理硬件），在硬件上安装独立的OS，然后再运行各种各样的应用程序。Docker利用Docker Engine运行应用程序，比虚拟机少了虚拟层，使得程序的启动速度，内存等小很多，所以更轻量。

## 2、为什么用Docker？

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Docker平台是唯一的容器平台，用于构建，保护和管理从开发到生产的最广泛的应用程序。其优点有：

1. 敏捷
2. 可移植性
3. 安全
4. 节约成本  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Docker赋予开发者自由，创造力和更快运送更多软件的权力。从所有容器化应用和服务的通用控制平面获得可视性，管理和安全性。另外，Docker是一个开放和可扩展的平台，可轻松集成到现有环境中，并在全球范围内培育充满活力的技术生态系统。

## 3、Docker解决的问题？

&nbsp;&nbsp;&nbsp;&nbsp;传统的开发过程中，开发、测试、运维是三个独立运作的团队，团队之间沟通不畅，开发运维之间冲突时有发生，导致协作效率低下，产品交付延迟， 影响了企业的业务运行。

​	传统的开发过程中，开发、测试、运维是三个独立运作的团队，团队之间沟通不畅，开发运维之间冲突时有发生，导致协作效率低下，产品交付延迟， 影响了企业的业务运行。	

​	Docker技术将应用以集装箱的方式打包交付，使应用在不同的团队中共享，通过镜像的方式应用可以部署于任何环境中。以容器方式交付的Docker技术支持不断地开发迭代，大大提升了产品开发和交付速度。

## 4、架构

Docker采用C/S架构模式。

![docker架构](C:\Users\王红伟\Desktop\docker架构.png)

### 4.1Docker容器（Container）

​	容器是独立运行的一个或一组应用。

### 4.2Docker镜像(Images)

​	Docker 镜像是用于创建 Docker 容器的模板。



容器和镜像的关系类似于面向对象编程中的对象与类。

| Docker | 面向对象 |
| ------ | -------- |
| 容器   | 对象     |
| 镜像   | 类       |

### 4.3Dockerfile

​	创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

# 二、Docker安装与使用

## 1、Docker安装：

以CentOS为例：

使用yum安装已实践）：https://www.w3cschool.cn/docker/centos-docker-install.html

可能遇到的问题：

重启docker服务可能出现问题：'overlay'
not found as a supported filesystem on thi...t loaded.

解决方法参考：

https://blog.csdn.net/hongwei15732623364/article/details/80069068



docker  run 出现问题：TLS handshake timeout

解决方法：

centos7 ，或者说 redhat 的 docker 的配置文件和其他发行版都不一样。 
按照阿里和 daoCloud 的手册很难顺利配置好镜像。 

两种方案：

第一种：为了永久性保留更改，您可以修改 `/etc/docker/daemon.json` 文件并添加上 registry-mirrors 键值。

```
{
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

修改保存后重启 Docker 以使配置生效。

另外，https://registry.docker-cn.com可以改成自己注册后获取的专有地址，需要注册云服务具体参考“设置Docker加速”）

第二种：证书

执行以下命令：

mkdir -p certs && openssl req \
  -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key \
  -x509 -days 365 -out certs/domain.crt

然后依次输入生成的证书的信息即可！

## 2、设置Docker加速：

注册登录[阿里云](https://cr.console.aliyun.com)

找到镜像加速器按照命令依次执行即可：

![docker加速](C:\Users\王红伟\Desktop\docker加速.png)

## 3、Docker命令：

常用命令：https://www.w3cschool.cn/docker/docker-nx3g2gxn.html

## 3、Docker实例：

以安装tomcat为例：

1.拉取tomcat镜像：

[root@localhost mysql]# docker pull tomcat

2.运行镜像：

[root@localhost tomcat]# docker run --name tomcat -p 8080:8080 -v $PWD/test:/usr/local/tomcat/webapps/test -d tomcat

3.查看容器启动情况：

[root@localhost tomcat]# docker ps

4.浏览器中输入：http://ip:8080/ （ip为虚拟机ip）

![tomcat](C:\Users\王红伟\Desktop\tomcat.png)                                                               

# 三、资源

Docker官网：<https://www.docker.com/>

Docker中文社区：<http://www.docker.org.cn/>

GitBook：https://yeasy.gitbooks.io/docker_practice/content/

Docker菜鸟教程，资源汇总：<http://www.runoob.com/docker/docker-resources.html>

Docker入门教程-慕课网：<https://www.imooc.com/learn/867>

 

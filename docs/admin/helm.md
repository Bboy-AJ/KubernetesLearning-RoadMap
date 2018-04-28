# helm

<div align=center><img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/helm1.png"></div>

helm作为Kubernetes一个包管理引擎，基于chart的概念，有效的对Kubernetes上应用的部署进行了优化。Chart通过模板引擎，下方对接Kubernetes中services模型，上端打造包管理仓库。最后的使得Kubernetes中，对应用的部署能够达到像使用apt-get和yum一样简单易用。

helm把kubenetes的资源（比如deployment、services或者ingress等）打包到一个chart中，而chart被保存到chart仓库中。通过chart仓库可用来保存和分享chart。helm使发布可配置，支持发布应用配置的版本管理，简化了kubenetes部署应用的版本控制、打包、发布、删除、更新等操作。

# 基本概念

1. Chart：是Helm管理的安装包，里面包含需要部署的安装包资源。可以把Chart比作CentOS yum使用的rpm文件。

​            包括：

- 包的基本描述文件Chart.yaml
- 放在templates目录中的一个或多个Kubernetes manifest文件模板

2. Release：是chart的部署实例，一个chart在一个Kubernetes集群上可以有多个release，即这个chart可以被安装多次。
3. Repository：chart的仓库，用于发布和存储chart

# helm安装

<div align=center><img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/helm5.png"></div>

- 客户端helm：helm是一个命令行工具，可在本地运行，一般运行在CI/CD Server上。

- 服务端tiller：tiller运行在Kubernetes集群上，管理chart安装的release。

  ​



**client安装：**

​     wget https://storage.googleapis.com/kubernetes-helm/helm-v2.6.1-linux-amd64.tar.gz

​    tar -zxvf helm-v2.6.1-linux-amd64.tgz && mv linux-amd64/helm /usr/local/bin/helm



**server安装：**

​       helm init --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.5.1 --stable-repo-url https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts



**具体参考**：https://blog.csdn.net/weiguang1017/article/details/78045013



#helm创建使用

- 方式一：创建新的char

​              1、创建

```
helm create 名字
```

<div align=center><img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/helm2.png"></div>

​              2、产生helm13（我命名的），新的chart，里面包含如下几项：

​                 <div align=center><img src="https://github.com/Bboy-AJ/KubernetesLearning-RoadMap/blob/master/images/helm3.png"></div>

| 文件                  | 说明                                       |
| :------------------ | ---------------------------------------- |
| Chart.yaml          | 描述Chart的基本信息，如名称版本等                      |
| templates           | Kubernetes  manifest文件模板目录，模板使用chart配置的值生成Kubernetes manifest文 件。 |
| templates/NOTES.txt | 可在其中填写chart的使用说明                         |
| value.yaml          | chart配置的默认值                              |
| charts              | charts目录中是本chart依赖的chart，当前是空的           |

​               3、安装char

```
 helm install ./
```

​               4、打包

```
helm package ./
```



- 方式二：利用chart库已有的进行创建

​                    1、搜索你需要的chart      

```
helm search jenkins
```

​                     2、安装

```
helm install --name my-release --set Persistence.StorageClass=slow stable/jenkins
```

-  方式三： 利用其他已有资源

  ​

  1、指定打包好的文件：

  ```
  helm install ./nginx-1.2.3.tgz
  ```

   2、指定打包目录：

  ```
  helm install ./nginx
  ```

  3、指定chart包URL

  ```
  helm install https://example.com/charts/nginx-1.2.3.tgz
  ```



​       参考链接：https://www.kubernetes.org.cn/3435.html

​       其他方式可自行探索。

# 自定义helm

​           

| 说明                  | 命令                                       |
| ------------------- | ---------------------------------------- |
| 通过--set key=value方式 | helm install --set name=prod ./redis  例如：helm install -n  mysql -f mysql/values.yaml --set resources.requests.memory=512Mi mysql |
| 覆盖chart中默认值         | helm install -f  myvalues.yaml ./redis   |

 其他参考链接：https://docs.bitnami.com/kubernetes/how-to/create-your-first-helm-chart/

​                         

# 其他命令

| 功能             | 命令                                       |
| -------------- | ---------------------------------------- |
| 查看部署           | helm get my-release                      |
| 查看状态           | helm status  my-release                  |
| 查看全部的release   | helm list -a                             |
| 更新版本           | helm upgrade  my-release -f mysql/values.yaml --set resources.requests.memory=1024Mi  my-release |
| 版本回滚           | helm rollback mysql  1                   |
| 查看release的版本信息 | helm hist  my-release                    |
| 删除release      | helm delete  my-release                  |
| 彻底删除           | helm delette  --purge my-release         |
| 查看是否已经删除       | helm ls -a  myrelease                    |

参考官网：https://docs.helm.sh/using_helm/


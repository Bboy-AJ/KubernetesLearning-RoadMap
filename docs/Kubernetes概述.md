# 前言 

&emsp;&emsp;随着容器技术的发展，Docker近几年突然崛起，变得炙手可热，已经成为容器技术的事实标准。然而想要在生成环境中成功部署和操作容器的关键是容器编排技术，市场上有各种各样的容器编排工具（如Docker原生的Swarm），其中谷歌公司开发的Kubernetes得到开源社区的全力支持，IBM、惠普、微软、RedHat等业界巨头纷纷加入，Kubernetes已经成为GitHub上的明星开源项目。

# 简介

&emsp;&emsp;Kubernetes这个名字源于希腊语，是舵手的意思，所以它的Logo既像一张渔网，又像一个罗盘。有意思的是Docker的Logo为驮着集装箱在大海上遨游的鲸鱼，Kubernetes与Docker的关系可见一斑。 

<div align=center><img src="https://raw.githubusercontent.com/Bboy-AJ/KubernetesLearning-RoadMap/master/images/k8s&docker.png"></div>

&emsp;&emsp;Kubernetes（也常称K8s，用8代替8个字符“ubernete”而成的缩写。）是一个全新的基于容器技术的分布式架构方案，它源自Google内部大规模集群管理系统——Borg，也是CNCF（Cloud Native Computing Foundation，今属Linux基金会）最重要的项目之一，旨在让部署容器化的应用简单并且高效。 


&emsp;&emsp;Kubernetes 具备完善的集群管理能力，包括多层次的安全防护和准入机制、多租户应用支撑能力、透明的服务注册和服务发现机制、内建负载均衡器、故障发现和自我修复能力、服务滚动升级和在线扩容、可扩展的资源自动调度机制、多粒度的资源配额管理能力。还提供完善的管理工具，涵盖开发、部署测试、运维监控等各个环节。

<div align=center><img src="https://raw.githubusercontent.com/Bboy-AJ/KubernetesLearning-RoadMap/master/images/SearchInterest.png" width=650></div>
<br>
&emsp;&emsp;Kubernetes 于2015年7月22日迭代到 v 1.0并正式对外公布，截至2018年3月28日稳定版已经发布到 v.1.10.0。无论是英文还是中文社区都非常活跃，全球开源解决方案领导者 RedHat 公司已经将自己Paas产品OpenShift V3 的底层技术换成了Kubernetes与Docker。Kubernetes俨然已经成为全新容器生态的领导者。

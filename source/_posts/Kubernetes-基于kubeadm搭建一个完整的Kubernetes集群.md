title: '[Kubernetes] 基于kubeadm搭建一个完整的Kubernetes集群'
tags:
  - kubeadm
  - k8s 
category: Kubernetes
date: 2020-09-29 09:42:25
---
## Overview
Kubernetes 是一个可移植的、可扩展的开源平台，用于管理容器化的工作负载和服务，可促进声明式配置和自动化。Kubernetes 拥有一个庞大且快速增长的生态系统。Kubernetes 是 Google 于 2014 年 6 月基于其内部使用的 Borg 系统开源出来的容器编排调度引擎，Google 将其作为初始和核心项目贡献给 CNCF（云原生计算基金会），近年来逐渐发展出了云原生生态。

Kubernetes 的目标不仅仅是一个编排系统，而是提供一个规范用以描述集群的架构，定义服务的最终状态，使系统自动地达到和维持该状态。Kubernetes 作为云原生应用的基石，相当于一个云操作系统，其重要性不言而喻。
<!--more-->

### 为什么需要 Kubernetes，它能做什么?
容器是打包和运行应用程序的好方式。在生产环境中，您需要管理运行应用程序的容器，并确保不会停机。例如，如果一个容器发生故障，则需要启动另一个容器。如果系统处理此行为，会不会更容易？

这就是 Kubernetes 的救援方法！Kubernetes 为您提供了一个可弹性运行分布式系统的框架。Kubernetes 会满足您的扩展要求、故障转移、部署模式等。例如，Kubernetes 可以轻松管理系统的 Canary 部署。

Kubernetes 能够提供以下一些功能：
* 服务发现和负载均衡  
Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器，如果到容器的流量很大，	Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定。
* 存储编排  
Kubernetes 允许您自动挂载您选择的存储系统，例如本地存储、公共云提供商等。
* 自动部署和回滚  
您可以使用 Kubernetes 描述已部署容器的所需状态，它可以以受控的速率将实际状态更改为所需状态。例如，您可以自动化 Kubernetes 来为您的部署创建新容器，删除现有容器并将它们的所有资源用于新容器。
* 自动二进制打包  
Kubernetes 允许您指定每个容器所需 CPU 和内存（RAM）。当容器指定了资源请求时，Kubernetes 可以做出更好的决策来管理容器的资源。
* 自我修复  
Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的运行状况检查的容器，并且在准备好服务之前不将其通告给客户端。
* 密钥与配置管理  
Kubernetes 允许您存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。您可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥。

因此综上，我们需要好好学习 Kubernetes 这个强大的云操作系统，增强自身的竞争力。工欲善其事，必先利其器，本篇也是 Kubernetes 系列的第一篇博文，讲述如何利用 Kubeadm 这个工具来搭建一套供自己学习的 Kubernetes 集群。

## Kubeadm 到底是什么？
Kubeadm 能够用以创建一个符合最佳实践的最小化 Kubernetes 集群。事实上，你可以使用 kubeadm 配置一个通过 Kubernetes 一致性测试的集群。 kubeadm 还支持其他集群生命周期功能， 例如：启动引导令牌和集群升级。

kubeadm 工具很棒，如果你需要：

* 一个尝试 Kubernetes 的简单方法。
* 一个现有用户可以自动设置集群并测试其应用程序的途径。
* 其他具有更大范围的生态系统和/或安装工具中的构建模块。

你可以在各种机器上安装和使用 kubeadm：笔记本电脑， 一组云服务器，Raspberry Pi 等。无论是部署到云还是本地，你都可以将 kubeadm 集成到预配置系统中，例如 Ansible 或 Terraform。
## 准备工作
* 一台或多台运行着下列系统的机器，去阿里云申请或者自己物理机用 VMvare 新建都行：
	- Ubuntu 16.04+
	- Debian 9+
	- CentOS 7
	- Red Hat Enterprise Linux (RHEL) 7
	- Fedora 25+
	- HypriotOS v1.0.1+
	- Container Linux (测试 1800.6.0 版本)
* 单机可用资源建议 2 核 CPU、8 GB 内存或以上，再小的话问题也不大，但是能调度的 Pod 数量就比较有限了
* 每台机器能够访问外网，因为需要拉取镜像
* 集群中所有计算机之间具有完全的网络连接

## 安装 Kubeadm 以及 Docker
上述介绍过 kubeadm 的基础用法，接下来我将会介绍基于 Centos 8 安装 Kubernetes 组件以及 Docker。

### 配置安装源
* 配置`docker`
```
$ wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo
```
* 配置`kubernetes`
```
$ cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```
* 本地缓存
```
$ dnf makecache
```

### 安装 Kubeadm 以及 docker
* 执行以下命令：
```
$ dnf -y install docker-ce
$ systemctl enable docker
$ systemctl start docker
$ dnf install -y kubelet-1.18.0 kubeadm-1.18.0 kubectl-1.18.0
```

这里安装了 Docker 公司发布的最新的 Docker CE（社区版），Kubernetes 相关的组件安装的 1.18.0 版本，基本是目前为止最新的版本了。


## 部署 Kubernetes 的 Master 节点
利用 kubeadm 可以很简单的来初始化 kubernetes 集群的 master 节点，执行以下命令：
```
kubeadm init \
  --apiserver-advertise-address=192.168.0.2 \
  --kubernetes-version v1.18.0 \
  --service-cidr=10.1.0.0/16 \
  --pod-network-cidr=10.244.0.0/16
  
 If you can't access foreign websites:
  kubeadm init \
  --apiserver-advertise-address=192.168.0.2 \
  --image-repository registry.aliyuncs.com/google_containers \
  --kubernetes-version v1.18.0 \
  --service-cidr=10.1.0.0/16 \
  --pod-network-cidr=10.244.0.0/16
```
上述命令行参数解释：
* `--apiserver-advertise-address`：可用于为控制平面节点的 API server 设置广播地址，


## 部署网络插件
在 Kubernetes 项目“一切皆容器”的设计理念指导下，部署网络插件也是通过启动`pod`的形式来配置网络，其中有两种网络插件可供部署：
* 部署 flannel 网络插件（详细介绍后面会在开一篇博文）：
```
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml
update:  https://raw.githubusercontent.com/coreos/flannel/v0.12.0/Documentation/kube-flannel.yml
```
* 部署 calico 网络插件：
```
$ kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
```
上述两个网络插件只需部署一个即可，部署成功与否可通过查看对应 pod 的运行状态即可：
```
$ kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-65f8bc95db-ngv7v   1/1     Running   0          3d19h
calico-node-9sr5c                          1/1     Running   0          3d19h
calico-node-jtt5w                          1/1     Running   0          3d19h
coredns-7ff77c879f-v645l                   1/1     Running   0          3d19h
coredns-7ff77c879f-vdrcf                   1/1     Running   0          3d19h
etcd-master                                1/1     Running   1          3d20h
kube-apiserver-master                      1/1     Running   1          3d20h
kube-controller-manager-master             1/1     Running   1          3d20h
kube-proxy-sbxzb                           1/1     Running   0          3d19h
kube-proxy-xfw7t                           1/1     Running   0          3d19h
kube-scheduler-master                      1/1     Running   1          3d20h
```

可以看到，所有的系统 Pod 都成功启动了，而刚刚部署的 calico 网络插件则在 kube-system 下面新建了一个名叫 calico-kube-controllers-65f8bc95db-ngv7v 的 Pod，一般来说，这些 Pod 就是容器网络插件在每个节点上的控制组件。

Kubernetes 支持容器网络插件，使用的是一个名叫 CNI 的通用接口，它也是当前容器网络的事实标准，市面上的所有容器网络开源项目都可以通过 CNI 接入 Kubernetes，比如 Flannel、Calico、Canal、Romana 等等，它们的部署方式也都是类似的“一键部署”。关于这些开源项目的实现细节和差异，后续会有相关的博文进行详细讲解。

至此，Kubernetes 的 Master 节点就部署完成了。如果你只需要一个单节点的 Kubernetes，现在你就可以使用了。不过，在默认情况下，Kubernetes 的 Master 节点是不能运行用户 Pod 的，所以还需要额外做一个小操作。在本篇的最后部分，我会介绍到它。

## 部署 Kubernetes 的计算节点
Kubernetes 的 计算节点跟 Master 节点几乎是相同的，它们运行着的都是一个 kubelet 组件。唯一的区别在于，在 kubeadm init 的过程中，kubelet 启动后，Master 节点上还会自动运行 kube-apiserver、kube-scheduler、kube-controller-manger 这三个系统 Pod。
执行以下命令：
```
$ kubeadm join 192.168.56.12:6443 --token japatq.5vib0jhpgmeeqsb2 \
    --discovery-token-ca-cert-hash sha256:c08f2729dbe9e2b1ce9f44e6d3159c493cc686b2e93dc252f7658cb26b87d726
```
用以加入`192.168.56.12`这个 IP 的计算节点到集群中，以上`Token`可用`kubeclt token list`得到。

通过执行以上步骤便可以搭建好一个网络完整的 Kubernetes 集群。当然，也可以继续往集群中添加其他的插件，比如部署 Dashboard 可视化插件，部署容器存储插件等等。这个就因人而异了，留给大家自由发挥了...

## 测试集群
* 列出集群的节点
```
[root@master ~]# kubectl get nodes
NAME     STATUS   ROLES    AGE   VERSION
master   Ready    master   33d   v1.18.0
node     Ready    <none>   33d   v1.18.0
```
* 列出系统的`pod`资源
```
[root@master ~]# kubectl get pod -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-65f8bc95db-ngv7v   1/1     Running   0          33d
calico-node-9sr5c                          1/1     Running   0          33d
calico-node-jtt5w                          1/1     Running   0          33d
coredns-7ff77c879f-v645l                   1/1     Running   0          33d
coredns-7ff77c879f-vdrcf                   1/1     Running   0          33d
etcd-master                                1/1     Running   1          33d
kube-apiserver-master                      1/1     Running   1          33d
kube-controller-manager-master             1/1     Running   1          33d
kube-proxy-sbxzb                           1/1     Running   0          33d
kube-proxy-xfw7t                           1/1     Running   0          33d
kube-scheduler-master                      1/1     Running   1          33d
```

## 参考
- [使用 kubeadm 创建集群](https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)
- [Kubernetes概述](https://kubernetes.io/zh/docs/concepts/overview/)
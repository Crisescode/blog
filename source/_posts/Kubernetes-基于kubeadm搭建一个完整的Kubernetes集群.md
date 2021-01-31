title: '[Kubernetes] 基于kubeadm搭建一个完整的Kubernetes集群'
tags:
  - kubeadm
  - k8s集群
category: Kubernetes
date: 2020-09-29 09:42:25
---
## Overview


## Kubeadm 到底是什么？


## 环境介绍

<!--more-->
## 安装准备


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

##
title: '[Kubernetes] 认识一下Kubernetes'
date: 2020-10-29 22:20:45
tags:
  - k8s
category: Kubernetes
---
## 有哪些组件
Kubernetes 集群分为两个部分：
* Kubernetes 控制平面
* （工作）节点

其中控制平面的组件有：
* Etcd 分布式持久化存储
* API Server
* Kube Scheduler
* Kube Controller

运行容器任务依赖于每个工作节点上运行的组件，包括：
* Kubelet
* Kubelet 服务代理（kube proxy）
* 容器运行时（docker，rkt，containerd或者其他）

<!--more>

除了控制平面以及工作节点的组件，还有几个附件组件，如下：
* Kubernetes DNS 服务器
* 仪表板
* Igress 控制器
* Heapster（容器集群监控）
* 容器网络接口插件

### 组件如何通信
Kubernetes 系统组件间只能通过 API Server 通信，他们之间不会直接通信。API Server 是和 etcd 通信的唯一组件，其他组件都不会直接和 etcd 通信，而是通过 API Server 来修改集群的状态。


### 检查控制平面组件的状态
API Server 对外暴露了一个名为 ComponentStatus 的 API 资源，用来显示每个控制平面组件的健康状态。可以通过 kubectl 列出各个组件以及它们的状态：
```
[root@bigdatafat041120 ~]# kubectl get componentstatuses
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE             ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-1               Healthy   {"health":"true"}
etcd-2               Healthy   {"health":"true"}
etcd-0               Healthy   {"health":"true"}

```
## Etcd 组件

## API Server 组件
`Kubernetes API Server` 作为中心组件，其他组件或者客户端（如 `kubectl`）都回去调用它。其以`RESTful API` 的形式提供了可以查询，修改集群状态的 `CURD（Create，Read，Update，Detele）`接口，然后将状态储存到 `etcd` 中。

`API Server` 除了提供一种一致的方式将对象存储到 `etcd`, 也对这些对象做校验，
这样客户端就无法存入非法的对象了（直接写入存储的话是有可能的）。除了校验，
还会处理乐观锁， 这样对于并发更新的情况， 对对象做更改就不会被其他客户端覆盖。

### API Server 如何创建一个资源
`API Server` 的客户端之一就是刚开始介绍的命令行工作 `kubectl`。当以 `yaml` 文件创建一个资源，`kubectl` 通过一个 `HTTP POST` 请求将文件内容发布到 `API Server`。

* 通过认证插件认证客户端
* 通过授权插件授权客户端
* 通过准入控制插件验证 `AND/OR` 修改资源请求   
如果请求尝试创建，修改或者删除从一个资源，请求需要经过准入控制插件的验证。服务器会配置多个准入控制插件，这些插件会因为各种原因修改资源，可能会初始化资源定义中漏配的字段为默认值，甚至重写他们。插件甚至还会去修改并不在请求中的相关资源，同时也会因为某些原因拒绝一个请求。   
准入控制插件包括：
    * AlwayPullImages —— 重写 pod 的 imagePullPolicy 为 Always，强制每次部署 pod 时拉取镜像；
    * ServiceAccount —— 未明确定义服务账户的使用默认账户；
    * NamespaceLifecycle —— 防止在命名空间中创建正在呗删除的 pod，或者在不存在的命令空间中创建 pod；
    * ResourceQuota —— 保证特定命名空间中的 pod 只能使用该命名空间分配数量的资源，如 CPU 和内存。
    
* 验证资源以及持久化存储   
请求通过了所有的准入控制插件后，API Server 会验证存储到 etcd 的对象，然后返回一个响应给客户端。

## Kube Scheduler 组件
### 默认的调度算法
选择节点进行调度，可以分解为两部分，如下：
* 过滤所有节点，找出能分配 pod 的可用节点列表；
* 对可用节点按优先级排序，找出最优节点，如果多个节点都有最高的优先级分数，则循环分配，确保平均分配的 pods。

### 使用多个调用器
可以在集群中运行多个调度器而非单个，然后，对每一个 `pod`，可以通过在 `pod` 特性中设置 `schedulerName` 属性指定调度器来调度特定的 pod。   
未设置该属性的 `pod` 由默认调度器调度， 因此其`schedulerName` 被设置为
`default-scheduler` 。其他设置了该属性的 `pod` 会被默认调度器忽略掉， 它们要
么是手动调用， 要么被监听这类 `pod` 的调度器调用。   
可以实现自己的调度器， 部署到集群， 或者可以部署有不同配置项的额外
`Kubernetes` 调度器实例。

## Kube-Controller 控制器组件
如前面提到的， `API Server` 只做了存储资源到 `etcd` 和通知客户端有变更的工作。
调度器则只是给 `pod` 分配节点， 所以需要有活跃的组件确保系统真实状态朝 `API Server` 定义的期望的状态收敛。这个工作由控制器管理器里的控制器来实现。   
单个控制器、管理器进程当前组合了多个执行不同非冲突任务的控制器。这些
控制器最终会被分解到不同的进程， 如果需要的话， 我们能够用自定义实现替换它
们每一个。其中控制器包括:
* Replication 管理器（ReplicationController 资源管理器）
* ReplicaSet、DaemonSet 以及 Job Controller
* Deployment Controller
* StatefulSet Controller
* Node Controller
* Service Controller
* Endpoints Controller
* Namespace Controller
* PersistentVolume Controller
* 其他

控制器执行一个“调和”循环，将实际状态调整为期望状态（在资源spec部分定义），然后将新的实际状态写入资源的 status 部分。控制器利用监听机制来订阅变更，但是由于使用监听机制并不保证控制器不会漏掉时间，所以仍需要定期执行重列举操作来确保不会丢掉什么。

## Worker 节点的组件介绍
### Kubelet 组件

### Kubernetes Service Proxy 组件
除了有 `Kubelet`，每个工作节点还会运行 `kube-proxy`组件，用于确保客户端可以用过 `Kubernetes API` 连接到你定义的服务。`kube-proxy` 确保对服务 `IP` 和端口的连接最终
能到达支持服务（或者其他，非 `pod` 服务终端）的某个 `pod` 处。如果有多个 `pod` 支
撑一个服务，那么代理会发挥对 `pod` 的负载均衡作用。

## 参考
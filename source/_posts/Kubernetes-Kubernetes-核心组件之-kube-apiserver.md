---
title: '[Kubernetes] Kubernetes 核心组件之 kube-apiserver'
tags:
  - k8s
  - api-server
index_img: /img/1.jpg
banner_img: /img/1.jpg
date: 2021-11-15 22:36:19
category: Kubernetes
---
`kube-apiserver` 是 `Kubernetes` 最重要的核心组件之一，主要能提供的功能如下：
* 给集群其他组件提供数据交互以及通信枢纽功能，以`RESTful API` 的形式提供其他组件查询，修改集群状态的 `CURD（Create，Read，Update，Detele）`接口，然后将状态储存到 `etcd` 中;
* 为用户请求接入认证，授权以及准入控制等功能；
* 并且可以用来处理乐观锁， 当有并发更新的情况， 对对象做更改就不会被其他客户端覆盖。
<!--more-->

### 如何部署


### 工作原理
`kube-apiserver` 提供了 `Kubernetes` 的 `REST API`，实现了认证、授权、准入控制等安全校验功能，同时也负责集群状态的存储操作（通过 `etcd`）。
#### 认证
#### 授权
#### 准入控制



### API 访问示例
`kube-apiserver` 提供了为每种语言提供了 `SDK`，方便用户访问集群其他组件，以及将数据存储或更新到`etcd`中，如下：
* [GO Client](https://github.com/kubernetes/client-go)
* [Python Client](https://github.com/kubernetes-client/python)
* [JavaScipt Client](https://github.com/kubernetes-client/javascript)
* [Java Client](https://github.com/kubernetes-client/java)
* [CSharp Client](https://github.com/kubernetes-client/csharp)
* 其他[OpenAPI](https://www.openapis.org/)支持的语言，可以通过[gen](https://github.com/kubernetes-client/gen)工具生成相应的 `Client`.

#### kubectl 访问
* 查询 `kube-apiserver` 支持`API`的版本
```
$ kubectl api-versions
admissionregistration.k8s.io/v1
admissionregistration.k8s.io/v1beta1
apiextensions.k8s.io/v1
apiextensions.k8s.io/v1beta1
apps/v1

...

scheduling.k8s.io/v1
scheduling.k8s.io/v1beta1
storage.k8s.io/v1
storage.k8s.io/v1beta1
traefik.containo.us/v1alpha1
v1
xgboostjob.kubeflow.org/v1

```
* 查询 `kube-apiserver` 支持`API`的资源对象
```
$ kubectl api-resources
NAME                              SHORTNAMES        APIGROUP                       NAMESPACED   KIND
bindings                                                                           true         Binding
componentstatuses                 cs                                               false        ComponentStatus
configmaps                        cm                                               true         ConfigMap

... 

tlsoptions                                          traefik.containo.us            true         TLSOption
tlsstores                                           traefik.containo.us            true         TLSStore
traefikservices                                     traefik.containo.us            true         TraefikService
xgboostjobs                                         xgboostjob.kubeflow.org        true         XGBoostJob

```
* 查看 `kube-apiserver` 的 `v1/namepsace` 信息

  ```
  $ kubectl get --raw /api/v1/namespaces
  {"kind":"NamespaceList","apiVersion":"v1","metadata":{"selfLink":"/api/v1/namespaces","resourceVersion":"190115288"},"items":[{"metadata":{"name":"airflow","selfLink":"/api/v1/namespaces/airflow","uid":"f4756c7a-8bd7-4988-a69c-c849a3c035be","resourceVersion":"79499605","creationTimestamp":"2021-05-06T07:39:19Z","managedFields":[{"manager":"kubectl-create","operation":"Update","apiVersion":"v1","time":"2021-05-06T07:39:19Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"crise","selfLink":"/api/v1/namespaces/crise","uid":"6f943c7a-8419-4c95-bf5a-2972f3631aab","resourceVersion":"155793497","creationTimestamp":"2021-09-26T05:09:22Z","managedFields":[{"manager":"kubectl-create","operation":"Update","apiVersion":"v1","time":"2021-09-26T05:09:22Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"default","selfLink":"/api/v1/namespaces/default","uid":"545db544-c735-4d06-a600-981144f245a8","resourceVersion":"157","creationTimestamp":"2020-11-12T07:25:33Z","managedFields":[{"manager":"kube-apiserver","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:25:33Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"elastic","selfLink":"/api/v1/namespaces/elastic","uid":"adcddefd-2a71-4b65-af74-ec4d5ac854b4","resourceVersion":"132059434","creationTimestamp":"2021-08-10T08:36:48Z","labels":{"name":"elastic"},"managedFields":[{"manager":"Go-http-client","operation":"Update","apiVersion":"v1","time":"2021-08-10T08:36:48Z","fieldsType":"FieldsV1","fieldsV1":{"f:metadata":{"f:labels":{".":{},"f:name":{}}},"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"kafka","selfLink":"/api/v1/namespaces/kafka","uid":"82f78a4a-b135-49af-a6d2-34b162deff34","resourceVersion":"126083706","creationTimestamp":"2021-07-30T06:28:47Z","labels":{"name":"kafka"},"managedFields":[{"manager":"Go-http-client","operation":"Update","apiVersion":"v1","time":"2021-07-30T06:28:47Z","fieldsType":"FieldsV1","fieldsV1":{"f:metadata":{"f:labels":{".":{},"f:name":{}}},"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"kube-node-lease","selfLink":"/api/v1/namespaces/kube-node-lease","uid":"16073100-6a4c-4275-9cb4-914d16e92dd9","resourceVersion":"43","creationTimestamp":"2020-11-12T07:25:32Z","managedFields":[{"manager":"kube-apiserver","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:25:32Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"kube-public","selfLink":"/api/v1/namespaces/kube-public","uid":"f6077460-b5ab-4250-ad00-bd7a32745873","resourceVersion":"41","creationTimestamp":"2020-11-12T07:25:32Z","managedFields":[{"manager":"kube-apiserver","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:25:32Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"kube-system","selfLink":"/api/v1/namespaces/kube-system","uid":"4671dc1b-9f93-4c23-be43-65579ff03293","resourceVersion":"1051","creationTimestamp":"2020-11-12T07:25:32Z","annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"kind\":\"Namespace\",\"metadata\":{\"annotations\":{},\"name\":\"kube-system\"}}\n"},"managedFields":[{"manager":"kube-apiserver","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:25:32Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}},{"manager":"kubectl-client-side-apply","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:29:44Z","fieldsType":"FieldsV1","fieldsV1":{"f:metadata":{"f:annotations":{".":{},"f:kubectl.kubernetes.io/last-applied-configuration":{}}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"kubeflow-operators","selfLink":"/api/v1/namespaces/kubeflow-operators","uid":"7a506123-310a-48b7-8c4c-ea95238ed559","resourceVersion":"31999589","creationTimestamp":"2021-01-28T09:08:10Z","managedFields":[{"manager":"kubectl-create","operation":"Update","apiVersion":"v1","time":"2021-01-28T09:08:10Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"monitoring","selfLink":"/api/v1/namespaces/monitoring","uid":"28f94d9b-781a-4503-8704-05be7fc0b03f","resourceVersion":"1932","creationTimestamp":"2020-11-12T07:34:49Z","annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"kind\":\"Namespace\",\"metadata\":{\"annotations\":{},\"name\":\"monitoring\"}}\n"},"managedFields":[{"manager":"kubectl-client-side-apply","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:34:49Z","fieldsType":"FieldsV1","fieldsV1":{"f:metadata":{"f:annotations":{".":{},"f:kubectl.kubernetes.io/last-applied-configuration":{}}},"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"portainer","selfLink":"/api/v1/namespaces/portainer","uid":"65ce6ed6-be89-4da5-86da-8a5decf8b190","resourceVersion":"45415995","creationTimestamp":"2021-02-25T08:26:29Z","annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"kind\":\"Namespace\",\"metadata\":{\"annotations\":{},\"name\":\"portainer\"}}\n"},"managedFields":[{"manager":"kubectl-client-side-apply","operation":"Update","apiVersion":"v1","time":"2021-02-25T08:26:29Z","fieldsType":"FieldsV1","fieldsV1":{"f:metadata":{"f:annotations":{".":{},"f:kubectl.kubernetes.io/last-applied-configuration":{}}},"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"rook-ceph","selfLink":"/api/v1/namespaces/rook-ceph","uid":"e4756739-9b75-43ac-8870-60191e336917","resourceVersion":"1716232","creationTimestamp":"2020-11-16T07:37:55Z","managedFields":[{"manager":"kubectl-create","operation":"Update","apiVersion":"v1","time":"2020-11-16T07:37:55Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}},{"metadata":{"name":"traefik-system","selfLink":"/api/v1/namespaces/traefik-system","uid":"ed85f7ef-2805-4ba7-84d1-5ae430471202","resourceVersion":"1381","creationTimestamp":"2020-11-12T07:31:22Z","managedFields":[{"manager":"kubectl-create","operation":"Update","apiVersion":"v1","time":"2020-11-12T07:31:22Z","fieldsType":"FieldsV1","fieldsV1":{"f:status":{"f:phase":{}}}}]},"spec":{"finalizers":["kubernetes"]},"status":{"phase":"Active"}}]}

  ```

* 通过启动 `proxy` 获取集群配置信息
```
$ kubectl proxy --port=8081 & curl http://localhost:8081/api/
{
  "kind": "APIVersions",
  "versions": [
    "v1"
  ],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "10.114.1.100:6443"
    }
  ]
}
```

* 查看集群各组件信息
```
$ kubectl cluster-info
Kubernetes master is running at https://10.114.1.100:6443
CoreDNS is running at https://10.114.1.100:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
kubernetes-dashboard is running at https://10.114.1.100:6443/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy
Metrics-server is running at https://10.114.1.100:6443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

```

#### curl 访问
* 在 pod 里面访问
```
$ TOKEN=$(cat /run/secrets/kubernetes.io/serviceaccount/token)
$ CACERT=/run/secrets/kubernetes.io/serviceaccount/ca.crt
$ curl --cacert $CACERT --header "Authorization: Bearer $TOKEN"  https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/api
{
  "kind": "APIVersions",
  "versions": [
    "v1"
  ],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "10.114.1.110:6443"(另外一个master节点ip)
    }
  ]
}
```

* 在 master 节点上访问
```
$ APISERVER=$(kubectl config view | grep server | cut -f 2- -d ":" | tr -d " ")
$ TOKEN=$(kubectl describe secret $(kubectl get secrets | grep default | cut -f1 -d ' ') | grep -E '^token'| cut -f2 -d':'| tr -d '\t')
$ curl $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {

  },
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}
```
### 参考
* [kube-apiserver](https://kubernetes.feisky.xyz/concepts/components/apiserver)
* [kubernetes 组件](https://kubernetes.io/zh/docs/concepts/overview/components/)
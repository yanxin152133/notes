# Kubernetes 组件

>一组工作机器，称为节点， 会运行容器化应用程序。每个集群至少有一个工作节点。
>工作节点会托管 Pod，而 Pod 就是作为应用负载的组件。 控制平面管理集群中的工作节点和 Pod。 在生产环境中，控制平面通常跨多台计算机运行， 一个集群通常运行多个节点，提供容错性和高可用性。

![一个正常运行的 Kubernetes 集群所需的各种组件](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

## 控制平面组件（Control Plane Components）
### kube-apiserver
### etcd
### kube-scheduler
### kube-controller-manager
### cloud-controller-manager

## Node 组件
### kubelet
### kube-proxy
### 容器运行时（Container Runtime）

## 插件（Addons）
### DNS
### Web 界面（仪表盘）
### 容器资源监控
### 集群层面日志
### 网络插件
# 1. Kubernetes介绍
Kubernetes是一个可以移植、可扩展的开源平台，使用`声明式的配置`并依据配置信息自动地执行容器化应用程序的管理。在所有的容器编排工具中（类似的还有 docker swarm / mesos等），Kubernetes的生态系统更大、增长更快，有更多的支持、服务和工具可供用户选择。

Kubernetes的名字起源于希腊语，含义是`舵手`、`领航员`、`向导`。Google于2014年将Brog系统开源为Kubernetes。

## 1.1. kubernetes 功能
Kubernetes 提供的特性有：
- 服务发现和负载均衡    
  Kubernetes 可以通过 DNS 名称或 IP 地址暴露容器的访问方式。并且可以在同组容器内分发负载以实现负载均衡
- 存储编排    
  Kubernetes可以自动挂载指定的存储系统，例如 local stroage/nfs/云存储等
- 自动发布和回滚    
  您可以在 Kubernetes 中声明您期望应用程序容器应该达到的状态，Kubernetes将以合适的速率调整容器的实际状态，并逐步达到最终期望的结果。
+ 自愈
  - 重启已经停机的容器
  - 替换、kill 那些不满足自定义健康检查条件的容器
  - 在容器就绪之前，避免调用者发现该容器
+ 密钥和配置管理     
  Kubernetes可以存储和管理敏感信息（例如，密码、OAuth token、ssh密钥等）。您可以更新容器应用程序的密钥、配置等信息，而无需：
  - 重新构建容器的镜像
  - 在不合适的地方暴露密码信息
  - Kubernetes的边界

## 1.2. Kubernetes 边界

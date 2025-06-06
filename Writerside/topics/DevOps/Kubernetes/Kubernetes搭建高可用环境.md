# Kubernetes搭建高可用环境
- os：ubuntu-20.04.5
- haproxy：HA-Proxy version 2.0.29-0ubuntu1 2022/08/26
- keepalived：Keepalived v2.0.19 (10/19,2019)
- Kubernetes：v1.26.0
- 链接：[版本偏差策略](https://kubernetes.io/zh-cn/releases/version-skew-policy/)
- 链接：[High Availability Considerations](https://github.com/kubernetes/kubeadm/blob/main/docs/ha-considerations.md#options-for-software-load-balancing)

|ip|角色|
|::|::|
|192.168.213.131|mater01、控制节点、haproxy、keepalived|
|192.168.213.132|mater02、控制节点、haproxy、keepalived|
|192.168.213.133|mater03、控制节点、haproxy、keepalived|
|192.168.213.134|node01、工作节点|
|192.168.213.135|虚拟ip|

## 高可用部署方式
链接：[利用 kubeadm 创建高可用集群](https://kubernetes.io/zh-cn/docs/setup/production-environment/tools/kubeadm/high-availability/)

### 堆叠（Stacked）etcd 拓扑
需要准备：

+ 配置满足`kubeadm 的最低要求`的三台机器作为控制面节点。控制平面节点为奇数有利于机器故障或者分区故障时重新选举。
    - 机器已经安装好容器运行时，并正常运行
+ 配置满足 kubeadm 的最低要求 的三台机器作为工作节点
    - 机器已经安装好容器运行时，并正常运行
+ 在集群中，确保所有计算机之间存在全网络连接（公网或私网）
+ 在所有机器上具有`sudo`权限
    - 可以使用其他工具；本教程以`sudo`举例
+ 从某台设备通过 SSH 访问系统中所有节点的能力
+ 所有机器上已经安装`kubeadm`和`kubelet`

![方式二](./方式2.png){ thumbnail="true" }

### 外部etcd拓扑
需要准备：

+ 配置满足 kubeadm 的最低要求 的三台机器作为控制面节点。控制平面节点为奇数有利于机器故障或者分区故障时重新选举。
    - 机器已经安装好容器运行时，并正常运行
+ 配置满足 kubeadm 的最低要求 的三台机器作为工作节点
    - 机器已经安装好容器运行时，并正常运行
+ 在集群中，确保所有计算机之间存在全网络连接（公网或私网）
+ 在所有机器上具有 sudo 权限
    - 可以使用其他工具；本教程以 sudo 举例
+ 从某台设备通过 SSH 访问系统中所有节点的能力
+ 所有机器上已经安装 kubeadm 和 kubelet

还需要准备：

+ 给 etcd 集群使用的另外至少三台机器。为了分布式一致性算法达到更好的投票效果，集群必须由奇数个节点组成。
    - 机器上已经安装 kubeadm 和 kubelet。
    - 机器上同样需要安装好容器运行时，并能正常运行。

![方式一](./方式1.png){ thumbnail="true" }

## 安装docker
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

## 设置网络
Ubuntu 20.04 配置网络，默认使用`netplan`方式进行设置。

netplan 方式的配置为`yaml`格式
```bash
sudo vi /etc/netplan/00-installer-config.yaml
```

编辑以下文件：
```yaml
network:
  ethernets:
    ens32:
      dhcp4: no
      addresses: [192.168.213.131/24] 
      gateway4: 192.168.213.2
      nameservers:
       addresses: [192.168.213.2]
  version: 2
```

启动服务：
```bash
sudo netplan apply
```

查看状态：
```bash
networkctl status
```

```bash
ubuntu@ubuntu:/etc/netplan$ networkctl status
●   State: routable                             
  Address: 192.168.213.131 on ens32             
           172.17.0.1 on docker0                
           fe80::20c:29ff:fe67:d209 on ens32    
  Gateway: 192.168.213.2 (VMware, Inc.) on ens32
      DNS: 192.168.213.2                        

Jan 11 08:43:27 ubuntu systemd-networkd[809]: ens32: Re-configuring with /run/systemd/network/10-netplan-ens32.network
Jan 11 08:43:27 ubuntu systemd-networkd[809]: ens32: IPv6 successfully enabled
Jan 11 08:47:26 ubuntu systemd-networkd[809]: ens32: Re-configuring with /run/systemd/network/10-netplan-ens32.network
Jan 11 08:47:26 ubuntu systemd-networkd[809]: ens32: IPv6 successfully enabled
Jan 11 08:47:26 ubuntu systemd-networkd[809]: ens32: Re-configuring with /run/systemd/network/10-netplan-ens32.network
Jan 11 08:47:26 ubuntu systemd-networkd[809]: ens32: IPv6 successfully enabled
Jan 11 08:49:44 ubuntu systemd-networkd[809]: ens32: Re-configuring with /run/systemd/network/10-netplan-ens32.network
Jan 11 08:49:44 ubuntu systemd-networkd[809]: ens32: IPv6 successfully enabled
Jan 11 08:49:44 ubuntu systemd-networkd[809]: ens32: Re-configuring with /run/systemd/network/10-netplan-ens32.network
Jan 11 08:49:44 ubuntu systemd-networkd[809]: ens32: IPv6 successfully enabled
```

## 节点互信
每个节点执行以下操作：

```bash
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@192.168.213.131
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@192.168.213.132
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@192.168.213.133
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@192.168.213.134
```

## hostname
编辑`/etc/hosts`：

```html
192.168.213.131 master01
192.168.213.132 master02
192.168.213.133 master03
192.168.213.134 node01

```

设置hostname（在对应的机器上执行以下命令）:

```bash
hostnamectl set-hostname master01
hostnamectl set-hostname master02
hostnamectl set-hostname master03
hostnamectl set-hostname node01
```

## 永久禁用swap
编辑`/etc/fstab`文件，注释以下内容：

```
/swap.img      none    swap    sw      0       0
```

## 关闭防火墙
```bash
sudo ufw disable
```

## 负载均衡
### keepalived 和 haproxy
- haproxy：HAProxy是一个特别适用于高可用性环境的TCP/HTTP开源的反向代理和负载均衡软件。在7层负载均衡方面的功能很强大(支持cookie track, header rewrite等等)，支持双机热备，支持虚拟主机，支持健康检查，同时还提供直观的监控页面，可以清晰实时的监控服务集群的运行状况。同时支持Linux 2.6内核中System Epoll，通过简化系统调用，大幅的提高了网络I/O性能。
- keepalived：keepalived是集群管理中保证集群高可用的一个服务软件，用来防止单点故障。keepalived是以VRRP协议为实现基础的，VRRP全称Virtual Router Redundancy Protocol，即虚拟路由冗余协议。（云服务器不支持VRRP）

### 安装
节点`master01`、`master02`、`master03`执行以下命令：

```bash
sudo apt install keepalived haproxy
```

### 配置
#### haproxy
`sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.backup`进行备份。

节点`master01`、`master02`、`master03`编辑`/etc/haproxy/haproxy.cfg`:

```cfg
# /etc/haproxy/haproxy.cfg
#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    log /dev/log local0
    log /dev/log local1 notice
    daemon

#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 1
    timeout http-request    10s
    timeout queue           20s
    timeout connect         5s
    timeout client          20s
    timeout server          20s
    timeout http-keep-alive 10s
    timeout check           10s

listen stats
  bind    *:8006
  mode    http
  stats   enable
  stats   hide-version
  stats   uri       /stats
  stats   refresh   30s
  stats   realm     Haproxy\ Statistics
  stats   auth      admin:admin

#---------------------------------------------------------------------
# apiserver frontend which proxys to the control plane nodes
#---------------------------------------------------------------------
frontend apiserver
    bind *:8443
    mode tcp
    option tcplog
    default_backend apiserver

#---------------------------------------------------------------------
# round robin balancing for apiserver
#---------------------------------------------------------------------
backend apiserver
    option httpchk GET /healthz
    http-check expect status 200
    mode tcp
    option ssl-hello-chk
    balance     roundrobin
        server master01 192.168.213.131:6443  check inter 2000 fall 2 rise 2 weight 100
        server master02 192.168.213.132:6443  check inter 2000 fall 2 rise 2 weight 100
        server master03 192.168.213.133:6443  check inter 2000 fall 2 rise 2 weight 100
```

#### keepalived
##### keepalived.conf
节点`master01`编辑`/etc/keepalived/keepalived.conf`:

```cfg
! /etc/keepalived/keepalived.conf
! Configuration File for keepalived
global_defs {
    router_id LVS_DEVEL
}
vrrp_script check_apiserver {
  script "/etc/keepalived/check_apiserver.sh"
  interval 3
  weight -2
  fall 10
  rise 2
}

vrrp_instance VI_1 {
    state MASTER
    interface ens32
    mcast_src_ip 192.168.213.131
    virtual_router_id 51
    priority 100
    authentication {
        auth_type PASS
        auth_pass 42
    }
    virtual_ipaddress {
        192.168.213.135/24 dev ens32
    }
    track_script {
        check_apiserver
    }
}
```

节点`master02`编辑`/etc/keepalived/keepalived.conf`:

```cfg
! /etc/keepalived/keepalived.conf
! Configuration File for keepalived
global_defs {
    router_id LVS_DEVEL
}
vrrp_script check_apiserver {
  script "/etc/keepalived/check_apiserver.sh"
  interval 3
  weight -2
  fall 10
  rise 2
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens32
    mcast_src_ip 192.168.213.132
    virtual_router_id 51
    priority 99
    authentication {
        auth_type PASS
        auth_pass 42
    }
    virtual_ipaddress {
        192.168.213.135/24 dev ens32
    }
    track_script {
        check_apiserver
    }
}
```

节点`master03`编辑`/etc/keepalived/keepalived.conf`:

```cfg
! /etc/keepalived/keepalived.conf
! Configuration File for keepalived
global_defs {
    router_id LVS_DEVEL
}
vrrp_script check_apiserver {
  script "/etc/keepalived/check_apiserver.sh"
  interval 3
  weight -2
  fall 10
  rise 2
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens32
    mcast_src_ip 192.168.213.133
    virtual_router_id 51
    priority 98
    authentication {
        auth_type PASS
        auth_pass 42
    }
    virtual_ipaddress {
        192.168.213.135/24 dev ens32
    }
    track_script {
        check_apiserver
    }
}
```

##### check_apiserver.sh
健康检查脚本`/etc/keepalived/check_apiserver.sh`：

```sh
#!/bin/sh

errorExit() {
    echo "*** $*" 1>&2
    exit 1
}

curl --silent --max-time 2 --insecure https://localhost:8443/ -o /dev/null || errorExit "Error GET https://localhost:8443/"
if ip addr | grep -q 192.168.231.135; then
    curl --silent --max-time 2 --insecure https://192.168.231.135:8443/ -o /dev/null || errorExit "Error GET https://192.168.231.135:8443/"
fi
```

节点`master01`、`master02`、`master03`运行`sudo chmod u+x /etc/keepalived/check_apiserver.sh` 设置脚本运行权限。

### 启动并设置开机自动启动
```bash
sudo systemctl enable --now keepalived haproxy
sudo systemctl status keepalived haproxy
```

结果如下：
```bash
ubuntu@master01:/etc/keepalived$ sudo systemctl status keepalived haproxy
● keepalived.service - Keepalive Daemon (LVS and VRRP)
     Loaded: loaded (/lib/systemd/system/keepalived.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2023-01-11 17:30:00 UTC; 38s ago
   Main PID: 16290 (keepalived)
      Tasks: 2 (limit: 2236)
     Memory: 7.0M
     CGroup: /system.slice/keepalived.service
             ├─16290 /usr/sbin/keepalived --dont-fork
             └─16291 /usr/sbin/keepalived --dont-fork

Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: Registering Kernel netlink command channel
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: Opening file '/etc/keepalived/keepalived.conf'.
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: WARNING - default user 'keepalived_script' for script execution does not exist - please create.
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: SECURITY VIOLATION - scripts are being executed but script_security not enabled.
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: Registering gratuitous ARP shared channel
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: (VI_1) Entering BACKUP STATE (init)
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: Script `check_apiserver` now returning 1
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: VRRP_Script(check_apiserver) failed (exited with status 1)
Jan 11 17:30:00 master01 Keepalived_vrrp[16291]: (VI_1) Changing effective priority from 101 to 99
Jan 11 17:30:04 master01 Keepalived_vrrp[16291]: (VI_1) Entering MASTER STATE

● haproxy.service - HAProxy Load Balancer
     Loaded: loaded (/lib/systemd/system/haproxy.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2023-01-11 14:21:37 UTC; 3h 9min ago
       Docs: man:haproxy(1)
             file:/usr/share/doc/haproxy/configuration.txt.gz
   Main PID: 10923 (haproxy)
      Tasks: 3 (limit: 2236)
     Memory: 34.6M
     CGroup: /system.slice/haproxy.service
             ├─10923 /usr/sbin/haproxy -Ws -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -S /run/haproxy-master.sock
             └─10942 /usr/sbin/haproxy -Ws -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -S /run/haproxy-master.sock

Jan 11 14:21:36 master01 systemd[1]: Starting HAProxy Load Balancer...
Jan 11 14:21:37 master01 haproxy[10923]: [NOTICE] 010/142136 (10923) : New worker #1 (10942) forked
Jan 11 14:21:37 master01 systemd[1]: Started HAProxy Load Balancer.
ubuntu@master01:/etc/keepalived$ ping 192.168.231.135
PING 192.168.231.135 (192.168.231.135) 56(84) bytes of data.
64 bytes from 192.168.231.135: icmp_seq=1 ttl=64 time=0.039 ms
64 bytes from 192.168.231.135: icmp_seq=2 ttl=64 time=0.042 ms
64 bytes from 192.168.231.135: icmp_seq=3 ttl=64 time=0.064 ms
64 bytes from 192.168.231.135: icmp_seq=4 ttl=64 time=0.083 ms

# 验证
ubuntu@master01:/etc/keepalived$ sudo systemctl stop keepalived haproxy

ubuntu@master03:/etc/keepalived$ ping 192.168.231.135
PING 192.168.231.135 (192.168.231.135) 56(84) bytes of data.
64 bytes from 192.168.231.135: icmp_seq=60 ttl=64 time=0.136 ms
64 bytes from 192.168.231.135: icmp_seq=61 ttl=64 time=0.029 ms


ubuntu@master03:/etc/keepalived$ sudo systemctl stop keepalived haproxy

ubuntu@master02:/etc/keepalived$ ping 192.168.231.135
PING 192.168.231.135 (192.168.231.135) 56(84) bytes of data.
64 bytes from 192.168.231.135: icmp_seq=117 ttl=64 time=0.188 ms
64 bytes from 192.168.231.135: icmp_seq=118 ttl=64 time=0.160 ms
64 bytes from 192.168.231.135: icmp_seq=119 ttl=64 time=0.041 ms
64 bytes from 192.168.231.135: icmp_seq=120 ttl=64 time=0.058 ms
```

## 安装kubeadm、kubelet和kubectl
- kubeadm：用来初始化集群的指令。
- kubelet：在集群中的每个节点上用来启动 Pod 和容器等。
- kubectl：用来与集群通信的命令行工具。

更新`apt`包索引并安装使用`Kubernetes apt`仓库所需要的包：
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```

下载 Google Cloud 公开签名秘钥：
```bash
sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```

添加 Kubernetes `apt` 仓库：
```bash
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

更新`apt`包索引，安装 `kubelet、kubeadm 和 kubectl`，并锁定其版本：
```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl enable kubelet
```

国内源：
```bash
apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
```

## 使用配置文件初始化
链接：[结合一份配置文件来使用 kubeadm init](https://kubernetes.io/zh-cn/docs/reference/setup-tools/kubeadm/kubeadm-init/#config-file)

### 生成kubeadm默认配置文件
在`master01`节点上执行：
```bash
kubeadm config print init-defaults --component-configs \
KubeProxyConfiguration,KubeletConfiguration > kubeadm-config.yaml
```

### 修改配置文件`kubeadm-config.yaml`
```yaml
apiVersion: kubeadm.k8s.io/v1beta3
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 192.168.213.135    ## Vip地址
  bindPort: 6443
nodeRegistration:
  criSocket: unix:///var/run/containerd/containerd.sock
  imagePullPolicy: IfNotPresent
  name: master01   ## 第一台执行节点的hostname
  taints: null
---
apiServer:
  certSANs:
  - "192.168.213.135"  ## Vip地址
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta3
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: {}
dns: {}
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: registry.k8s.io   ## imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers    #aliyun地址
controlPlaneEndpoint: "192.168.213.135:8443"  ## Vip地址和端口
kind: ClusterConfiguration
kubernetesVersion: 1.26.0
networking:
  dnsDomain: cluster.local
  serviceSubnet: 10.96.0.0/12
  podSubnet: 10.244.0.0/16    #pod网段
scheduler: {}
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
bindAddress: 0.0.0.0
bindAddressHardFail: false
clientConnection:
  acceptContentTypes: ""
  burst: 0
  contentType: ""
  kubeconfig: /var/lib/kube-proxy/kubeconfig.conf
  qps: 0
clusterCIDR: ""
configSyncPeriod: 0s
conntrack:
  maxPerCore: null
  min: null
  tcpCloseWaitTimeout: null
  tcpEstablishedTimeout: null
detectLocal:
  bridgeInterface: ""
  interfaceNamePrefix: ""
detectLocalMode: ""
enableProfiling: false
healthzBindAddress: ""
hostnameOverride: ""
iptables:
  localhostNodePorts: null
  masqueradeAll: false
  masqueradeBit: null
  minSyncPeriod: 0s
  syncPeriod: 0s
ipvs:
  excludeCIDRs: null
  minSyncPeriod: 0s
  scheduler: ""
  strictARP: false
  syncPeriod: 0s
  tcpFinTimeout: 0s
  tcpTimeout: 0s
  udpTimeout: 0s
kind: KubeProxyConfiguration
metricsBindAddress: ""
mode: ""
nodePortAddresses: null
oomScoreAdj: null
portRange: ""
showHiddenMetricsForVersion: ""
winkernel:
  enableDSR: false
  forwardHealthCheckVip: false
  networkName: ""
  rootHnsEndpointName: ""
  sourceVip: ""
---
apiVersion: kubelet.config.k8s.io/v1beta1
authentication:
  anonymous:
    enabled: false
  webhook:
    cacheTTL: 0s
    enabled: true
  x509:
    clientCAFile: /etc/kubernetes/pki/ca.crt
authorization:
  mode: Webhook
  webhook:
    cacheAuthorizedTTL: 0s
    cacheUnauthorizedTTL: 0s
cgroupDriver: systemd
clusterDNS:
- 10.96.0.10
clusterDomain: cluster.local
cpuManagerReconcilePeriod: 0s
evictionPressureTransitionPeriod: 0s
fileCheckFrequency: 0s
healthzBindAddress: 127.0.0.1
healthzPort: 10248
httpCheckFrequency: 0s
imageMinimumGCAge: 0s
kind: KubeletConfiguration
logging:
  flushFrequency: 0
  options:
    json:
      infoBufferSize: "0"
  verbosity: 0
memorySwap: {}
nodeStatusReportFrequency: 0s
nodeStatusUpdateFrequency: 0s
resolvConf: /run/systemd/resolve/resolv.conf
rotateCertificates: true
runtimeRequestTimeout: 0s
shutdownGracePeriod: 0s
shutdownGracePeriodCriticalPods: 0s
staticPodPath: /etc/kubernetes/manifests
streamingConnectionIdleTimeout: 0s
syncFrequency: 0s
volumeStatsAggPeriod: 0s
```

### 初始化集群
在`master01`节点上执行：
```bash
sudo kubeadm init --config kubeadm-config.yaml --upload-certs

# --upload-certs 将证书保存到 kube-system 名称空间下名为 extension-apiserver-authentication 的 configmap 中，这样其他控制平面加入的话只要加上 --control-plane --certificate-key 并带上相应的key就可以拿到证书并下载到本地。
# 安装keepalived需要保证虚拟ip可以ping通
```

初始化成功的信息如下：
```bash
ubuntu@master01:~$ sudo kubeadm init --config kubeadm-config.yaml --upload-certs
[init] Using Kubernetes version: v1.26.0
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local master01] and IPs [10.96.0.1 192.168.213.135]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [localhost master01] and IPs [192.168.213.135 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [localhost master01] and IPs [192.168.213.135 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
W0112 19:53:13.354392   15895 endpoint.go:57] [endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "admin.conf" kubeconfig file
W0112 19:53:13.464600   15895 endpoint.go:57] [endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "kubelet.conf" kubeconfig file
W0112 19:53:13.651047   15895 endpoint.go:57] [endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
W0112 19:53:13.749433   15895 endpoint.go:57] [endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 6.030845 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
9cafc1f897b53b4e599247bdcdd727ede48957d6a341a03a9fff2f00eb46a19b
[mark-control-plane] Marking the node master01 as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers][mark-control-plane] Marking the node master01 as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: abcdef.0123456789abcdef
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
W0112 19:53:24.548796   15895 endpoint.go:57] [endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 192.168.213.135:8443 --token abcdef.0123456789abcdef \
        --discovery-token-ca-cert-hash sha256:afc9c47236426463fa78e6a70fe227297946ce3cf0a05fbc0b7cc76a95eb53f4 \
        --control-plane --certificate-key 9cafc1f897b53b4e599247bdcdd727ede48957d6a341a03a9fff2f00eb46a19b

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.213.135:8443 --token abcdef.0123456789abcdef \
        --discovery-token-ca-cert-hash sha256:afc9c47236426463fa78e6a70fe227297946ce3cf0a05fbc0b7cc76a95eb53f4 
```

执行以下命令：
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 出错
##### [ERROR CRI]: container runtime is not running
```bash
ubuntu@master01:~$ sudo kubeadm init --config kubeadm-config.yaml --upload-certs
[init] Using Kubernetes version: v1.26.0
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR CRI]: container runtime is not running: output: E0112 16:41:12.923466   13689 remote_runtime.go:948] "Status from runtime service failed" err="rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
time="2023-01-12T16:41:12Z" level=fatal msg="getting status of runtime: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

解决方法：
```bash
sudo rm -rf /etc/containerd/config.toml
sudo systemctl restart containerd
```

##### error execution phase preflight: [preflight] Some fatal errors occurred:
```bash
[init] Using Kubernetes version: v1.26.0
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR Port-10259]: Port 10259 is in use
        [ERROR Port-10257]: Port 10257 is in use
        [ERROR FileAvailable--etc-kubernetes-manifests-kube-apiserver.yaml]: /etc/kubernetes/manifests/kube-apiserver.yaml already exists
        [ERROR FileAvailable--etc-kubernetes-manifests-kube-controller-manager.yaml]: /etc/kubernetes/manifests/kube-controller-manager.yaml already exists
        [ERROR FileAvailable--etc-kubernetes-manifests-kube-scheduler.yaml]: /etc/kubernetes/manifests/kube-scheduler.yaml already exists
        [ERROR FileAvailable--etc-kubernetes-manifests-etcd.yaml]: /etc/kubernetes/manifests/etcd.yaml already exists
        [ERROR Port-10250]: Port 10250 is in use
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

解决办法：
```bash
sudo kubeadm reset
```

## 安装 Pod 网络附加组件
1. 下载flannel资源配置文件
```bash
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

2. 修改`kube-flannel.yml`
```yaml
# 修改pod网络和网络模式
  net-conf.json: |
    {
      "Network": "10.244.0.0/16",
      "Backend": {
        "Type": "host-gw"
      }
    }
```

3. 部署
```bash
kubectl apply -f kube-flannel.yml
```

4. 验证
```bash
ubuntu@master01:~$ kubectl get pods --all-namespaces
NAMESPACE      NAME                               READY   STATUS    RESTARTS   AGE
kube-flannel   kube-flannel-ds-fxcgl              1/1     Running   0          11m  # 确认其输出结果中 Pod 的状态为 “Running”
kube-system    coredns-787d4945fb-sc9n5           0/1     Running   0          35m
kube-system    coredns-787d4945fb-ttkr2           0/1     Running   0          35m
kube-system    etcd-master01                      1/1     Running   19         35m
kube-system    kube-apiserver-master01            1/1     Running   16         35m
kube-system    kube-controller-manager-master01   1/1     Running   3          35m
kube-system    kube-proxy-6bt7q                   1/1     Running   0          35m
kube-system    kube-scheduler-master01            1/1     Running   3          35m
ubuntu@master01:~$ kubectl get node
NAME       STATUS   ROLES           AGE   VERSION
master01   Ready    control-plane   35m   v1.26.0          #目前节点状态是Ready，表示集群节点现在是可用的
```

## 加入节点
```bash
# master02节点
sudo kubeadm reset
sudo kubeadm join 192.168.213.135:8443 --token abcdef.0123456789abcdef \
        --discovery-token-ca-cert-hash sha256:afc9c47236426463fa78e6a70fe227297946ce3cf0a05fbc0b7cc76a95eb53f4 \
        --control-plane --certificate-key 9cafc1f897b53b4e599247bdcdd727ede48957d6a341a03a9fff2f00eb46a19b

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# master03节点
sudo kubeadm reset
sudo kubeadm join 192.168.213.135:8443 --token abcdef.0123456789abcdef \
        --discovery-token-ca-cert-hash sha256:afc9c47236426463fa78e6a70fe227297946ce3cf0a05fbc0b7cc76a95eb53f4 \
        --control-plane --certificate-key 9cafc1f897b53b4e599247bdcdd727ede48957d6a341a03a9fff2f00eb46a19b

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# node01节点
sudo kubeadm reset
sudo kubeadm join 192.168.213.135:8443 --token abcdef.0123456789abcdef \
        --discovery-token-ca-cert-hash sha256:afc9c47236426463fa78e6a70fe227297946ce3cf0a05fbc0b7cc76a95eb53f4 
```

查看集群所有节点信息：
```bash
ubuntu@master01:~$ kubectl get pods --all-namespaces
NAMESPACE      NAME                               READY   STATUS    RESTARTS         AGE
kube-flannel   kube-flannel-ds-5ktvb              1/1     Running   2 (3m49s ago)    21m
kube-flannel   kube-flannel-ds-nnj4r              1/1     Running   2 (3m49s ago)    21m
kube-flannel   kube-flannel-ds-nz4ls              1/1     Running   2 (3m49s ago)    21m
kube-flannel   kube-flannel-ds-t7flw              1/1     Running   2 (3m42s ago)    21m
kube-system    coredns-787d4945fb-497k8           1/1     Running   1 (6m18s ago)    31m
kube-system    coredns-787d4945fb-wbnhl           1/1     Running   1 (6m18s ago)    31m
kube-system    etcd-master01                      1/1     Running   24 (6m19s ago)   31m
kube-system    etcd-master02                      1/1     Running   1 (5m37s ago)    27m
kube-system    etcd-master03                      1/1     Running   1 (6m11s ago)    25m
kube-system    kube-apiserver-master01            1/1     Running   21 (6m19s ago)   31m
kube-system    kube-apiserver-master02            1/1     Running   1 (5m37s ago)    27m
kube-system    kube-apiserver-master03            1/1     Running   1 (6m11s ago)    25m
kube-system    kube-controller-manager-master01   1/1     Running   9 (6m18s ago)    31m
kube-system    kube-controller-manager-master02   1/1     Running   1 (5m37s ago)    27m
kube-system    kube-controller-manager-master03   1/1     Running   1 (6m11s ago)    25m
kube-system    kube-proxy-f4qts                   1/1     Running   1 (6m19s ago)    25m
kube-system    kube-proxy-h9gg6                   1/1     Running   1 (6m20s ago)    31m
kube-system    kube-proxy-k9mdj                   1/1     Running   1 (6m12s ago)    25m
kube-system    kube-proxy-zknxn                   1/1     Running   1 (5m37s ago)    27m
kube-system    kube-scheduler-master01            1/1     Running   9 (6m18s ago)    31m
kube-system    kube-scheduler-master02            1/1     Running   1 (5m37s ago)    27m
kube-system    kube-scheduler-master03            1/1     Running   1 (6m12s ago)    25m
ubuntu@master01:~$ kubectl get node
NAME       STATUS   ROLES           AGE   VERSION
master01   Ready    control-plane   31m   v1.26.0
master02   Ready    control-plane   27m   v1.26.0
master03   Ready    control-plane   25m   v1.26.0
node01     Ready    <none>          26m   v1.26.0
```

## 测试
```bash
ubuntu@master01:~$ kubectl create deployment nginx --image=nginx
deployment.apps/nginx created
ubuntu@master01:~$ kubectl expose deployment nginx --port=80 --type=NodePort
service/nginx exposed
ubuntu@master01:~$ kubectl get pod,svc -o wide
NAME                         READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
pod/nginx-748c667d99-v9vbc   1/1     Running   0          23s   10.244.2.2   node01   <none>           <none>

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE   SELECTOR
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        35m   <none>
service/nginx        NodePort    10.110.49.104   <none>        80:32201/TCP   16s   app=nginx
```

结果如下：
```bash
ubuntu@master01:~$ curl 10.110.49.104
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

![结果](./image_2023-01-13_04-37-49.png){ thumbnail="true" }

## 安装 Dashboard 插件
### 下载清单文件
```bash
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

### 修改recommended.yaml
```yaml
# Copyright 2017 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

apiVersion: v1
kind: Namespace
metadata:
  name: kubernetes-dashboard

---

apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard

---

kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
spec:
  type: NodePort    ## 添加 type: NodePort
  ports:
    - port: 443
      targetPort: 8443
      nodePort: 30443  ## 添加nodePort
  selector:
    k8s-app: kubernetes-dashboard


---



kind: Deployment
apiVersion: apps/v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
spec:
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      k8s-app: kubernetes-dashboard
  template:
    metadata:
      labels:
        k8s-app: kubernetes-dashboard
    spec:
      securityContext:
        seccompProfile:
          type: RuntimeDefault
      containers:
        - name: kubernetes-dashboard
          image: kubernetesui/dashboard:v2.7.0
          imagePullPolicy: Always
          ports:
            - containerPort: 8443
              protocol: TCP
          args:
            - --tls-cert-file=/tls.crt  ## tls.crt
            - --tls-key-file=/tls.key   ## tls.key
            - --auto-generate-certificates
            - --namespace=kubernetes-dashboard
           
```

### 部署
```bash
kubectl create secret generic kubernetes-dashboard-certs --from-file=$HOME/certs -n kubernetes-dashboard
kubectl create -f recommended.yaml
```

### 生成证书
```bash
openssl genrsa -des3 -passout pass:over4chars -out dashboard.pass.key 2048
openssl rsa -passin pass:over4chars -in dashboard.pass.key -out dashboard.key
rm dashboard.pass.key
openssl req -new -key dashboard.key -out dashboard.csr
openssl x509 -req -sha256 -days 365 -in dashboard.csr -signkey dashboard.key -out dashboard.crt
```

将生成的`dashboard.key`和`dashboard.crt` 改为`tls.key`和`tls.crt`，并移动到`$HOME/certs`路径下。

### token
```bash
tee dashboard-admin.yaml<<EOF
---
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: dashboard-admin
  namespace: kubernetes-dashboard

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: dashboard-admin-bind-cluster-role
  labels:
    k8s-app: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: dashboard-admin
  namespace: kubernetes-dashboard
EOF

kubectl create -f dashboard-admin.yaml
```

生成token:
```bash
ubuntu@master01:~$ kubectl -n kubernetes-dashboard create token dashboard-admin
eyJhbGciOiJSUzI1NiIsImtpZCI6ImYtU3k5a3Joa0p1ZktyVDU4RjFfZEJ2MUJFbllBU252YlM0WF8xSXhPOWcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjczNTcyOTU0LCJpYXQiOjE2NzM1NjkzNTQsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJkYXNoYm9hcmQtYWRtaW4iLCJ1aWQiOiIyMWJmMDhjNi05Njg0LTQzN2UtOGY3Mi1jZDVkMGUyZGViMzAifX0sIm5iZiI6MTY3MzU2OTM1NCwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmVybmV0ZXMtZGFzaGJvYXJkOmRhc2hib2FyZC1hZG1pbiJ9.bJcya-Zw7uEtzwD_tk_LtKXLB8wtWO20dpy29P-Y0GQQpBGhvaxZ_ss5kqcIWceHGWWHnnnMmPsXLc35zAv4CbPcFs5e4wL07RlNsW6ijhNAPVH5Yy6RdTFcwmfuoUkd5cZbTIIzWbSwLiLMOZ7t5B-SsXjKtvPZIJ1IsbK07dpzNLqV_vDoNH6WsAukocrmYOjY-SgWON7K-fsu5NxYqaeUllxqPARsepVnoiVsltONvVP2x5Id6MQRlqtQgMZ7uvy_KOuG8C3KzMWO4z6uU-EtdW0Vo7HL6iKPz592dg_jWedlTV2eMAReVD2kvmOu8vWjillMi1tI09vudZ-xWg
```

### 结果
```bash
ubuntu@master01:~$ kubectl get service -A
NAMESPACE              NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
default                kubernetes                  ClusterIP   10.96.0.1        <none>        443/TCP                  4h45m
default                nginx                       NodePort    10.110.49.104    <none>        80:32201/TCP             4h10m
kube-system            kube-dns                    ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP   4h45m
kubernetes-dashboard   dashboard-metrics-scraper   ClusterIP   10.98.74.149     <none>        8000/TCP                 20m
kubernetes-dashboard   kubernetes-dashboard        NodePort    10.109.175.181   <none>        443:30443/TCP            20m
```

![kubernetes-dashboard](./image_2023-01-13_08-39-36.png){ thumbnail="true" }

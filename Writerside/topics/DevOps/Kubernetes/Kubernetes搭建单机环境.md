# Kubernetes搭建单机环境
<show-structure for="chapter,procedure"/>

## 前提

- 2 CPUs or more
- 2GB of free memory
- 20GB of free disk space
- Internet connection
- Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or
  VMware Fusion/Workstation

## Docker

[Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)

## minikube

### 安装文档

[minikube](https://minikube.sigs.k8s.io/docs/start/)

### centos安装minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### 启动集群

```bash
minikube start
```

如下：

```bash
[centos@localhost ~]$ minikube start
😄  minikube v1.32.0 on Centos 7.9.2009
✨  Automatically selected the docker driver. Other choices: none, ssh

🧯  The requested memory allocation of 2200MiB does not leave room for system overhead (total system memory: 2827MiB). You may face stability issues.
💡  Suggestion: Start minikube with less memory allocated: 'minikube start --memory=2200mb'

📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.28.3 preload ...
    > preloaded-images-k8s-v18-v1...:  403.35 MiB / 403.35 MiB  100.00% 8.81 Mi
    > gcr.io/k8s-minikube/kicbase...:  453.90 MiB / 453.90 MiB  100.00% 8.13 Mi
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

### 安装kubectl

#### Add the Kubernetes yum repository

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.29/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.29/rpm/repodata/repomd.xml.key
EOF
```

#### Install kubectl using yum

```bash
sudo yum install -y kubectl
```

### 命令补全

```bash
sudo yum install -y bash-completion
source /usr/share/bash-completion/bash_completion
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

### 验证

```bash
[centos@localhost ~]$ kubectl version 
Client Version: v1.29.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.28.3
[centos@localhost ~]$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[centos@localhost ~]$ kubectl get nodes 
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   6m13s   v1.28.3
[centos@localhost ~]$ kubectl get pods -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-5dd5756b68-2xf4p           1/1     Running   0          6m18s
kube-system   etcd-minikube                      1/1     Running   0          6m32s
kube-system   kube-apiserver-minikube            1/1     Running   0          6m30s
kube-system   kube-controller-manager-minikube   1/1     Running   0          6m30s
kube-system   kube-proxy-7rg4q                   1/1     Running   0          6m19s
kube-system   kube-scheduler-minikube            1/1     Running   0          6m30s
kube-system   storage-provisioner                1/1     Running   0          6m29s
```

### 停止集群

```bash
[centos@localhost ~]$ minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
```

### 问题

#### ❗ You appear to be using a proxy, but your NO_PROXY environment does not include the minikube IP (192.168.49.2).

##### macOS and Linux

```bash
export HTTP_PROXY=http://<proxy hostname:port>
export HTTPS_PROXY=https://<proxy hostname:port>
export NO_PROXY=localhost,127.0.0.1,10.96.0.0/12,192.168.59.0/24,192.168.49.0/24,192.168.39.0/24

minikube start
```

##### Windows

```bash
set HTTP_PROXY=http://<proxy hostname:port>
set HTTPS_PROXY=https://<proxy hostname:port>
set NO_PROXY=localhost,127.0.0.1,10.96.0.0/12,192.168.59.0/24,192.168.49.0/24,192.168.39.0/24

minikube start
```

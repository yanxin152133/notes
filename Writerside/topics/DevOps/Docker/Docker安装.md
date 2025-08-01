# Docker 安装
参考以下链接：    
[Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)     
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)     

## Centos
### 设置开机启动Docker

```bash
sudo systemctl enable docker
```

### centos docker命令补全

```bash
# 首先下载 bash-completion
sudo yum install -y bash-completion
# 然后立即生效
source /usr/share/bash-completion/bash_completion
source /usr/share/bash-completion/completions/docker
```

### 防火墙问题
```bash
sudo firewall-cmd --zone=trusted --remove-interface=docker0 --permanent
sudo firewall-cmd --reload
```

## 普通用户使用docker
建立docker组:

```bash
sudo groupadd docker
```

将当前用户加入docker组：

```bash
sudo usermod -aG docker $USER
```

退出重新登录即生效。

## 关于重新登录后该用户无法正常使用docker的问题
### 重启docker服务
```bash
sudo service docker restart
```

### 切换当前会话到新group或者重启X会话
```bash
newgrp - docker
```

## 更换第三方源和设置DNS
加速镜像：
- https://github.com/dongyubin/DockerHub
- https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6

创建文件：

```bash
sudo vim /etc/docker/daemon.json
```

加入以下内容：

```json
{
  "registry-mirrors": [
    "https://docker.1ms.run"
  ],
  "dns" : [
    "119.29.29.29",
    "8.8.8.8"
  ]
}
```

然后重启Docker。

## 设置socks5
参考链接：https://docs.docker.com/engine/daemon/proxy/

新建文件`/etc/systemd/system/docker.service.d/proxy.conf`：

```conf
[Service]
Environment="ALL_PROXY=socks5://10.0.16.15:10808"
Environment="HTTP_PROXY=socks5://10.0.16.15:10808"
Environment="HTTPS_PROXY=socks5://10.0.16.15:10808"
Environment="NO_PROXY="localhost,127.0.0.1,::1"
```
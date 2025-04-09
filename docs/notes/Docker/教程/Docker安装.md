# 1. Docker 安装
参考以下链接：    
[Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)     
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)     

## 1.1. Centos
### 1.1.1. 设置开机启动Docker

```bash
sudo systemctl enable docker
```

### 1.1.2. centos docker命令补全

```bash
# 首先下载 bash-completion
sudo yum install -y bash-completion
# 然后立即生效
source /usr/share/bash-completion/bash_completion
source /usr/share/bash-completion/completions/docker
```

### 1.1.3. 防火墙问题
```bash
sudo firewall-cmd --zone=trusted --remove-interface=docker0 --permanent
sudo firewall-cmd --reload
```

## 1.2. 普通用户使用docker
建立docker组:

```bash
sudo groupadd docker
```

将当前用户加入docker组：

```bash
sudo usermod -aG docker $USER
```

退出重新登录即生效。

## 1.3. 关于重新登录后该用户无法正常使用docker的问题
### 1.3.1. 重启docker服务
```bash
sudo service docker restart
```

### 1.3.2. 切换当前会话到新group或者重启X会话
```bash
newgrp - docker
```

## 1.4. 更换国内Docker仓库
创建文件：

```bash
sudo vim /etc/docker/daemon.json
```

加入以下内容：

```json
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn"
  ]
}
```

然后重启Docker。

## 1.5. 设置socks5
新建文件`/etc/systemd/system/docker.service.d/proxy.conf`：

```conf
[Service]
Environment="ALL_PROXY=socks5://10.0.16.15:10808"
Environment="HTTP_PROXY=socks5://10.0.16.15:10808"
Environment="HTTPS_PROXY=socks5://10.0.16.15:10808"
Environment="NO_PROXY="localhost,127.0.0.1,::1"
```

# 命令行工具 (Kubectl)

Kubectl 是使用 Kubernetes API 与 Kubernetes 集群的控制面进行通信的命令行工具。

## Linux安装Kubectl

参考链接：[在 Linux 系统中安装并设置 kubectl](https://kubernetes.io/zh-cn/docs/tasks/tools/install-kubectl-linux/)

### ubuntu 安装Kubectl

1. 更新 apt 包索引，并安装使用 Kubernetes apt 仓库所需要的包

```bash
sudo apt-get update
# apt-transport-https 可以是一个虚拟包；如果是这样，你可以跳过这个包
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
```

2. 下载 Kubernetes 软件包仓库的公共签名密钥。 同一个签名密钥适用于所有仓库，因此你可以忽略 URL 中的版本信息

```bash
# 如果 `/etc/apt/keyrings` 目录不存在，则应在 curl 命令之前创建它，请阅读下面的注释。
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg # allow unprivileged APT programs to read this keyring   
```

3. 添加合适的 Kubernetes apt 仓库。如果你想用 v1.33 之外的 Kubernetes 版本， 请将下面命令中的 v1.33 替换为所需的次要版本

```bash
# 这会覆盖 /etc/apt/sources.list.d/kubernetes.list 中的所有现存配置
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list   # 有助于让诸如 command-not-found 等工具正常工作
```

4. 更新 apt 包索引，然后安装 kubectl

```bash
sudo apt-get update
sudo apt-get install -y kubectl
```

## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。


## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。## Kubectl命令
### 语法
```bash
kubectl [command] [TYPE] [NAME] [flags]
```
- command：指定要对一个或多个资源执行的操作，例如 create、get、describe、delete。
- TYPE：指定资源类型。资源类型不区分大小写， 可以指定单数、复数或缩写形式。
- NAME：指定资源的名称。名称区分大小写。 如果省略名称，则显示所有资源的详细信息。
- flags：指定可选的参数。例如，可以使用 -s 或 --server 参数指定 Kubernetes API 服务器的地址和端口。
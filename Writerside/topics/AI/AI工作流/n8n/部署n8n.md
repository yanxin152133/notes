# 部署n8n

## docker

部署命令如下：

```Bash
docker run -it \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 -e N8N_SECURE_COOKIE=false \             # 外部访问
 -e GENERIC_TIMEZONE="Asia/Shanghai" \    # 要指定 n8n 应使用的时区，可以设置环境变量GENERIC_TIMEZONE。此变量生效的一个例子是“调度”节点。
 -e TZ="Asia/Shanghai" \                  # 系统时区可以通过环境变量单独设置TZ
 docker.n8n.io/n8nio/n8n \
 start --tunnel
```

访问路径：http://localhost:5678/setup

![n8n访问页面](img/n8n访问页面.png){ thumbnail="true" }

## 使用npm安装

> 从 n8n 3.0 开始，基于 npm 的安装方式已被弃用。


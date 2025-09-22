# 1. Jenkins
链接：[Jenkins](https://www.jenkins.io/zh/doc/tutorials/)

## 1.1. 搭建环境
### 1.1.1. 准备工作
第一次使用 Jenkins，您需要：
    
机器要求：

- 256 MB 内存，建议大于 512 MB

- 10 GB 的硬盘空间（用于 Jenkins 和 Docker 镜像）

需要安装以下软件：
     
- Java 8 ( JRE 或者 JDK 都可以)

- Docker
    
### 1.1.2. 下载并运行Jenkins
1. 下载 Jenkins.
2. 打开终端进入到下载目录.
3. 运行命令 java -jar jenkins.war --httpPort=8080.
4. 打开浏览器进入链接 http://localhost:8080.
5. 按照说明完成安装.

### 1.1.3. docker 搭建 Jenkins 环境
```bash
# 注意文件权限
mkdir /home/jenkins

docker run --name jenkins --restart always -it -d -p 8080:8080 -p 50000:50000 -v /home/jenkins:/var/jenkins_home jenkins/jenkins:latest

```

admin用户密码位置：`/var/jenkins_home/secrets/initialAdminPassword`

# 1. Azkaban3.x编译及部署
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
        
>Azkaban：3.70.0
      
## 1.1. Azkaban源码编译
### 1.1.1. 下载
Azkaban 的源码托管在 GitHub 上，地址为 https://github.com/azkaban/azkaban。
      
```bash
[root@localhost usr]# wget https://github.com/azkaban/azkaban/archive/3.70.0.tar.gz
```
       
### 1.1.2. 解压
```bash
[root@localhost usr]# tar -zxvf 3.70.0.tar.gz -C /usr/app/
```
     
### 1.1.3. 编译环境
#### 1.1.3.1. jdk
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)
  
#### 1.1.3.2. Gradle
##### 1.1.3.2.1. 查看所需的Gradle版本
```bash
[root@localhost app]# cd azkaban-3.70.0/
[root@localhost azkaban-3.70.0]# cat gradle/wrapper/gradle-wrapper.properties 
#
# Copyright 2018 LinkedIn Corp.
#
# Licensed under the Apache License, Version 2.0 (the "License"); you may not
# use this file except in compliance with the License. You may obtain a copy of
# the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
# License for the specific language governing permissions and limitations under
# the License.
#

distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.6-all.zip    ## gradle版本
```
       
##### 1.1.3.2.2. 下载Gradle
```bash
[root@localhost azkaban-3.70.0]# cd gradle/wrapper/   ## 进入该目录
[root@localhost wrapper]# wget https://services.gradle.org/distributions/gradle-4.6-all.zip   ## 下载Gradle
[root@localhost wrapper]# ls   ##  查看
gradle-4.6-all.zip  gradle-wrapper.jar  gradle-wrapper.properties
```
     
##### 1.1.3.2.3. 修改配置文件
```bash
[root@localhost wrapper]# vim gradle-wrapper.properties 
```

修改内容如下：
         
```html
#
# Copyright 2018 LinkedIn Corp.
#
# Licensed under the Apache License, Version 2.0 (the "License"); you may not
# use this file except in compliance with the License. You may obtain a copy of
# the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
# License for the specific language governing permissions and limitations under
# the License.
#

distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=gradle-4.6-all.zip   ## 修改为本地的Gradle
```
      
#### 1.1.3.3. git
```bash
[root@localhost wrapper]# yum install -y git
```
         
### 1.1.4. 编译
```bash
[root@localhost wrapper]# cd /usr/app/azkaban-3.70.0/
[root@localhost azkaban-3.70.0]# ./gradlew build installDist -x test
```
          
#### 1.1.4.1. vmware ssr
1. 开启本地代理
      
![](https://live.staticflickr.com/65535/50293504713_b8cf624881_c.jpg)
       
2. 查看IP
      
![](https://live.staticflickr.com/65535/50294183471_7cbb7995e7_c.jpg)
       
3. 配置代理
       
![](https://live.staticflickr.com/65535/50293504488_d14c7d4993_c.jpg)
         
#### 1.1.4.2. 编译成功
```html
## 最后显示为BUILD SUCCESSFUL，则成功
BUILD SUCCESSFUL in 24m 6s
82 actionable tasks: 11 executed, 71 up-to-date
```
       
## 1.2. Azkaban部署模式
按照官方文档的说明，Azkaban 3.x 之后版本提供 2 种运行模式：
- solo server model(单服务模式) ：元数据默认存放在内置的 H2 数据库（可以修改为 MySQL），该模式中 webServer(管理服务器) 和 executorServer(执行服务器) 运行在同一个进程中，进程名是 AzkabanSingleServer。该模式适用于小规模工作流的调度。
- multiple-executor(分布式多服务模式) ：存放元数据的数据库为 MySQL，MySQL 应采用主从模式进行备份和容错。这种模式下 webServer 和 executorServer 在不同进程中运行，彼此之间互不影响，适合用于生产环境。

## 1.3. Solo Server 模式部署
### 1.3.1. 解压
进入以下目录，并进行执行解压操作。
      
```bash
[root@localhost /]# cd /usr/app/azkaban-3.70.0/azkaban-solo-server/build/distributions
[root@localhost distributions]# tar -zxvf azkaban-solo-server-0.1.0-SNAPSHOT.tar.gz
```
        
### 1.3.2. 修改时区配置文件
```bash
[root@localhost distributions]# cd azkaban-solo-server-0.1.0-SNAPSHOT/conf/
[root@localhost conf]# vim azkaban.properties 
```
         
修改为以下内容：
     
```html
default.timezone.id=Asia/Shanghai
```
       
### 1.3.3. commonprivate.properties
```bash
[root@localhost distributions]# cd /usr/app/azkaban-3.70.0/azkaban-solo-server/build/distributions/azkaban-solo-server-0.1.0-SNAPSHOT/plugins/jobtypes
[root@localhost jobtypes]# vim commonprivate.properties 
```
        
修改为以下内容：
           
```html
# set execute-as-user
execute.as.user=false 
memCheck.enabled=false
```
      
### 1.3.4. 启动solo-server
```bash
[root@localhost azkaban-3.70.0]# cd azkaban-solo-server/build/distributions/azkaban-solo-server-0.1.0-SNAPSHOT/
## 在根目录执行该命令，否则无法运行
[root@localhost azkaban-solo-server-0.1.0-SNAPSHOT]# bin/start-solo.sh
```
      
#### 1.3.4.1. 验证
##### 1.3.4.1.1. 进程
使用`jps`命令查看是否有`AzkabanSingleServer`进程：
      
```bash
[root@localhost azkaban-solo-server-0.1.0-SNAPSHOT]# jps
23473 Jps
21436 GradleDaemon
23405 AzkabanSingleServer
```
        
##### 1.3.4.1.2. UI界面
UI 界面访问的端口为`8081`，账号和密码都为`azkaban`。
         
![](https://live.staticflickr.com/65535/50294326381_57dd027699_k.jpg)

### 1.3.5. 使用
1. 点击`Create Project`
          
![](https://live.staticflickr.com/65535/50294476392_0e7253dd22_k.jpg)
       
2. 填写`Name`和`Description`
       
![](https://live.staticflickr.com/65535/50294325981_8b6b64cc1d_k.jpg)
       
3. 点击`Upload`
       
![](https://live.staticflickr.com/65535/50294475662_b3a992b88f_k.jpg)
        
4. 创建一个`foo.job`文件，文件内容为：
      
```html
type=command 
command=echo "hello world"
```
          
5. 上传该文件的zip格式的压缩包
      
![](https://live.staticflickr.com/65535/50294476097_5e22b737d5_k.jpg)

6. 点击`Execute Flow`
          
![](https://live.staticflickr.com/65535/50294475662_b3a992b88f_k.jpg)
       
7. 点击`execute`
       
![](https://live.staticflickr.com/65535/50293645973_6ad5b5400c_k.jpg)
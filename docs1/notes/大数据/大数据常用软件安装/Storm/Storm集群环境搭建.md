# 1. Storm集群环境搭建
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>Zookeeper：3.4.14
      
>python版本：Python-3.6.8
      
>storm：apache-storm-1.2.2.tar.gz
        
## 1.1. 规划
![](https://camo.githubusercontent.com/348b011a9c837efec5ebc6a405ca57b972c4f93c/68747470733a2f2f67697465652e636f6d2f68656962616979696e672f426967446174612d4e6f7465732f7261772f6d61737465722f70696374757265732f73746f726d2de99b86e7bea4e8a784e588922e706e67)
      
## 1.2. 前提
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)
- [Linux 下 python 安装](notes/大数据/大数据常用软件安装/基础软件/python安装.md)
- [Zookeeper集群环境搭建](notes/大数据/大数据常用软件安装/zookeeper/Zookeeper集群环境搭建.md)

## 1.3. storm
### 1.3.1. 下载
下载安装包，之后进行解压。官方下载地址：http://storm.apache.org/downloads.html
        
### 1.3.2. 解压
```bash
[root@hadoop001 usr]# tar -zxvf apache-storm-1.2.2.tar.gz -C /usr/app/
```
### 1.3.3. 配置环境变量
```bash
[root@hadoop001 apache-storm-1.2.2]# vim /etc/profile 
```
        
将以下内容添加到该文件的末尾：
       
```html
export STORM_HOME=/usr/app/apache-storm-1.2.2
export PATH=$STORM_HOME/bin:$PATH
```
       
使配置立即生效。
     
```bash
[root@hadoop001 apache-storm-1.2.2]# source /etc/profile
```
        
### 1.3.4. 配置storm
修改 `${STORM_HOME}/conf/storm.yaml` 文件。
       
```bash
[root@hadoop001 /]# cd /usr/app/apache-storm-1.2.2/conf/
[root@hadoop001 conf]# vim storm.yaml 
```
          
修改内容如下：
        
```html
# Zookeeper集群的主机列表
storm.zookeeper.servers:
     - "hadoop001"
     - "hadoop002"
     - "hadoop003"

# Nimbus的节点列表
nimbus.seeds: ["hadoop001","hadoop002"]

# Nimbus和Supervisor需要使用本地磁盘上来存储少量状态（如jar包，配置文件等）
storm.local.dir: "/home/storm"

# workers进程的端口，每个worker进程会使用一个端口来接收消息
supervisor.slots.ports:
     - 6700
     - 6701
     - 6702
     - 6703
```
           
`supervisor.slots.ports`参数用来配置`workers`进程接收消息的端口，默认每个`supervisor`节点上会启动`4`个`worker`，当然你也可以按照自己的需要和服务器性能进行设置，假设只想启动`2`个`worker`的话，此处配置`2`个端口即可。
          
### 1.3.5. 分发安装包
```bash
[root@hadoop001 conf]# scp -r /usr/app/apache-storm-1.2.2/ hadoop002:/usr/app/
[root@hadoop001 conf]# scp -r /usr/app/apache-storm-1.2.2/ hadoop003:/usr/app/
```
         
**hadoop002、hadoop003记得配置环境变量**
**hadoop002、hadoop003记得配置环境变量**
**hadoop002、hadoop003记得配置环境变量**

## 1.4. 启动集群
### 1.4.1. zookeeper
```bash
# 在hadoop001、hadoop002、hadoop003上分别执行
[root@hadoop00X /]# zkServer.sh start
```
       
### 1.4.2. storm
在`${STORM_HOME}/bin`路径下执行：
     
hadoop001:
      
```bash
[root@hadoop001 bin]# nohup sh storm nimbus &       ## 启动主节点nimbus
[root@hadoop001 bin]# nohup sh storm supervisor &   ## 启动从节点supervisor
[root@hadoop001 bin]# nohup sh storm ui &           ## 启动UI界面
[root@hadoop001 bin]# nohup sh storm logviewer &    ## 启动日志查看服务
```
       
hadoop002:
      
```bash
[root@hadoop002 bin]# nohup sh storm nimbus &       ## 启动主节点nimbus
[root@hadoop002 bin]# nohup sh storm supervisor &   ## 启动从节点supervisor
[root@hadoop002 bin]# nohup sh storm ui &           ## 启动UI界面
[root@hadoop002 bin]# nohup sh storm logviewer &    ## 启动日志查看服务
```
      
hadoop003:
        
```bash
[root@hadoop003 bin]# nohup sh storm supervisor &   ## 启动从节点supervisor
[root@hadoop003 bin]# nohup sh storm logviewer &    ## 启动日志查看服务
```
             
## 1.5. 查看集群
### 1.5.1. 进程
```bash
## hadoop001
[root@hadoop001 bin]# jps
3777 nimbus
3909 Supervisor
4265 Jps
3722 QuorumPeerMain
4028 core
4142 logviewer

## hadoop002
[root@hadoop002 bin]# jps
2628 QuorumPeerMain
2697 nimbus
3085 config_value
2798 Supervisor
2830 core
3118 Jps

## hadoop003
[root@hadoop003 bin]# jps
2704 Supervisor
2650 QuorumPeerMain
2875 config_value
2891 Jps
```
      
### 1.5.2. UI界面
UI界面的端口为`8080`。
        
![1](https://live.staticflickr.com/65535/50278337986_7d5cc80a64_h.jpg)
        
![2](https://live.staticflickr.com/65535/50277663853_58b965ca26_h.jpg)
           
## 1.6. 关闭storm
创建一个脚本如`stop_storm.sh`，内容如下（记得执行该命令`chmod a+x stop_storm.sh`）：
          
```bash
#!/bin/bash

#nimbus节点
nimbusServers='hadoop001 hadoop002'

#supervisor节点
supervisorServers='hadoop001 hadoop002 hadoop003'

#logviewer节点
logviewerServers='hadoop001 hadoop002 hadoop003'

#停止所有的nimbus和ui
for nim in $nimbusServers
do
    echo 从节点 $nim 停止nimbus和ui...[ done ]
    ssh $nim "kill -9 `ssh $nim ps -ef | grep daemon.nimbus | awk '{print $2}'| head -n 1`" >/dev/null 2>&1
    ssh $nim "kill -9 `ssh $nim ps -ef | grep ui.core | awk '{print $2}'| head -n 1`" >/dev/null 2>&1
done

#停止所有的supervisor
for visor in $supervisorServers
do
    echo 从节点 $visor 停止supervisor...[ done ]
    ssh $visor "kill -9 `ssh $visor ps -ef | grep daemon.supervisor | awk '{print $2}'| head -n 1`" >/dev/null 2>&1
done

#停止所有的logviewer
for log in $logviewerServers
do
    echo 从节点 $log停止logviewer...[ done ]
    ssh $log "kill -9 `ssh $log ps -ef | grep daemon.logviewer | awk '{print $2}'| head -n 1`" >/dev/null 2>&1
done
```
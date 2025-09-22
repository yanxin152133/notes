# 1. Flink集群部署
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
     
>Zookeeper：3.4.14
         
>hadoop：hadoop-2.6.0-cdh5.15.2
        
>Flink：flink-1.9.1-bin-scala_2.12
    
## 1.1. 前提
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)
- [Zookeeper集群环境搭建](notes/大数据/大数据常用软件安装/zookeeper/Zookeeper集群环境搭建.md)
- [基于Zookeeper搭建Hadoop高可用集群](notes/大数据/大数据常用软件安装/Hadoop/基于Zookeeper搭建Hadoop高可用集群.md)

## 1.2. flink
### 1.2.1. 下载
- [flink](https://flink.apache.org/downloads.html)
       
### 1.2.2. 解压
```bash
[root@hadoop001 usr]# tar -zxvf flink-1.9.1-bin-scala_2.12.tgz -C /usr/app/
```
       
### 1.2.3. 配置环境变量
```bash
[root@hadoop001 usr]# vim /etc/profile
```
       
将以下内容添加到末尾：
       
```html
export FLINK_HOME=/usr/app/flink-1.9.1
export PATH=$FLINK_HOME/bin:$PATH
```
         
### 1.2.4. 添加Hadoop组件包
在官网的下载页面找到以下内容，点击下载。并将下载的文件上传到`/usr/app/flink-1.9.1/lib`路径下。
       
![](https://camo.githubusercontent.com/dc7293eb963224390bc2963ace2a357cfcac8991/68747470733a2f2f67697465652e636f6d2f68656962616979696e672f426967446174612d4e6f7465732f7261772f6d61737465722f70696374757265732f666c696e6b2d6f7074696f6e616c2d636f6d706f6e656e74732e706e67)
         
### 1.2.5. 配置flink
进入到`/usr/app/flink-1.9.1/conf`路径下。对以下文件进行修改
     
#### 1.2.5.1. masters
对`masters`文件进行修改：
      
```html
hadoop001:8081
hadoop002:8081
```
         
#### 1.2.5.2. slaves
对`slaves`文件进行修改：
      
```html
hadoop002
hadoop003
```
       
#### 1.2.5.3. flink-conf.yaml
对``flink-conf.yaml`文件添加以下内容：
      
```html
# 配置使用zookeeper来开启高可用模式
high-availability: zookeeper
# 配置zookeeper的地址，采用zookeeper集群时，可以使用逗号来分隔多个节点地址
high-availability.zookeeper.quorum: hadoop001:2181,hadoop002:2181,hadoop003:2181
# 在zookeeper上存储flink集群元信息的路径
high-availability.zookeeper.path.root: /flink
# 集群id
high-availability.cluster-id: /standalone_cluster_one
# 持久化存储JobManager元数据的地址，zookeeper上存储的只是指向该元数据的指针信息
high-availability.storageDir: hdfs://hadoop001:8020/flink/recovery
```
          
### 1.2.6. 分发安装包
```bash
[root@hadoop001 conf]# scp -r /usr/app/flink-1.9.1/ hadoop002:/usr/app/
[root@hadoop001 conf]# scp -r /usr/app/flink-1.9.1/ hadoop003:/usr/app/
```
        
## 1.3. 启动
1. 启动zookeeper
2. 启动Hadoop
3. 启动flink
      
### 1.3.1. 启动flink
```bash
[root@hadoop001 hadoop]# cd /usr/app/flink-1.9.1/bin/
[root@hadoop001 bin]# ./start-cluster.sh
Starting HA cluster with 2 masters.
Starting standalonesession daemon on host hadoop001.
Starting standalonesession daemon on host hadoop002.
Starting taskexecutor daemon on host hadoop002.
Starting taskexecutor daemon on host hadoop003.
```
         
## 1.4. 验证集群是否启动
### 1.4.1. 相关进程
```bash
[root@hadoop001 conf]# jps
6963 Jps
2996 QuorumPeerMain
5190 StandaloneSessionClusterEntrypoint
3147 NameNode
3467 JournalNode
3260 DataNode
3647 DFSZKFailoverController


[root@hadoop002 sbin]# jps
4832 StandaloneSessionClusterEntrypoint
2706 QuorumPeerMain
2851 DataNode
2932 JournalNode
5285 TaskManagerRunner
3016 DFSZKFailoverController
2782 NameNode
5854 Jps


[root@hadoop003 sbin]# jps
4643 Jps
4344 TaskManagerRunner
2858 JournalNode
2717 QuorumPeerMain
3679 ResourceManager
```
        
### 1.4.2. UI界面
Flink 提供了 WEB 界面用于直观的管理 Flink 集群，访问端口为`8081`：
![flink UI](https://live.staticflickr.com/65535/50269817503_91f4d3aa6c_h.jpg)
          
## 1.5. 作业提交
```bash
[root@hadoop001 flink-1.9.1]# yum -y install nc
[root@hadoop001 flink-1.9.1]# nc -lk 9000    ## 监听9000端口
[root@hadoop001 flink-1.9.1]# flink run examples/streaming/SocketWindowWordCount.jar --hostname hadoop001 --port 9000
```
        
具体步骤如以下：    
     
![](https://live.staticflickr.com/65535/50270860982_c42de2263b_h.jpg)
       
![](https://live.staticflickr.com/65535/50270687266_ce18ce5dec_h.jpg)
        
![](https://live.staticflickr.com/65535/50270687206_f376fbaf1a_h.jpg)
        
## 1.6. 关于hadoop001的namenode不是active状态的问题
解决方法：
        
```bash
hdfs haadmin -DFSHAAdmin -failover nn2 nn1
```
### 1.6.1. hdfs haadmin命令
1、-transitionToActive <namenode id>与-transitionToStandbyl <namenode id>：将指定的namenode ID切换为Active或者standby。这个指令并不会触发“fencing method”，所以不常用，我们通常使用"hdfs haadmin -failover"来切换Namenode状态。
     
2、-failover [--forcefence] [--foreactive] <serviceId-fist> <serviceId-second>：在两个Namenode之间failover。这个指令会触发将first节点failover到second节点。如果first处于standby，那么只是简单的将second提升为Active。如果first为Active，那么将会友好的将其切换为standby，如果失败，那么fencing methods将会触发直到成功，此后second将会提升为Active。如果fencing method失败，那么second将不会被提升为Active。
       
例如："hdfs haadmin -DFSHAAdmin -failover nn1 nn2"
       
3、-getServiceState <serviceId>：获取serviceId的状态，Active还是Standby。链接到指定的namenode上，并获取其当前的状态，打印出“standby”或者“active”。我可以在crontab中使用此命令，用来监测各个Namenode的状况。
        
4、-checkHealth <serviceId>：检测指定的namenode的健康状况。
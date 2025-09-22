# 1. 基于Zookeeper搭建Spark高可用集群
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>Zookeeper：3.4.14
         
>hadoop：hadoop-2.6.0-cdh5.15.2
        
>spark：spark-2.4.6-bin-hadoop2.6
      
## 1.1. 规划
![](https://camo.githubusercontent.com/b402b1ec1a97f9e7aba7fa796ec798ed622296b9/68747470733a2f2f67697465652e636f6d2f68656962616979696e672f426967446174612d4e6f7465732f7261772f6d61737465722f70696374757265732f737061726be99b86e7bea4e8a784e588922e706e67)
      
## 1.2. JDK安装
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)

在三台虚拟机上分别`对应地`执行以下命令:
       
```bash
## hadoop001
[root@hadoop001 ~]$ hostnamectl set-hostname hadoop001

## hadoop002
[root@hadoop002 ~]$ hostnamectl set-hostname hadoop002

## hadoop003
[root@hadoop003 ~]$ hostnamectl set-hostname hadoop003

```
          
## 1.3. 添加节点
在三台虚拟机上`都`执行以下命令：    
      
```bash
[root@hadoop001 hadoop]# vim /etc/hosts
```
          
在该文件后面`添加`以下内容:   
       
```html
192.168.31.132 hadoop001
192.168.31.133 hadoop002
192.168.31.134 hadoop003
```
         
>192.168.31.132 hadoop001         
192.168.31.133 hadoop002           
192.168.31.134 hadoop003           
分别对应三台虚拟机设置的静态ip和hostname
           
## 1.4. ssh免密登录
执行以下命令：     
          
```bash
[root@hadoop001 hadoop]# ssh localhost              ## 该命令用于生成隐藏目录.ssh
[root@hadoop001 hadoop]# cd ~/.ssh
[root@hadoop001 hadoop]# ssh-keygen -t rsa #遇到提示一路回车就行
[root@hadoop001 hadoop]# ll #会看到 id_rsa id_rsa.pub 两文件前为私钥，后为公钥
[root@hadoop001 hadoop]# cat id_rsa.pub >> authorized_keys #把公钥内容追加到authorized_keys文件中
[root@hadoop001 hadoop]# chmod 600 authorized_keys #修改文件权限，重要不要忽略
```
          
将密钥分发给其他两台虚拟机：      
           
```bash
[root@hadoop001 hadoop]# cd ~/.ssh
[root@hadoop001 hadoop]# scp * hadoop002:~/.ssh
[root@hadoop001 hadoop]# scp * hadoop003:~/.ssh
```
           
### 1.4.1. 验证是否可以免密登录
在`hostname为hadoop001`上执行：
           
```bash
[root@hadoop001 hadoop]# ssh hadoop002
[root@hadoop001 hadoop]# ssh hadoop003
```
         
在`hostname为hadoop002`上执行：
           
```bash
[root@hadoop002 hadoop]# ssh hadoop001
[root@hadoop002 hadoop]# ssh hadoop003
```
           
在`hostname为hadoop003`上执行：
           
```bash
[root@hadoop003 hadoop]# ssh hadoop002
[root@hadoop003 hadoop]# ssh hadoop001
```
          
## 1.5. Zookeeper
### 1.5.1. 下载
```bash
[root@hadoop001]# cd /usr 
[root@hadoop001 usr]# wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
```
          
### 1.5.2. 解压
```bash
[root@hadoop001 usr]# tar -zxvf zookeeper-3.4.14.tar.gz
[root@hadoop001 usr]# mv zookeeper-3.4.14 /usr/app
```
     
### 1.5.3. 环境变量
```
[root@hadoop001 usr]# vim /etc/profile
```
          
在该文件最后添加以下内容：
          
```html
export ZOOKEEPER_HOME=/usr/app/zookeeper-3.4.14
export PATH=$ZOOKEEPER_HOME/bin:$PATH
```
          
### 1.5.4. 修改配置
```bash
[root@hadoop001 usr]# cd /usr/app/zookeeper-3.4.14/conf
```
          
修改为以下内容：    
        
```html
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/zookeeper-cluster/data/
dataLogDir=/usr/local/zookeeper-cluster/log/
clientPort=2181

# server.1 这个1是服务器的标识，可以是任意有效数字，标识这是第几个服务器节点，这个标识要写到dataDir目录下面myid文件里
# 指名集群间通讯端口和选举端口
server.1=hadoop001:2287:3387
server.2=hadoop002:2287:3387
server.3=hadoop003:2287:3387
```
           
配置参数说明：
    
- tickTime：用于计算的基础时间单元。比如 session 超时：N*tickTime；
- initLimit：用于集群，允许从节点连接并同步到 master 节点的初始化连接时间，以 tickTime 的倍数来表示；
- syncLimit：用于集群， master 主节点与从节点之间发送消息，请求和应答时间长度（心跳机制）；
- dataDir：数据存储位置；
- dataLogDir：日志目录；
- clientPort：用于客户端连接的端口，默认 2181
          
### 1.5.5. 标识节点
执行以下命令：        
```bash
# 三台虚拟机都执行
mkdir -vp  /usr/local/zookeeper-cluster/data/


# hadoop001主机
echo "1" > /usr/local/zookeeper-cluster/data/myid
# hadoop002主机
echo "2" > /usr/local/zookeeper-cluster/data/myid
# hadoop003主机
echo "3" > /usr/local/zookeeper-cluster/data/myid
```
           
### 1.5.6. 启动zookeeper
启动命令为：    
      
```bash
/usr/app/zookeeper-3.4.14/bin/zkServer.sh start
```
          
### 1.5.7. 验证
查看集群各节点状态的命令为：  
```bash
/usr/app/zookeeper-3.4.14/bin/zkServer.sh status
```
           
如下所示：三个节点进程均启动成功，并且 hadoop001 为 leader 节点，hadoop002 和 hadoop003 为 follower 节点。
            
```bash
[root@hadoop001 conf]# /usr/app/zookeeper-3.4.14/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /usr/app/zookeeper-3.4.14/bin/../conf/zoo.cfg
Mode: leader
```


```bash
[root@hadoop002 data]# /usr/app/zookeeper-3.4.14/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /usr/app/zookeeper-3.4.14/bin/../conf/zoo.cfg
Mode: follower
```


```bash
[root@hadoop003 data]# /usr/app/zookeeper-3.4.14/bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /usr/app/zookeeper-3.4.14/bin/../conf/zoo.cfg
Mode: follower
```

### 1.5.8. zookeeper日志文件路径
1、修改`$ZOOKEEPER_HOME/bin`目录下的`zkEnv.sh`文件，`ZOO_LOG_DIR`指定想要输出到哪个目录，`ZOO_LOG4J_PROP`，指定`INFO,ROLLINGFILE`的日志APPENDER。
2、修改`$ZOOKEEPER_HOME/conf/log4j.properties`文件的：`zookeeper.root.logger`的值与前一个文件的`ZOO_LOG4J_PROP`保持一致，该日志配置是以日志文件大小轮转的，如果想要按照天轮转，可以修改为`DaliyRollingFileAppender`。
              
### 1.5.9. zookeeper无法启动问题
- 端口占用
- 防火墙
        
## 1.6. Hadoop
### 1.6.1. 下载
下载 Hadoop。这里我下载的是 CDH 版本 Hadoop，下载地址为：http://archive.cloudera.com/cdh5/cdh/5/。在链接后面加上自己下载版本即可下载。
                 
### 1.6.2. 上传
```bash
$ scp hadoop-2.6.0-cdh5.15.2.tar.gz root@192.168.31.132:/usr/app
```
### 1.6.3. 解压
```bash
[root@hadoop001 /]# cd /usr/app
[root@hadoop001 app]# tar -zvxf hadoop-2.6.0-cdh5.15.2.tar.gz 
```
         
### 1.6.4. 配置环境变量
```bash
[root@hadoop001 app]# vim /etc/profile
```
          
将以下内容添加到该文件最后即可：
        
```html
export HADOOP_HOME=/usr/app/hadoop-2.6.0-cdh5.15.2
export PATH=${HADOOP_HOME}/bin:$PATH
```

使配置生效：      

```bash
[root@hadoop001 app]# source /etc/profile
```
           
### 1.6.5. 配置hadooop
进入`/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop`下，对以下几个文件进行修改。
         
#### 1.6.5.1. hadoop-env.sh
```html
# 指定JDK的安装位置
export JAVA_HOME=/usr/jdk1.8.0_261
```

#### 1.6.5.2. core-site.xml
```html
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->
<configuration>
    <property>
        <!-- 指定 namenode 的 hdfs 协议文件系统的通信地址 -->
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop001:8020</value>
    </property>
    <property>
        <!-- 指定 hadoop 集群存储临时文件的目录 -->
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/tmp</value>
    </property>
    <property>
        <!-- ZooKeeper 集群的地址 -->
        <name>ha.zookeeper.quorum</name>
        <value>hadoop001:2181,hadoop002:2181,hadoop002:2181</value>
    </property>
    <property>
        <!-- ZKFC 连接到 ZooKeeper 超时时长 -->
        <name>ha.zookeeper.session-timeout.ms</name>
        <value>10000</value>
    </property>
</configuration>

```
         
#### 1.6.5.3. hdfs-site.xml
```html
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->
<configuration>
    <property>
        <!-- 指定 HDFS 副本的数量 -->
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <!-- namenode 节点数据（即元数据）的存放位置，可以指定多个目录实现容错，多个目录用逗号分隔 -->
        <name>dfs.namenode.name.dir</name>
        <value>/home/hadoop/namenode/data</value>
    </property>
    <property>
        <!-- datanode 节点数据（即数据块）的存放位置 -->
        <name>dfs.datanode.data.dir</name>
        <value>/home/hadoop/datanode/data</value>
    </property>
    <property>
        <!-- 集群服务的逻辑名称 -->
        <name>dfs.nameservices</name>
        <value>mycluster</value>
    </property>
    <property>
        <!-- NameNode ID 列表-->
        <name>dfs.ha.namenodes.mycluster</name>
        <value>nn1,nn2</value>
    </property>
    <property>
        <!-- nn1 的 RPC 通信地址 -->
        <name>dfs.namenode.rpc-address.mycluster.nn1</name>
        <value>hadoop001:8020</value>
    </property>
    <property>
        <!-- nn2 的 RPC 通信地址 -->
        <name>dfs.namenode.rpc-address.mycluster.nn2</name>
        <value>hadoop002:8020</value>
    </property>
    <property>
        <!-- nn1 的 http 通信地址 -->
        <name>dfs.namenode.http-address.mycluster.nn1</name>
        <value>hadoop001:50070</value>
    </property>
    <property>
        <!-- nn2 的 http 通信地址 -->
        <name>dfs.namenode.http-address.mycluster.nn2</name>
        <value>hadoop002:50070</value>
    </property>
    <property>
        <!-- NameNode 元数据在 JournalNode 上的共享存储目录 -->
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://hadoop001:8485;hadoop002:8485;hadoop003:8485/mycluster</value>
    </property>
    <property>
        <!-- Journal Edit Files 的存储目录 -->
        <name>dfs.journalnode.edits.dir</name>
        <value>/home/hadoop/journalnode/data</value>
    </property>
    <property>
        <!-- 配置隔离机制，确保在任何给定时间只有一个 NameNode 处于活动状态 -->
        <name>dfs.ha.fencing.methods</name>
        <value>sshfence</value>
    </property>
    <property>
        <!-- 使用 sshfence 机制时需要 ssh 免密登录 -->
        <name>dfs.ha.fencing.ssh.private-key-files</name>
        <value>/root/.ssh/id_rsa</value>
    </property>
    <property>
        <!-- SSH 超时时间 -->
        <name>dfs.ha.fencing.ssh.connect-timeout</name>
        <value>30000</value>
    </property>
    <property>
        <!-- 访问代理类，用于确定当前处于 Active 状态的 NameNode -->
        <name>dfs.client.failover.proxy.provider.mycluster</name>
        <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>
    <property>
        <!-- 开启故障自动转移 -->
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>
</configuration>

```
           
#### 1.6.5.4. yarn-site.xml
```html
<?xml version="1.0"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->
<configuration>
    <property>
        <!--配置 NodeManager 上运行的附属服务。需要配置成 mapreduce_shuffle 后才可以在 Yarn 上运行 MapReduce 程序。-->
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <!-- 是否启用日志聚合 (可选) -->
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>
    <property>
        <!-- 聚合日志的保存时间 (可选) -->
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>86400</value>
    </property>
    <property>
        <!-- 启用 RM HA -->
        <name>yarn.resourcemanager.ha.enabled</name>
        <value>true</value>
    </property>
    <property>
        <!-- RM 集群标识 -->
        <name>yarn.resourcemanager.cluster-id</name>
        <value>my-yarn-cluster</value>
    </property>
    <property>
        <!-- RM 的逻辑 ID 列表 -->
        <name>yarn.resourcemanager.ha.rm-ids</name>
        <value>rm1,rm2</value>
    </property>
    <property>
        <!-- RM1 的服务地址 -->
        <name>yarn.resourcemanager.hostname.rm1</name>
        <value>hadoop002</value>
    </property>
    <property>
        <!-- RM2 的服务地址 -->
        <name>yarn.resourcemanager.hostname.rm2</name>
        <value>hadoop003</value>
    </property>
    <property>
        <!-- RM1 Web 应用程序的地址 -->
        <name>yarn.resourcemanager.webapp.address.rm1</name>
        <value>hadoop002:8088</value>
    </property>
    <property>
        <!-- RM2 Web 应用程序的地址 -->
        <name>yarn.resourcemanager.webapp.address.rm2</name>
        <value>hadoop003:8088</value>
    </property>
    <property>
        <!-- ZooKeeper 集群的地址 -->
        <name>yarn.resourcemanager.zk-address</name>
        <value>hadoop001:2181,hadoop002:2181,hadoop003:2181</value>
    </property>
    <property>
        <!-- 启用自动恢复 -->
        <name>yarn.resourcemanager.recovery.enabled</name>
        <value>true</value>
    </property>
    <property>
        <!-- 用于进行持久化存储的类 -->
        <name>yarn.resourcemanager.store.class</name>
        <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
    </property>
</configuration>

```
            
#### 1.6.5.5. mapred-site.xml
```bash
[root@hadoop001 hadoop]# cp mapred-site.xml.template mapred-site.xml
```
再进行修改其文件内容：
          
```html
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->
<configuration>
    <property>
        <!--指定 mapreduce 作业运行在 yarn 上-->
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
           
#### 1.6.5.6. slaves
```html
hadoop001
hadoop002
hadoop003
```
         
#### 1.6.5.7. 配置hadoop002和hadoop003
```bash
[root@hadoop001 hadoop]# scp -r /usr/app/hadoop-2.6.0-cdh5.15.2/ hadoop002:/usr/app/
[root@hadoop001 hadoop]# scp -r /usr/app/hadoop-2.6.0-cdh5.15.2/ hadoop003:/usr/app/
```

**记得配置hadoop环境变量**
**记得配置hadoop环境变量**
**记得配置hadoop环境变量**
                 
### 1.6.6. 启动
#### 1.6.6.1. zookeeper
由于配置了zookeeper环境变量，所以直接运行以下命令。
         
```bash
[root@hadoop001 hadoop]# zkServer.sh start
```
            
#### 1.6.6.2. 启动Journalnode
分别到三台服务器的的 `${HADOOP_HOME}/sbin `目录下，启动 journalnode 进程：
       
```bash
[root@hadoop001 sbin]# ./hadoop-daemon.sh start journalnode
```
            
#### 1.6.6.3. 初始化NameNode
在 `hadop001` 上执行 NameNode 初始化命令：
           
```bash
[root@hadoop001 sbin]# hdfs namenode -format
```
         
#### 1.6.6.4. hadoop002的namenode
```bash
[root@hadoop001 sbin]# scp -r /home/hadoop/namenode/data hadoop002:/home/hadoop/namenode/
```
         
**记得查看`hadoop002:/home/hadoop/namenode/`下是否有`/data`该目录。**
            
#### 1.6.6.5. 初始化HA状态
在任意一台 NameNode 上使用以下命令来初始化 ZooKeeper 中的 HA 状态：
       
```bash
[root@hadoop001 sbin]# hdfs zkfc -formatZK
```
          
#### 1.6.6.6. 启动HDFS
```bash
[root@hadoop001 sbin]# ./start-dfs.sh
```
          
#### 1.6.6.7. 启动YARN
进入到 `hadoop002` 的 `${HADOOP_HOME}/sbin` 目录下，启动 YARN。
         
```bash
[root@hadoop002 sbin]# ./start-yarn.sh
```

需要注意的是，这个时候 `hadoop003` 上的 `ResourceManager` 服务通常是没有启动的，需要手动启动： 
           
```bash
[root@hadoop003 sbin]# ./yarn-daemon.sh start resourcemanager
```
          
###  1.6.7. 查看集群
#### 1.6.7.1. 查看进程
成功启动后，每台服务器上的进程应该如下：
          
```bash
[root@hadoop001 sbin]# jps
2226 QuorumPeerMain
5411 Jps
4293 DFSZKFailoverController
3782 NameNode
3926 DataNode
4502 NodeManager
3468 JournalNode



[root@hadoop002 sbin]# jps
3600 JournalNode
3922 DataNode
5746 Jps
3800 NameNode
4219 ResourceManager
2252 QuorumPeerMain
4124 DFSZKFailoverController
4317 NodeManager



[root@hadoop003 sbin]# jps
3538 JournalNode
4754 Jps
3877 NodeManager
4027 ResourceManager
3661 DataNode
2206 QuorumPeerMain
```
             
#### 1.6.7.2. 查看Web UI
HDFS 和 YARN 的端口号分别为 50070 和 8080，界面应该如下：
##### 1.6.7.2.1. hadoop001
![](https://live.staticflickr.com/65535/50236634646_6ba3759258_b.jpg>)
![](https://live.staticflickr.com/65535/50236850732_a98d1fd42b_b.jpg)
![](https://live.staticflickr.com/65535/50236850767_887e558167_b.jpg)
         
#### 1.6.7.3. hadoop002
![](https://live.staticflickr.com/65535/50235984213_c807cd69a8_b.jpg)
![](https://live.staticflickr.com/65535/50236634176_ba79583836_b.jpg)
![](https://live.staticflickr.com/65535/50236634591_06fe6d004f_b.jpg)
![](https://live.staticflickr.com/65535/50236850657_d0ebb31cc7_b.jpg)
#### 1.6.7.4. hadoop003
![](https://live.staticflickr.com/65535/50235984148_3bc276ba79_b.jpg)
             
### 1.6.8. 二次启动
```bash
## 首先先启动zookeeper
[root@hadoop001 sbin]# ./start-dfs.sh
[root@hadoop002 sbin]# ./start-yarn.sh
[root@hadoop003 sbin]# ./yarn-daemon.sh start resourcemanager
```
     
## 1.7. spark
### 1.7.1. 下载
官网下载地址：[http://spark.apache.org/downloads.html](http://spark.apache.org/downloads.html)
       
![spark](https://live.staticflickr.com/65535/50246803513_6f2eea897f_h.jpg)
      
### 1.7.2. 上传
```bash
scp spark-2.4.6-bin-hadoop2.6.tgz root@192.168.31.132:/usr
```
     
### 1.7.3. 解压
```bash
[root@hadoop001 usr]# tar -zxvf spark-2.4.6-bin-hadoop2.6.tgz
[root@hadoop001 usr]# mv spark-2.4.6-bin-hadoop2.6 /usr/app/
```
        
### 1.7.4. 配置环境变量
```bash
[root@hadoop001 app]# vim /etc/profile
```
      
将以下内容加入该文件的末尾：      
     
```html
export SPARK_HOME=/usr/app/spark-2.4.6-bin-hadoop2.6
export PATH=${SPARK_HOME}/bin:$PATH
```
       
使配置立即生效。
       
```bash
[root@hadoop001 app]# source /etc/profile
```
      
## 1.8. 配置spark
```bash
[root@hadoop001 /]# cd /usr/app/spark-2.4.6-bin-hadoop2.6/conf/
```
        
### 1.8.1. spark-env.sh
```bash
[root@hadoop001 conf]# cp spark-env.sh.template spark-env.sh
[root@hadoop001 conf]# vim spark-env.sh
```
       
添加以下内容：   
    
```html
# 配置JDK安装位置
JAVA_HOME=/usr/jdk1.8.0_261
# 配置hadoop的位置
HADOOP_HOME=/usr/app/hadoop-2.6.0-cdh5.15.2
# 配置hadoop配置文件的位置
HADOOP_CONF_DIR=/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
# 配置zookeeper地址
SPARK_DAEMON_JAVA_OPTS="-Dspark.deploy.recoveryMode=ZOOKEEPER -Dspark.deploy.zookeeper.url=hadoop001:2181,hadoop002:2181,hadoop003:2181 -Dspark.deploy.zookeeper.dir=/spark"
```
       
### 1.8.2. slaves
```bash
[root@hadoop001 conf]# cp slaves.template slaves
[root@hadoop001 conf]# vim slaves
```
       
修改为以下内容：    
      
```html
hadoop001
hadoop002
hadoop003
```
      
### 1.8.3. spark-defaults.conf（可选）
```bash
[root@hadoop001 conf]# cp spark-defaults.conf.template spark-defaults.conf
[root@hadoop001 conf]# vim spark-defaults.conf
```
       
修改为以下内容：
      
```html
spark.master                     spark://master:7077     ## 可选
spark.eventLog.enabled           true       ## 可选
spark.eventLog.dir               hdfs://mycluster/spark/eventlog  ## 可选,可以配置为active的namenode节点如hdfs://hadoop:8020/mycluster/spark/eventlog
spark.yarn.jars                  hdfs://mycluster/spark/lib_jars/*.jar  ## 可选，该项可以省略上传jar到hdfs的步骤，节省时间。可以配置为active的namenode节点如hdfs://hadoop:8020/spark/lib_jars/*.jar
spark.yarn.preserve.staging.files false ## 可选
```
      
### 1.8.4. 分发安装包
```bash
[root@hadoop001 conf]# scp -r /usr/app/spark-2.4.6-bin-hadoop2.6/ hadoop002:/usr/app/
[root@hadoop001 conf]# scp -r /usr/app/spark-2.4.6-bin-hadoop2.6/ hadoop003:/usr/app/
```
       
**hadoop002和hadoop003记得配置环境变量**
**hadoop002和hadoop003记得配置环境变量**
**hadoop002和hadoop003记得配置环境变量**
          
## 1.9. 启动spark
### 1.9.1. 启动zookeeper
在三台虚拟机上分别运行以下命令：  
     
```bash
[root@hadoop001 /]# zkServer.sh start
[root@hadoop002 /]# zkServer.sh start
[root@hadoop003 /]# zkServer.sh start
```
          
### 1.9.2. 启动Hadoop
hadoop001:    

```bash
[root@hadoop001 /]# cd /usr/app/hadoop-2.6.0-cdh5.15.2/sbin/
[root@hadoop001 sbin]# ./start-dfs.sh 
```
          
hadoop002:
       
```bash
[root@hadoop002 /]# cd /usr/app/hadoop-2.6.0-cdh5.15.2/sbin/
[root@hadoop002 sbin]# ./start-yarn.sh
```
        
hadoop003:
      
```bash
[root@hadoop003 /]# cd /usr/app/hadoop-2.6.0-cdh5.15.2/sbin/
[root@hadoop003 sbin]# ./yarn-daemon.sh start resourcemanager
```
        
#### 1.9.2.1. hdfs相关设置（若配置了spark-defaults.conf就需要该步骤）
```bash
## 不确定哪个为active的namenode，所以就是用 hdfs://mycluster/path
[root@hadoop001 sbin]# hdfs dfs -mkdir hdfs://mycluster/spark/eventlog
[root@hadoop001 sbin]# hdfs dfs -mkdir hdfs://mycluster/spark/lib_jars
[root@hadoop001 sbin]# hdfs dfs -put /usr/app/spark-2.4.6-bin-hadoop2.6/jars/* hdfs://myclusterspark/lib_jars/
```
      
可以通过`hadoop:50070`查看自己创建的目录和上传的文件是否存在。
      
### 1.9.3. 启动spark
hadoop001:
       
```bash
[root@hadoop001 sbin]# cd /usr/app/spark-2.4.6-bin-hadoop2.6/sbin/
[root@hadoop001 sbin]# ./start-all.sh
```
        
hadoop002:
      
```bash
[root@hadoop002 hadoop]# cd /usr/app/spark-2.4.6-bin-hadoop2.6/sbin/
[root@hadoop002 sbin]# ./start-master.sh 
```
        
hadoop003:
       
```bash
[root@hadoop003 hadoop]# cd /usr/app/spark-2.4.6-bin-hadoop2.6/sbin/
[root@hadoop003 sbin]# ./start-master.sh 
```
           
#### 1.9.3.1. 查看服务
查看 Spark 的 Web-UI 页面，端口为 8080。此时可以看到 hadoop001 上的 Master 节点处于 ALIVE 状态，并有 3 个可用的 Worker 节点。
        
hadoop001:(在虚拟机上直接访问`hadoop001:8080`)
         
![](https://live.staticflickr.com/65535/50247710646_95aa6c8f13_h.jpg)
          
hadoop002:(在虚拟机上直接访问`hadoop002:8080`)
         
![](https://live.staticflickr.com/65535/50247710476_24fdf70616_h.jpg)
         
hadoop002:(在虚拟机上直接访问`hadoop003:8080`)
         
![](https://live.staticflickr.com/65535/50247913457_6d9d47640b_h.jpg)
          
#### 1.9.3.2. 验证集群高可用
此时可以使用`kill`命令杀死`hadoop001`上的`Master`进程，此时备用`Master`会中会有一个再次成为`主 Master`。过程省略。。。
         
       
### 1.9.4. 提交作业
以 Spark 内置的计算 Pi 的样例程序为例，提交命令如下：
     
```bash
## 根据文章的步骤需要保证hadoop001的namenode为active状态，以下命令才可以运行成功。 
## 若hadoop002为active时spark不知道为什么不能使用hadoop002
## 所以碰到ApplicationMaster: Failed to cleanup staging dir .sparkStaging/application_xxxxxx_xxxx相关的就需要将后面的节点的namenode设置为active状态

[root@hadoop001 sbin]# spark-submit \
--class org.apache.spark.examples.SparkPi \
--master yarn \
--deploy-mode client \
--executor-memory 1G \
--total-executor-cores 10 \
/usr/app/spark-2.4.6-bin-hadoop2.6/examples/jars/spark-examples_2.11-2.4.6.jar \
100
```

运行结果（部分）：
      
```html
20/08/21 18:11:28 INFO scheduler.DAGScheduler: ResultStage 0 (reduce at SparkPi.scala:38) finished in 7.143 s
20/08/21 18:11:28 INFO cluster.YarnScheduler: Removed TaskSet 0.0, whose tasks have all completed, from pool 
20/08/21 18:11:28 INFO scheduler.DAGScheduler: Job 0 finished: reduce at SparkPi.scala:38, took 7.238847 s

Pi is roughly 3.1418103141810314

20/08/21 18:11:28 INFO server.AbstractConnector: Stopped Spark@6a175569{HTTP/1.1,[http/1.1]}{0.0.0.0:4040}
20/08/21 18:11:28 INFO ui.SparkUI: Stopped Spark web UI at http://hadoop002:4040
```
        
有`Pi is roughly XXX`表示成功。
        
相关记录可以通过`hadoop002:8088`或`hadoop003:8088`看到运行情况。
         
        
## 1.10. 关于hadoop001的namenode不是active状态的问题
解决方法：
        
```bash
hdfs haadmin -DFSHAAdmin -failover nn2 nn1
```
### 1.10.1. hdfs haadmin命令
1、-transitionToActive <namenode id>与-transitionToStandbyl <namenode id>：将指定的namenode ID切换为Active或者standby。这个指令并不会触发“fencing method”，所以不常用，我们通常使用"hdfs haadmin -failover"来切换Namenode状态。
     
2、-failover [--forcefence] [--foreactive] <serviceId-fist> <serviceId-second>：在两个Namenode之间failover。这个指令会触发将first节点failover到second节点。如果first处于standby，那么只是简单的将second提升为Active。如果first为Active，那么将会友好的将其切换为standby，如果失败，那么fencing methods将会触发直到成功，此后second将会提升为Active。如果fencing method失败，那么second将不会被提升为Active。
       
例如："hdfs haadmin -DFSHAAdmin -failover nn1 nn2"
       
3、-getServiceState <serviceId>：获取serviceId的状态，Active还是Standby。链接到指定的namenode上，并获取其当前的状态，打印出“standby”或者“active”。我可以在crontab中使用此命令，用来监测各个Namenode的状况。
        
4、-checkHealth <serviceId>：检测指定的namenode的健康状况。
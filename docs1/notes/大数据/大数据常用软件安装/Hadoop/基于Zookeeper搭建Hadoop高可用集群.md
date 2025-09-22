# 1. 基于Zookeeper搭建Hadoop高可用集群
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>Zookeeper：3.4.14
         
>hadoop：hadoop-2.6.0-cdh5.15.2
         
## 1.1. 规划
![规划](https://camo.githubusercontent.com/db74bec6b51697170ec29a7c4b4750dd08993b8d07443b6e98ca6f25afa8ec56/68747470733a2f2f67697465652e636f6d2f68656962616979696e672f426967446174612d4e6f7465732f7261772f6d61737465722f70696374757265732f6861646f6f70e9ab98e58fafe794a8e99b86e7bea4e8a784e588922e706e67)

## 1.2. JDK安装
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)

## 1.3. 修改hostname
在三台虚拟机上分别`对应地`执行以下命令:

```bash
## hadoop001
[root@hadoop001 ~]$ hostnamectl set-hostname hadoop001

## hadoop002
[root@hadoop002 ~]$ hostnamectl set-hostname hadoop002

## hadoop003
[root@hadoop003 ~]$ hostnamectl set-hostname hadoop003

```

## 1.4. 添加节点
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

分别对对应三台虚拟机设置的`静态ip`和`hostname`。

## 1.5. ssh免密登录
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

### 1.5.1. 验证是否可以免密登录
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

## 1.6. zookeeper
[zookeeper](notes/大数据/大数据常用软件安装/zookeeper/Zookeeper集群环境搭建.md)

## 1.7. Hadoop
### 1.7.1. 下载
下载 Hadoop。这里我下载的是 CDH 版本 Hadoop，下载地址为：http://archive.cloudera.com/cdh5/cdh/5/。在链接后面加上自己下载版本即可下载。
                 
### 1.7.2. 上传
```bash
$ scp hadoop-2.6.0-cdh5.15.2.tar.gz root@192.168.31.132:/usr/app
```
### 1.7.3. 解压
```bash
[root@hadoop001 /]# cd /usr/app
[root@hadoop001 app]# tar -zvxf hadoop-2.6.0-cdh5.15.2.tar.gz 
```
         
### 1.7.4. 配置环境变量
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
           
### 1.7.5. 配置hadooop
进入`/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop`下，对以下几个文件进行修改。
         
#### 1.7.5.1. hadoop-env.sh
```html
# 指定JDK的安装位置
export JAVA_HOME=/usr/jdk1.8.0_261
```

#### 1.7.5.2. core-site.xml
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
         
#### 1.7.5.3. hdfs-site.xml
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
           
#### 1.7.5.4. yarn-site.xml
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
            
#### 1.7.5.5. mapred-site.xml
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
           
#### 1.7.5.6. slaves
```html
hadoop001
hadoop002
hadoop003
```
         
#### 1.7.5.7. 配置hadoop002和hadoop003
```bash
[root@hadoop001 hadoop]# scp -r /usr/app/hadoop-2.6.0-cdh5.15.2/ hadoop002:/usr/app/
[root@hadoop001 hadoop]# scp -r /usr/app/hadoop-2.6.0-cdh5.15.2/ hadoop003:/usr/app/
```

**记得配置hadoop环境变量**
**记得配置hadoop环境变量**
**记得配置hadoop环境变量**
                 
### 1.7.6. 启动
#### 1.7.6.1. zookeeper
由于配置了zookeeper环境变量，所以直接运行以下命令(三台都需要执行该命令)。
         
```bash
[root@hadoop001 hadoop]# zkServer.sh start
```
            
#### 1.7.6.2. 启动Journalnode
分别到三台服务器的的 `${HADOOP_HOME}/sbin `目录下，启动 journalnode 进程：
       
```bash
[root@hadoop001 sbin]# ./hadoop-daemon.sh start journalnode
```
            
#### 1.7.6.3. 初始化NameNode
在 `hadop001` 上执行 NameNode 初始化命令：
           
```bash
[root@hadoop001 sbin]# hdfs namenode -format
```
         
#### 1.7.6.4. hadoop002的namenode
```bash
[root@hadoop001 sbin]# scp -r /home/hadoop/namenode/data hadoop002:/home/hadoop/namenode/
```
         
**记得查看`hadoop002:/home/hadoop/namenode/`下是否有`/data`该目录。**
            
#### 1.7.6.5. 初始化HA状态
在任意一台 NameNode 上使用以下命令来初始化 ZooKeeper 中的 HA 状态：
       
```bash
[root@hadoop001 sbin]# hdfs zkfc -formatZK
```
          
#### 1.7.6.6. 启动HDFS
```bash
[root@hadoop001 sbin]# ./start-dfs.sh
```
          
#### 1.7.6.7. 启动YARN
进入到 `hadoop002` 的 `${HADOOP_HOME}/sbin` 目录下，启动 YARN。
         
```bash
[root@hadoop002 sbin]# ./start-yarn.sh
```

需要注意的是，这个时候 `hadoop003` 上的 `ResourceManager` 服务通常是没有启动的，需要手动启动： 
           
```bash
[root@hadoop003 sbin]# ./yarn-daemon.sh start resourcemanager
```
          
###  1.7.7. 查看集群
#### 1.7.7.1. 查看进程
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
             
#### 1.7.7.2. 查看Web UI
HDFS 和 YARN 的端口号分别为 50070 和 8080，界面应该如下：
##### 1.7.7.2.1. hadoop001
![](https://live.staticflickr.com/65535/50236634646_6ba3759258_b.jpg>)
![](https://live.staticflickr.com/65535/50236850732_a98d1fd42b_b.jpg)
![](https://live.staticflickr.com/65535/50236850767_887e558167_b.jpg)
         
#### 1.7.7.3. hadoop002
![](https://live.staticflickr.com/65535/50235984213_c807cd69a8_b.jpg)
![](https://live.staticflickr.com/65535/50236634176_ba79583836_b.jpg)
![](https://live.staticflickr.com/65535/50236634591_06fe6d004f_b.jpg)
![](https://live.staticflickr.com/65535/50236850657_d0ebb31cc7_b.jpg)
#### 1.7.7.4. hadoop003
![](https://live.staticflickr.com/65535/50235984148_3bc276ba79_b.jpg)
             
### 1.7.8. 二次启动
```bash
## 首先先启动zookeeper
[root@hadoop001 sbin]# ./start-dfs.sh
[root@hadoop002 sbin]# ./start-yarn.sh
[root@hadoop003 sbin]# ./yarn-daemon.sh start resourcemanager
```
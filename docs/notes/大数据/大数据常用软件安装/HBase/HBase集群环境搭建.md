# 1. HBase集群环境搭建
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>Zookeeper：3.4.14
         
>hadoop：hadoop-2.6.0-cdh5.15.2
      
>HBase：hbase-1.2.0-cdh5.15.2
         
## 1.1. 规划
![](https://camo.githubusercontent.com/b72f7a1904af343ecf5ce521abf33286332b132d/68747470733a2f2f67697465652e636f6d2f68656962616979696e672f426967446174612d4e6f7465732f7261772f6d61737465722f70696374757265732f6862617365e99b86e7bea4e8a784e588922e706e67)
        
## 1.2. 前提
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)
- [Zookeeper集群环境搭建](notes/大数据/大数据常用软件安装/zookeeper/Zookeeper集群环境搭建.md)
- [基于Zookeeper搭建Hadoop高可用集群](notes/大数据/大数据常用软件安装/Hadoop/基于Zookeeper搭建Hadoop高可用集群.md)

## 1.3. HBase
### 1.3.1. 下载
下载并解压，这里我下载的是 CDH 版本 HBase，下载地址为：http://archive.cloudera.com/cdh5/cdh/5/
      
### 1.3.2. 解压
```bash
[root@hadoop001 usr]# tar -zxvf hbase-1.2.0-cdh5.15.2.tar.gz -C app/
```
          
### 1.3.3. 配置环境变量
```bash
[root@hadoop001 app]# vim /etc/profile
```
        
将以下内容添加到该文件末尾：
            
```html
# hbase
export HBASE_HOME=/usr/app/hbase-1.2.0-cdh5.15.2
export PATH=$HBASE_HOME/bin:$PATH
```
       
使配置立即生效。
        
```bash
[root@hadoop001 app]# source /etc/profile
```
        
### 1.3.4. 配置HBase
进入`${HBASE_HOME}/conf`目录下，修改配置：
#### 1.3.4.1. hbase-env.sh
```html
# 配置JDK安装位置
export JAVA_HOME=/usr/jdk1.8.0_261
# 不使用内置的zookeeper服务
export HBASE_MANAGES_ZK=false
```
#### 1.3.4.2. hbase-site.xml
```html
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
/**
 *
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
-->
<configuration>
    <property>
        <!-- 指定 hbase 以分布式集群的方式运行 -->
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <property>
        <!-- 指定 hbase 在 HDFS 上的存储位置 -->
        <name>hbase.rootdir</name>
        <value>hdfs://hadoop001:8020/hbase</value>
    </property>
    <property>
        <!-- 指定 zookeeper 的地址-->
        <name>hbase.zookeeper.quorum</name>
        <value>hadoop001:2181,hadoop002:2181,hadoop003:2181</value>
    </property>
</configuration>
```
#### 1.3.4.3. regionservers
```html
hadoop001
hadoop002
hadoop003
```
#### 1.3.4.4. backup-masters
需要新建一个`backup-masters`。
      
```html
hadoop002
```
        
### 1.3.5. HDFS配置
将`Hadoop`配置文件的位置信息添加到`hbase-env.sh`的`HBASE_CLASSPATH`属性，示例如下：
       
```html
export HBASE_CLASSPATH=/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
```
          
## 1.4. 启动
### 1.4.1. zookeeper
由于配置了zookeeper环境变量，所以直接运行以下命令(三台都需要执行该命令)。
         
```bash
[root@hadoop001 hadoop]# zkServer.sh start
```
       
### 1.4.2. hadoop
```bash
[root@hadoop001 sbin]# ./start-dfs.sh
[root@hadoop002 sbin]# ./start-yarn.sh
[root@hadoop003 sbin]# ./yarn-daemon.sh start resourcemanager
```
      
### 1.4.3. Hbase
```bash
[root@hadoop001 bin]# ./start-hbase.sh 
```
        
## 1.5. 验证
### 1.5.1. 进程
```bash
## hadoop001
[root@hadoop001 bin]# jps
7600 DFSZKFailoverController
7057 NameNode
7713 NodeManager
7395 JournalNode
9781 Jps
9274 HRegionServer
7179 DataNode
9116 HMaster
4047 QuorumPeerMain

## hadoop002
[root@hadoop002 sbin]# jps
3553 QuorumPeerMain
6354 HMaster
6260 HRegionServer
5221 JournalNode
5365 DFSZKFailoverController
5030 NameNode
5114 DataNode
6698 Jps
5611 NodeManager
5486 ResourceManager

## hadoop003
[root@hadoop003 sbin]# jps
5088 ResourceManager
5604 Jps
4678 DataNode
4790 JournalNode
4903 NodeManager
4600 QuorumPeerMain
5374 HRegionServer
```
### 1.5.2. UI界面
访问`HBase`的`Web-UI`界面，这里我安装的`HBase`版本为`1.2`，访问端口为`60010`，如果你安装的是`2.0`以上的版本，则访问端口号为`16010`。可以看到`Master`在`hadoop001`上，三个`Regin Servers`分别在`hadoop001，hadoop002，和hadoop003`上，并且还有一个`Backup Matser`服务在`hadoop002`上。
        
![1](https://live.staticflickr.com/65535/50281217213_855a966024_h.jpg)
         
![2](https://live.staticflickr.com/65535/50281896736_957bbfe3df_h.jpg)

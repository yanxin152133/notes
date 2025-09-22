# 1. 基于Zookeeper搭建Kafka高可用集群
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>Zookeeper：3.4.14
      
>Kafka：2.2.0
      
## 1.1. 前提
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)
- [Zookeeper集群环境搭建](notes/大数据/大数据常用软件安装/zookeeper/Zookeeper集群环境搭建.md)
      
## 1.2. Kafka
### 1.2.1. 下载
`Kafka`下载地址：http://kafka.apache.org/downloads
         
```bash
wget https://archive.apache.org/dist/kafka/2.2.0/kafka_2.12-2.2.0.tgz
```
          
>`kafka`安装包的`命名规则`：以`kafka_2.12-2.2.0.tgz`为例，前面的`2.12`代表`Scala`的版本号（Kafka采用Scala语言进行开发），后面的`2.2.0`则代表`Kafka`的版本号。
       
### 1.2.2. 解压
```bash
[root@hadoop001 usr]# tar -zxvf kafka_2.12-2.2.0.tgz
[root@hadoop001 usr]# mv kafka_2.12-2.2.0 app/
```
       
### 1.2.3. 配置
```bash
## 进入该路径
[root@hadoop001 app]# cd kafka_2.12-2.2.0/config/
[root@hadoop001 config]# ls
connect-console-sink.properties    connect-file-sink.properties    connect-standalone.properties  producer.properties     trogdor.conf
connect-console-source.properties  connect-file-source.properties  consumer.properties            server.properties       zookeeper.properties
connect-distributed.properties     connect-log4j.properties        log4j.properties               tools-log4j.properties
## 拷贝配置文件
[root@hadoop001 config]# cp server.properties server_1.properties 
[root@hadoop001 config]# cp server.properties server_2.properties 
[root@hadoop001 config]# cp server.properties server_3.properties 
[root@hadoop001 config]# ls
connect-console-sink.properties    connect-file-sink.properties    connect-standalone.properties  producer.properties  server_3.properties     trogdor.conf
connect-console-source.properties  connect-file-source.properties  consumer.properties            server_1.properties  server.properties       zookeeper.properties
connect-distributed.properties     connect-log4j.properties        log4j.properties               server_2.properties  tools-log4j.properties
```
        
#### 1.2.3.1. server_1.properties 
修改为以下内容：
      
```html
# The id of the broker. 集群中每个节点的唯一标识
broker.id=0
# 监听地址
listeners=PLAINTEXT://hadoop001:9092
# 数据的存储位置
log.dirs=/usr/local/kafka-logs/00
# Zookeeper连接地址
zookeeper.connect=hadoop001:2181,hadoop002:2181,hadoop003:2181
```

#### 1.2.3.2. server_2.properties 
修改为以下内容：
      
```html
# The id of the broker. 集群中每个节点的唯一标识
broker.id=1
# 监听地址
listeners=PLAINTEXT://hadoop002:9092
# 数据的存储位置
log.dirs=/usr/local/kafka-logs/01
# Zookeeper连接地址
zookeeper.connect=hadoop001:2181,hadoop002:2181,hadoop003:2181
```

#### 1.2.3.3. server_3.properties 
修改为以下内容：
      
```html
# The id of the broker. 集群中每个节点的唯一标识
broker.id=2
# 监听地址
listeners=PLAINTEXT://hadoop003:9092
# 数据的存储位置
log.dirs=/usr/local/kafka-logs/02
# Zookeeper连接地址
zookeeper.connect=hadoop001:2181,hadoop002:2181,hadoop003:2181
```
       
### 1.2.4. 分发
```bash
[root@hadoop001 config]# scp -r /usr/app/kafka_2.12-2.2.0/ hadoop002:/usr/app/
[root@hadoop001 config]# scp -r /usr/app/kafka_2.12-2.2.0/ hadoop003:/usr/app/
```
       
## 1.3. 启动
### 1.3.1. zookeeper
```bash
zkServer.sh start
```
     
### 1.3.2. Kafka
```bash
# 第一种方式
[root@hadoop001 bin]# pwd
/usr/app/kafka_2.12-2.2.0/bin
## hadoop001
[root@hadoop001 bin]# ./kafka-server-start.sh ../config/server_1.properties
## hadoop002
[root@hadoop002 bin]# ./kafka-server-start.sh ../config/server_2.properties
## hadoop003
[root@hadoop003 bin]# ./kafka-server-start.sh ../config/server_3.properties

# 第二种方式(步骤同上)
./kafka-server-start.sh -daemon ../config/server_1.properties
./kafka-server-start.sh -daemon ../config/server_2.properties
./kafka-server-start.sh -daemon ../config/server_3.properties
```
       
## 1.4. 验证
### 1.4.1. 进程
```bash
[root@hadoop001 bin]# jps
4240 Kafka
2596 QuorumPeerMain
4628 Jps

[root@hadoop002 bin]# jps
3458 Kafka
3813 Jps
2461 QuorumPeerMain

[root@hadoop003 bin]# jps
2723 QuorumPeerMain
6503 Jps
6141 Kafka
```
      
### 1.4.2. 节点注册信息
```bash
[root@hadoop003 bin]# zkCli.sh 
log4j:ERROR Could not find value for key log4j.appender.DaliyRollingFileAppender
log4j:ERROR Could not instantiate appender named "DaliyRollingFileAppender".
Connecting to localhost:2181
log4j:WARN No appenders could be found for logger (org.apache.zookeeper.ZooKeeper).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
Welcome to ZooKeeper!
JLine support is enabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
[zk: localhost:2181(CONNECTED) 0] ls /brokers/ids
[0, 1, 2]
[zk: localhost:2181(CONNECTED) 1] get /brokers/ids/0
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://hadoop001:9092"],"jmx_port":-1,"host":"hadoop001","timestamp":"1599215228005","port":9092,"version":4}
cZxid = 0x1e000000d3
ctime = Fri Sep 04 18:27:08 CST 2020
mZxid = 0x1e000000d3
mtime = Fri Sep 04 18:27:08 CST 2020
pZxid = 0x1e000000d3
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x2000015cf480001
dataLength = 188
numChildren = 0
[zk: localhost:2181(CONNECTED) 2] get /brokers/ids/1
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://hadoop002:9092"],"jmx_port":-1,"host":"hadoop002","timestamp":"1599215247282","port":9092,"version":4}
cZxid = 0x1e000000e5
ctime = Fri Sep 04 18:27:27 CST 2020
mZxid = 0x1e000000e5
mtime = Fri Sep 04 18:27:27 CST 2020
pZxid = 0x1e000000e5
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x2000015cf480002
dataLength = 188
numChildren = 0
[zk: localhost:2181(CONNECTED) 3] get /brokers/ids/2
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://hadoop003:9092"],"jmx_port":-1,"host":"hadoop003","timestamp":"1599215335737","port":9092,"version":4}
cZxid = 0x1e00000114
ctime = Fri Sep 04 18:28:55 CST 2020
mZxid = 0x1e00000114
mtime = Fri Sep 04 18:28:55 CST 2020
pZxid = 0x1e00000114
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x2000015cf480003
dataLength = 188
numChildren = 0
```
       
## 1.5. 创建测试主题
```bash
[root@hadoop001 bin]# ./kafka-topics.sh --create --bootstrap-server hadoop001:9092 --replication-factor 3 --partitions 1 --topic my-replicated-topic
[root@hadoop001 bin]# ./kafka-topics.sh --describe --bootstrap-server hadoop001:9092 --topic my-replicated-topic
Topic:my-replicated-topic       PartitionCount:1        ReplicationFactor:3     Configs:segment.bytes=1073741824
        Topic: my-replicated-topic      Partition: 0    Leader: 2       Replicas: 2,1,0 Isr: 2,1,0
```
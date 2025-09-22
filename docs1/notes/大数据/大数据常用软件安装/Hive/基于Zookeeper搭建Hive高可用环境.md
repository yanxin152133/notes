# 1. 基于Zookeeper搭建Hive高可用环境
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>Zookeeper：3.4.14
         
>hadoop：hadoop-2.6.0-cdh5.15.2
        
>mysql：5.6.49
       
>Hive：hive-1.1.0-cdh5.15.2
       
## 1.1. 规划
- zookeeper：hadoop001,hadoop002,hadoop003
- namenode：hadoop001,hadoop002
- datanode：hadoop001,hadoop002,hadoop003
- hive：hadoop001,hadoop002,hadoop003
- mysql：hadoop003
           
## 1.2. 前提
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)
- [Zookeeper集群环境搭建](notes/大数据/大数据常用软件安装/zookeeper/Zookeeper集群环境搭建.md)
- [基于Zookeeper搭建Hadoop高可用集群](notes/大数据/大数据常用软件安装/Hadoop/基于Zookeeper搭建Hadoop高可用集群.md)
- [mysql](notes/大数据/大数据常用软件安装/mysql/Linux安装mysql.md)
       
## 1.3. 下载
下载地址：http://archive.cloudera.com/cdh5/cdh/5/
       
## 1.4. 解压
```bash
[root@localhost usr]# tar -zxvf hive-1.1.0-cdh5.15.2.tar.gz -C app/
```
        
## 1.5. 配置环境变量
```bash
[root@localhost hive-1.1.0-cdh5.15.2]# vim /etc/profile
```
       
将以下内容加入该文件末尾：
       
```html
# hive
export HIVE_HOME=/usr/app/hive-1.1.0-cdh5.15.2
export PATH=$HIVE_HOME/bin:$PATH
```
       
使配置立即生效。
        
```bash
[root@localhost hive-1.1.0-cdh5.15.2]# source /etc/profile
```
        
## 1.6. 配置
进入`conf`目录，对以下文件进行配置修改。
       
### 1.6.1. hive-env.sh
```bash
[root@localhost conf]# cp hive-env.sh.template hive-env.sh
[root@localhost conf]# vim hive-env.sh
```
       
修改为以下内容：
       
```html
# Set HADOOP_HOME to point to a specific hadoop install directory
HADOOP_HOME=/usr/app/hadoop-2.6.0-cdh5.15.2

# Hive Configuration Directory can be controlled by:
export HIVE_CONF_DIR=/usr/app/hive-1.1.0-cdh5.15.2/conf

# Folder containing extra ibraries required for hive compilation/execution can be controlled by:
export HIVE_AUX_JARS_PATH=/usr/app/hive-1.1.0-cdh5.15.2/lib
```
       
### 1.6.2. hive-site.xml
新建 hive-site.xml 文件，内容如下：
       
```html
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration> 
  <property> 
    <name>hive.metastore.local</name>  
    <value>true</value> 
  </property>  
  <!-- 数据库集成配置 -->  
  <property> 
    <name>javax.jdo.option.ConnectionURL</name>  
    <value>jdbc:mysql://hadoop003:3306/hadoop_hive?createDatabaseIfNotExist=true</value> 
  </property>  
  <property> 
    <name>javax.jdo.option.ConnectionDriverName</name>  
    <value>com.mysql.jdbc.Driver</value> 
  </property>  
  <property> 
    <name>javax.jdo.option.ConnectionUserName</name>  
    <value>root</value> 
  </property>  
  <property> 
    <name>javax.jdo.option.ConnectionPassword</name>  
    <value>yan1+</value> 
  </property>  
  <!-- hive工作目录配置 -->  
  <property> 
    <!-- 指定HDFS内hive数据临时文件存放目录 -->  
    <name>hive.exec.scratchdir</name>  
    <value>/usr/app/hive-1.1.0-cdh5.15.2/tmp</value>  
    <description>HDFS root scratch dir for Hive jobs which gets created with write all (733) permission. For each connecting user, an HDFS scratch dir: ${hive.exec.scratchdir}/&lt;username&gt; is created, with ${hive.scratch.dir.permission}.</description> 
  </property>  
  <property> 
    <!-- hive的本地临时目录 -->  
    <name>hive.exec.local.scratchdir</name>  
    <value>/usr/app/hive-1.1.0-cdh5.15.2/jobslog/${system:user.name}</value>  
    <description>Local scratch space for Hive jobs</description> 
  </property>  
  <property> 
    <!-- hive下载的本地临时目录 -->  
    <name>hive.downloaded.resources.dir</name>  
    <value>/usr/app/hive-1.1.0-cdh5.15.2/jobslog/${hive.session.id}_resources</value>  
    <description>Temporary local directory for added resources in the remote file system.</description> 
  </property>  
  <!-- 指定HDFS内hive数据存放目录 -->  
  <property> 
    <name>hive.metastore.warehouse.dir</name>  
    <value>/user/hive/warehouse</value>  
    <description>location of default database for the warehouse</description> 
  </property>  
  <!-- 元数据 -->  
  <property> 
    <name>datanucleus.schema.autoCreateAll</name>  
    <value>true</value> 
  </property>  
  <property> 
    <name>hive.metastore.schema.verification</name>  
    <value>false</value> 
  </property>  
  <!-- HiveServer2 HA config -->  
  <property> 
    <name>hive.zookeeper.quorum</name>  
    <value>hadoop001:2181,hadoop002:2181,hadoop003:2181</value>  
    <description>List of ZooKeeper servers to talk to. This is needed for: 1. Read/write locks - when hive.lock.manager is set to org.apache.hadoop.hive.ql.lockmgr.zookeeper.ZooKeeperHiveLockManager, 2. When HiveServer2 supports service discovery via Zookeeper. 3. For delegation token storage if zookeeper store is used, if hive.cluster.delegation.token.store.zookeeper.connectString is not set 4. LLAP daemon registry service 5. Leader selection for privilege synchronizer</description> 
  </property>  
  <property> 
    <name>hive.server2.support.dynamic.service.discovery</name>  
    <value>true</value>  
    <description>Whether HiveServer2 supports dynamic service discovery for its clients. To support this, each instance of HiveServer2 currently uses ZooKeeper to register itself, when it is brought up. JDBC/ODBC clients should use the ZooKeeper ensemble: hive.zookeeper.quorum in their connection string.</description> 
  </property>  
  <property> 
    <name>hive.server2.zookeeper.namespace</name>  
    <value>hiveserver2_zk</value>  
    <description>The parent node in ZooKeeper used by HiveServer2 when supporting dynamic service discovery.</description> 
  </property>  
  <property> 
    <name>hive.server2.zookeeper.publish.configs</name>  
    <value>true</value>  
    <description>Whether we should publish HiveServer2's configs to ZooKeeper.</description> 
  </property>  
  <property> 
    <name>hive.zookeeper.client.port</name>  
    <value>2181</value> 
  </property>  
  <!-- thrift config -->  
  <property> 
    <name>hive.server2.thrift.bind.host</name>  
    <!-- 这个不同的机器上面，对应不同的主机名 -->  
    <value>hadoop001</value>  
    <description>Bind host on which to run the HiveServer2 Thrift service.</description> 
  </property>  
  <property> 
    <name>hive.server2.thrift.port</name>  
    <value>10000</value>  
    <description>Port number of HiveServer2 Thrift interface when hive.server2.transport.mode is 'binary'.</description> 
  </property>  
  <property> 
    <name>hive.server2.thrift.http.port</name>  
    <value>10001</value>  
    <description>Port number of HiveServer2 Thrift interface when hive.server2.transport.mode is 'http'.</description> 
  </property>  
  <!-- 这里要和hadoop，core-site.xml 代理用户要一致 -->  
  <!-- 由于本机全部都是使用root用户，因此这里配置为root用户以及密码 -->
  <property> 
    <name>hive.server2.thrift.client.user</name>  
    <value>root</value>  
    <description>Username to use against thrift client</description> 
  </property>  
  <property> 
    <name>hive.server2.thrift.client.password</name>  
    <value>yan1+</value>  
    <description>Password to use against thrift client</description> 
  </property>  
  <!-- 使用连接用户，执行Hive相关的操作 -->  
  <property> 
    <name>hive.server2.enable.doAs</name>  
    <value>false</value>  
    <description>Setting this property to true will have HiveServer2 execute Hive operations as the user making the calls to it.</description> 
  </property>  
  <!-- java.io config -->  
  <property> 
    <name>system:java.io.tmpdir</name>  
    <value>/usr/app/hive-1.1.0-cdh5.15.2/javaio</value> 
  </property>  
  <property> 
    <name>system:user.name</name>  
    <value>${user.name}</value> 
  </property> 
</configuration>
```
       
### 1.6.3. hive-log4j.properties
```bash
[root@hadoop001 conf]# cp hive-log4j.properties.template hive-log4j.properties
[root@hadoop001 conf]# vim hive-log4j.properties
```
        
修改内容为：
       
```html
hive.log.dir=/usr/app/hive-1.1.0-cdh5.15.2/log
```
       
### 1.6.4. hive-exec-log4j.properties
```bash
[root@hadoop001 conf]# cp hive-exec-log4j.properties.template hive-exec-log4j.properties
[root@hadoop001 conf]# vim hive-exec-log4j.properties
```
      
修改内容为：
        
```html
hive.log.dir=/usr/app/hive-1.1.0-cdh5.15.2/exelog
```
       
### 1.6.5. 拷贝数据库驱动
将`MySQL驱动包`拷贝到`Hive`安装目录的`lib`目录下, `MySQL 驱动`的下载地址为：https://dev.mysql.com/downloads/connector/j/
        
```bash
[root@hadoop001 usr]# tar -zxvf mysql-connector-java-5.1.15.tar.gz 
[root@hadoop001 mysql-connector-java-5.1.15]# cp mysql-connector-java-5.1.15-bin.jar ../app/hive-1.1.0-cdh5.15.2/lib/
[root@hadoop001 lib]# pwd
/usr/app/hive-1.1.0-cdh5.15.2/lib
[root@hadoop001 lib]# ll mysql-*
-rw-r--r--. 1 root root 785998 9月   3 00:51 mysql-connector-java-5.1.15-bin.jar    ## 这个是解压后的文件，不是下载的那个
```
      
## 1.7. 分发安装包
```bash
[root@hadoop001 conf]# scp -r /usr/app/hive-1.1.0-cdh5.15.2/ hadoop002:/usr/app
[root@hadoop001 conf]# scp -r /usr/app/hive-1.1.0-cdh5.15.2/ hadoop003:/usr/app
```
      
### 1.7.1. 环境变量
**配置hadoop002,hadoop003的环境变量**
         
### 1.7.2. hive-site.xml
`hadoop002`:
       
```html
    <!-- thrift config -->
    <property>
        <name>hive.server2.thrift.bind.host</name>
    <!-- 这个不同的机器上面，对应不同的主机名 -->
        <value>hadoop002</value>
        <description>Bind host on which to run the HiveServer2 Thrift service.</description>
    </property>
```
       
`hadoop003`:
       
```html
    <!-- thrift config -->
    <property>
        <name>hive.server2.thrift.bind.host</name>
    <!-- 这个不同的机器上面，对应不同的主机名 -->
        <value>hadoop003</value>
        <description>Bind host on which to run the HiveServer2 Thrift service.</description>
    </property>
```
         
## 1.8. 开启hadoop的webhdfs配置
```bash
[root@hadoop001 conf]# cd /usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
[root@hadoop001 hadoop]# vim hdfs-site.xml 
```
         
添加以下内容：
      
```html
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>
```
       
### 1.8.1. 分发
```bash
[root@hadoop001 hadoop]# scp hdfs-site.xml hadoop002:/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
hdfs-site.xml                                                                                                                          100% 3879    41.5KB/s   00:00    
[root@hadoop001 hadoop]# scp hdfs-site.xml hadoop003:/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
hdfs-site.xml    
```
        
## 1.9. 配置hadoop访问用户
`core-site.xml`配置, 这个配置很重要`name`页签中的`hadoop`登录`hdfs`的具体的用户名，如果写错的话，我们使用hadoop用户，访问hive的时候会没有权限访问。

本机全部使用的是`root`用户，因此配置如下：
         
```html
    <property>
        <name>hadoop.proxyuser.root.hosts</name>
        <value>*</value>
    </property>
    <property>
        <name>hadoop.proxyuser.root.groups</name>
        <value>*</value>
    </property>
```
      
### 1.9.1. 分发
```bash
[root@hadoop001 hadoop]# scp core-site.xml hadoop002:/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
core-site.xml                                                                                                                          100% 1697   411.9KB/s   00:00    
[root@hadoop001 hadoop]# scp core-site.xml hadoop003:/usr/app/hadoop-2.6.0-cdh5.15.2/etc/hadoop
core-site.xml       
```
      
## 1.10. 启动
### 1.10.1. zookeeper
```bash
zkServer.sh start
```
      
### 1.10.2. hadoop
```
cd /usr/app/hadoop-2.6.0-cdh5.15.2/sbin/
./start-dfs.sh 
```
      
### 1.10.3. 创建HIVE数仓目录和job中间目录
```bash
hdfs dfs -mkdir -p /user/hive/warehouse
hdfs dfs -mkdir -p /usr/app/hive-1.1.0-cdh5.15.2/tmp
hdfs dfs -chmod 777 /usr/app/hive-1.1.0-cdh5.15.2/tmp
hdfs dfs -chmod 777 /user/hive/warehouse
hdfs dfs -ls /
```
      
### 1.10.4. 初始化Metastore元信息Schema
- 当使用的`hive`是`1.x`版本时，可以不进行初始化操作，Hive 会在第一次启动的时候会自动进行初始化，但不会生成所有的元数据信息表，只会初始化必要的一部分，在之后的使用中用到其余表时会自动创建；
      
- 当使用的`hive`是`2.x`版本时，必须手动初始化元数据库。初始化命令：
              
```bash
schematool -dbType mysql -initSchema
```
      
具体操作：
       
由于上面已经配置过环境变量，这里直接启动即可：
     
```bash
[root@hadoop001 lib]# schematool -dbType mysql -initSchema
ls: 无法访问/usr/app/spark-2.4.6-bin-hadoop2.6/lib/spark-assembly-*.jar: 没有那个文件或目录
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/app/hbase-1.2.0-cdh5.15.2/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/app/hadoop-2.6.0-cdh5.15.2/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
2020-09-03 00:54:16,134 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
20/09/03 00:54:17 WARN conf.HiveConf: HiveConf of name hive.server2.thrift.client.user does not exist
20/09/03 00:54:17 WARN conf.HiveConf: HiveConf of name hive.metastore.local does not exist
20/09/03 00:54:17 WARN conf.HiveConf: HiveConf of name hive.server2.zookeeper.publish.configs does not exist
20/09/03 00:54:17 WARN conf.HiveConf: HiveConf of name hive.server2.thrift.client.password does not exist
Metastore connection URL:        jdbc:mysql://hadoop003:3306/hadoop_hive?createDatabaseIfNotExist=true
Metastore Connection Driver :    com.mysql.jdbc.Driver
Metastore connection User:       root
Starting metastore schema initialization to 1.1.0-cdh5.15.2
Initialization script hive-schema-1.1.0.mysql.sql
Initialization script completed
schemaTool completed
```
      
### 1.10.5. 查看元信息Schema
由于上面已经配置过环境变量，这里直接启动即可：
      
```bash
[root@hadoop001 lib]# schematool -dbType mysql -info
ls: 无法访问/usr/app/spark-2.4.6-bin-hadoop2.6/lib/spark-assembly-*.jar: 没有那个文件或目录
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/app/hbase-1.2.0-cdh5.15.2/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/app/hadoop-2.6.0-cdh5.15.2/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
2020-09-03 00:55:07,436 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
20/09/03 00:55:08 WARN conf.HiveConf: HiveConf of name hive.server2.thrift.client.user does not exist
20/09/03 00:55:08 WARN conf.HiveConf: HiveConf of name hive.metastore.local does not exist
20/09/03 00:55:08 WARN conf.HiveConf: HiveConf of name hive.server2.zookeeper.publish.configs does not exist
20/09/03 00:55:08 WARN conf.HiveConf: HiveConf of name hive.server2.thrift.client.password does not exist
Metastore connection URL:        jdbc:mysql://hadoop003:3306/hadoop_hive?createDatabaseIfNotExist=true
Metastore Connection Driver :    com.mysql.jdbc.Driver
Metastore connection User:       root
Hive distribution version:       1.1.0-cdh5.15.2
Metastore schema version:        1.1.0-cdh5.12.0
schemaTool completed
```
           
### 1.10.6. 启动HiveServe2
由于上面已经配置过环境变量，这里直接启动即可：
     
```bash
nohup hiveserver2 &
```
         
### 1.10.7. 验证
#### 1.10.7.1. 进程
查看进程，多了一个`RunJar`的进行，启动成功。
      
```bash
[root@hadoop001 sbin]# jps
3312 RunJar
2706 JournalNode
3554 Jps
2470 DataNode
1976 QuorumPeerMain
2905 DFSZKFailoverController
2362 NameNode
```
         
#### 1.10.7.2. UI界面
UI 界面的端口为`10002`。
        
![](https://live.staticflickr.com/65535/50300148418_581238eb2e_k.jpg)
         
### 1.10.8. 启动hadoop002、hadoop003上的hive
由于上面已经配置过环境变量，这里直接启动即可：
     
```bash
nohup hiveserver2 &
```
       
### 1.10.9. zkCli.sh 
按照以下图中的步骤运行命令，红框中显示为自己的设置zookeeper节点即表示HiveServer全部启动，并且注册到zookeeper。
         
![](https://live.staticflickr.com/65535/50300846566_e363f7f9fb_k.jpg)
      
## 1.11. HIVE CLI
直接使用以下命令启动。
        
```bash
[root@hadoop001 hadoop]# hive
ls: 无法访问/usr/app/spark-2.4.6-bin-hadoop2.6/lib/spark-assembly-*.jar: 没有那个文件或目录
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/app/hbase-1.2.0-cdh5.15.2/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/app/hadoop-2.6.0-cdh5.15.2/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
2020-09-03 16:46:22,426 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
20/09/03 16:46:23 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
20/09/03 16:46:24 WARN conf.HiveConf: HiveConf of name hive.server2.thrift.client.user does not exist
20/09/03 16:46:24 WARN conf.HiveConf: HiveConf of name hive.metastore.local does not exist
20/09/03 16:46:24 WARN conf.HiveConf: HiveConf of name hive.server2.zookeeper.publish.configs does not exist
20/09/03 16:46:24 WARN conf.HiveConf: HiveConf of name hive.server2.thrift.client.password does not exist

Logging initialized using configuration in file:/usr/app/hive-1.1.0-cdh5.15.2/conf/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.
hive> show databases;
OK
default
Time taken: 3.345 seconds, Fetched: 1 row(s)
hive> 
```
         
## 1.12. Beeline CLI
按照以下命令执行：
     
```bash
[root@hadoop001 hadoop]# beeline 
ls: 无法访问/usr/app/spark-2.4.6-bin-hadoop2.6/lib/spark-assembly-*.jar: 没有那个文件或目录
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/app/hbase-1.2.0-cdh5.15.2/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/app/hadoop-2.6.0-cdh5.15.2/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
2020-09-03 16:48:07,965 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Beeline version 1.1.0-cdh5.15.2 by Apache Hive
beeline> !connect jdbc:hive2://hadoop001:10000 root yan1+
scan complete in 1ms
Connecting to jdbc:hive2://hadoop001:10000
Connected to: Apache Hive (version 1.1.0-cdh5.15.2)
Driver: Hive JDBC (version 1.1.0-cdh5.15.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://hadoop001:10000> show databases;
INFO  : Compiling command(queryId=root_20200903164949_bfa266c7-ba2c-49ed-92c8-a460729ed163): show databases
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:database_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=root_20200903164949_bfa266c7-ba2c-49ed-92c8-a460729ed163); Time taken: 0.702 seconds
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Executing command(queryId=root_20200903164949_bfa266c7-ba2c-49ed-92c8-a460729ed163): show databases
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=root_20200903164949_bfa266c7-ba2c-49ed-92c8-a460729ed163); Time taken: 0.112 seconds
INFO  : OK
+----------------+--+
| database_name  |
+----------------+--+
| default        |
+----------------+--+
1 row selected (1.297 seconds)
0: jdbc:hive2://hadoop001:10000> !quit
Closing: 0: jdbc:hive2://hadoop001:10000
```
        
## 1.13. Beeline集群模式
```bash
[root@hadoop001 hadoop]# beeline 
ls: 无法访问/usr/app/spark-2.4.6-bin-hadoop2.6/lib/spark-assembly-*.jar: 没有那个文件或目录
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/app/hbase-1.2.0-cdh5.15.2/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/app/hadoop-2.6.0-cdh5.15.2/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
2020-09-03 16:51:58,050 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Beeline version 1.1.0-cdh5.15.2 by Apache Hive
beeline> !connect jdbc:hive2://hadoop001:2181,hadoop002:2181,hadoop003:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk   ## 启动集群模式
scan complete in 2ms
Connecting to jdbc:hive2://hadoop001:2181,hadoop002:2181,hadoop003:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Enter username for jdbc:hive2://hadoop001:2181,hadoop002:2181,hadoop003:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk: root   ## 输入用户名
Enter password for jdbc:hive2://hadoop001:2181,hadoop002:2181,hadoop003:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk: *****  ## 输入密码
20/09/03 16:52:15 [main]: INFO jdbc.HiveConnection: Connected to hadoop003:10000
Connected to: Apache Hive (version 1.1.0-cdh5.15.2)
Driver: Hive JDBC (version 1.1.0-cdh5.15.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://hadoop001:2181,hadoop002:2181> show databases;
INFO  : Compiling command(queryId=root_20200903165252_e1046ac6-0190-4435-874f-d2cb3d986b0a): show databases
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:database_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=root_20200903165252_e1046ac6-0190-4435-874f-d2cb3d986b0a); Time taken: 0.503 seconds
INFO  : Concurrency mode is disabled, not creating a lock manager
INFO  : Executing command(queryId=root_20200903165252_e1046ac6-0190-4435-874f-d2cb3d986b0a): show databases
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=root_20200903165252_e1046ac6-0190-4435-874f-d2cb3d986b0a); Time taken: 0.041 seconds
INFO  : OK
+----------------+--+
| database_name  |
+----------------+--+
| default        |
+----------------+--+
1 row selected (0.832 seconds)
0: jdbc:hive2://hadoop001:2181,hadoop002:2181> !quit
Closing: 0: jdbc:hive2://hadoop001:2181,hadoop002:2181,hadoop003:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
```
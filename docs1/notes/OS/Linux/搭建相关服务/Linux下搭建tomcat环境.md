# 1. Linux下搭建tomcat环境
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
      
>tomcat：Apache Tomcat/7.0.106
        
# 2. Java jdk
## 2.1. 下载
- [jdk 8u261](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

## 2.2. 解压
```bash
[root@localhost usr]# tar -zxvf jdk-8u261-linux-x64.tar.gz
```
      
## 2.3. 设置环境变量
```bash
[root@localhost jdk1.8.0_261]# vim /etc/profile
```
          
在该文件最后加上以下内容：     
     
```html
export JAVA_HOME=/usr/jdk1.8.0_261      ## 具体路径根据本机jdk安装路径进行更改
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
```
           
执行`source`命令，使配置生效：
           
```bash
[root@localhost jdk1.8.0_261]# source /etc/profile
```
        
## 2.4. 验证
```bash
[root@localhost jdk1.8.0_261]# java -version
java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)
```
      
显示以上内容则为安装成功。
      
# 3. tomcat
## 3.1. 下载
下载地址：[tomcat 7](https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-7/v7.0.106/bin/apache-tomcat-7.0.106.tar.gz)
        
## 3.2. 解压
将下载后的文件上传到服务器上，并执行以下解压命令：
        
```bash
[root@localhost usr]# tar -zxvf apache-tomcat-7.0.106.tar.gz 
```
          
## 3.3. 启动
执行以下命令：
       
```bash
[root@localhost /]# cd /usr/apache-tomcat-7.0.106/bin/
## 启动tomcat，以下二者选其一
[root@localhost bin]# ./startup.sh 
[root@localhost bin]# ./catalina.sh start
```
        
## 3.4. 开放端口
执行以下命令：
       
```bash
[root@localhost bin]# netstat -tunlp | grep 8080      ## 查看端口占用
tcp6       0      0 :::8080                 :::*                    LISTEN      2305/java           
[root@localhost bin]# systemctl status firewalld      ## 查看防火墙状态
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since 六 2020-10-10 16:39:41 CST; 18min ago
     Docs: man:firewalld(1)
 Main PID: 791 (firewalld)
    Tasks: 2
   CGroup: /system.slice/firewalld.service
           └─791 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

10月 10 16:39:40 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
10月 10 16:39:41 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
10月 10 16:39:42 localhost.localdomain firewalld[791]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure configuration option. It will b...g it now.
Hint: Some lines were ellipsized, use -l to show in full.
[root@localhost bin]# firewall-cmd --zone=public --add-port=8080/tcp --permanent    ## 开放8080端口
success
[root@localhost bin]# firewall-cmd --reload      ## 重新加载防火墙
success
[root@localhost bin]# firewall-cmd --zone=public --query-port=8080/tcp    ## 查看是否修改成功
yes
```
          
## 3.5. 验证
浏览器输入`ip:8080`，如下：
      
![tomcat](https://live.staticflickr.com/65535/50444345976_fe02860bbd_b.jpg)
       
## 3.6. 配置
### 3.6.1. 帐号密码权限
修改`tomcat-users.xml`文件：
       
```bash
[root@localhost]# vim /usr/apache-tomcat-7.0.106/conf/tomcat-users.xml
```
         
改为以下内容：
        
```html
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
<!--
  NOTE:  By default, no user is included in the "manager-gui" role required
  to operate the "/manager/html" web application.  If you wish to use this app,
  you must define such a user - the username and password are arbitrary. It is
  strongly recommended that you do NOT use one of the users in the commented out
  section below since they are intended for use with the examples web
  application.
-->
<!--
  NOTE:  The sample user and role entries below are intended for use with the
  examples web application. They are wrapped in a comment and thus are ignored
  when reading this file. If you wish to configure these users for use with the
  examples web application, do not forget to remove the <!.. ..> that surrounds
  them. You will also need to set the passwords to something appropriate.
-->
    <role rolename="tomcat"/>
    <role rolename="manager-gui"/>
    <role rolename="admin-gui"/>
    <role rolename="manager-script"/>
    <role rolename="admin-script"/>
    <user username="tomcat" password="tomcat" roles="tomcat,manager-gui,admin-gui,admin-script,manager-script"/>
</tomcat-users>
```

如果无法访问：修改`context.xml`文件：
                  
```html
<?xml version='1.0' encoding='utf-8'?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<!-- The contents of this file will be loaded for each web application -->
<Context>

    <!-- Default set of monitored resources -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->

    <!-- Uncomment this to enable Comet connection tacking (provides events
         on session expiration as well as webapp lifecycle) -->
    <!--
    <Valve className="org.apache.catalina.valves.CometConnectionManagerValve" />
    -->

    <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|\d+\.\d+\.\d+\.\d+" />
</Context>
```
         
### 3.6.2. 配置服务
新建一个脚本：
        
```bash
[root@localhost /]# vim /etc/init.d/tomcat
```
        
添加以下内容：
       
```html
#!/bin/bash
# description: Tomcat7 Start Stop Restart
# processname: tomcat7
# chkconfig: 234 20 80

CATALINA_HOME=/usr/apache-tomcat-7.0.106   ## 填写自己本机tomcat的路径

case $1 in
        start)
                sh $CATALINA_HOME/bin/startup.sh
                ;;
        stop)
                sh $CATALINA_HOME/bin/shutdown.sh
                ;;
        restart)
                sh $CATALINA_HOME/bin/shutdown.sh
                sh $CATALINA_HOME/bin/startup.sh
                ;;
        *)
                echo 'please use : tomcat {start | stop | restart}'
        ;;
esac
exit 0
```
         
配置权限：
      
```bash
[root@localhost init.d]# ll
总用量 44
-rw-r--r--. 1 root root 18281 8月  19 2019 functions
-rwxr-xr-x. 1 root root  4569 8月  19 2019 netconsole
-rwxr-xr-x. 1 root root  7928 8月  19 2019 network
-rw-r--r--. 1 root root  1160 8月   7 01:30 README
-rw-r--r--. 1 root root   557 10月 12 08:49 tomcat

[root@localhost init.d]# chmod 744 tomcat 

[root@localhost init.d]# ll
总用量 44
-rw-r--r--. 1 root root 18281 8月  19 2019 functions
-rwxr-xr-x. 1 root root  4569 8月  19 2019 netconsole
-rwxr-xr-x. 1 root root  7928 8月  19 2019 network
-rw-r--r--. 1 root root  1160 8月   7 01:30 README
-rwxr--r--. 1 root root   557 10月 12 08:49 tomcat
```
          
执行脚本，启动、停止和重启服务：
       
```html
service tomcat start    ## 启动
service tomcat stop     ## 停止
service tomcat restart  ## 重启
```
         
### 3.6.3. 配置开机启动
```bash
[root@localhost init.d]# chkconfig --add tomcat
[root@localhost init.d]# chkconfig tomcat on
[root@localhost init.d]# chkconfig --list | grep tomcat

注：该输出结果只显示 SysV 服务，并不包含
原生 systemd 服务。SysV 配置数据
可能被原生 systemd 配置覆盖。 

      要列出 systemd 服务，请执行 'systemctl list-unit-files'。
      查看在具体 target 启用的服务请执行
      'systemctl list-dependencies [target]'。

tomcat          0:关    1:关    2:开    3:开    4:开    5:开    6:关
```
       
### 3.6.4. 配置端口号
相关配置文件在`conf`目录下的`server.xml`。
             
修改：
         
```html
## port="8080" 修改该项即可
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
        
重启tomcat，防火墙也需要开放该端口，之后即可更改tomcat端口。
# 1. Linux下Flume的安装
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
     
>flume：flume-ng-1.6.0-cdh5.15.2.tar.gz
       
## 1.1. 前提
- [Linux下 Java JDK 安装](notes/大数据/大数据常用软件安装/基础软件/JDK安装.md)

## 1.2. flume
### 1.2.1. 下载
下载所需版本的 Flume，这里我下载的是 CDH 版本的 Flume。下载地址为：http://archive.cloudera.com/cdh5/cdh/5/
        
### 1.2.2. 解压
```bash
[root@hadoop001 usr]# tar -zxvf flume-ng-1.6.0-cdh5.15.2.tar.gz -C app/
```
          
### 1.2.3. 配置环境变量
```bash
[root@hadoop001 usr]# vim /etc/profile
```
           
将以下内容加入该文件末尾：
      
```html
# flume
export FLUME_HOME=/usr/app/apache-flume-1.6.0-cdh5.15.2-bin
export PATH=$FLUME_HOME/bin:$PATH
```
           
使配置立即生效。
        
```bash
[root@hadoop001 app]# source /etc/profile
```
         
### 1.2.4. 配置
进入`conf`目录，执行以下命令：
        
```bash
[root@hadoop001 conf]# cp flume-env.sh.template flume-env.sh
[root@hadoop001 conf]# vim flume-env.sh
```
        
指定Java jdk路径。
       
```html
export JAVA_HOME=/usr/jdk1.8.0_261
```
        
## 1.3. 验证
```bash
[root@hadoop001 conf]# flume-ng version
错误: 找不到或无法加载主类 org.apache.flume.tools.GetJavaProperty     ## 可忽略
Flume 1.6.0-cdh5.15.2
Source code repository: https://git-wip-us.apache.org/repos/asf/flume.git
Revision: f46a6a50ceb577f08bba42a42190d006f4f2cafd
Compiled by jenkins on Tue Nov 13 05:59:29 PST 2018
From source with checksum 7d51c62363fd688efa872bf1bfb32098
```
# 1. Linux下 Java JDK 安装
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
        
## 1.1. 下载
- [jdk 8u261](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

## 1.2. 解压
```bash
[root@localhost usr]# tar -zxvf jdk-8u261-linux-x64.tar.gz
```
      
## 1.3. 设置环境变量
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
        
## 1.4. 验证
```bash
[root@localhost jdk1.8.0_261]# java -version
java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)
```
      
显示以上内容则为安装成功。
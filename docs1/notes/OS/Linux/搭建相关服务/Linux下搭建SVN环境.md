# 1. Linux下搭建SVN环境
>系统版本：CentOS Linux release 7.8.2003 (Core)

>SVN（离线安装）：subversion-1.6.16
      
>相关依赖版本参考文中其他内容
       
## 1.1. 在线安装
### 1.1.1. 检测是否安装过SVN
输入以下命令：       
       
```bash
[centos@localhost ~]$ rpm -qa | grep subversion
```
       
### 1.1.2. 安装
```bash
[centos@localhost centos]# sudo yum install -y subversion
```
        
## 1.2. 离线安装
### 1.2.1. 下载
+ svn(安装包以及相关依赖)
  - [subversion-1.6.16.tar.gz](https://archive.apache.org/dist/subversion/subversion-1.6.16.tar.gz)
  - [subversion-deps-1.6.16.tar.gz](https://archive.apache.org/dist/subversion/subversion-deps-1.6.16.tar.gz)
+ gcc&g++
+ openssl

       
### 1.2.2. gcc&gcc-c++
##### 1.2.2.0.1. yumdownloader
为了简化下载有关依赖的RPM的过程，使用`yumdownloader`工具来下载相关RPM包。
     
```bash
sudo yum install yum-utils -y
mkdir /usr/local/src/gcc
mkdir /usr/local/src/gcc-c++

yumdownloader --resolve --destdir mkdir /usr/local/src/gcc gcc
yumdownloader --resolve --destdir mkdir /usr/local/src/gcc-c++ gcc-c++
```
      
##### 1.2.2.0.2. 安装
`将上一步下载的rpm包上传到离线环境的服务器上，执行以下命令进行安装。`
          
```bash
## 本机存放路径/usr/local/src/gcc
[root@localhost gcc]# rpm -Uvh *.rpm --nodeps --force

## 本机存放路径/usr/local/src/gcc-c++
[root@localhost gcc-c++]# rpm -Uvh *.rpm --nodeps --force
```
       
##### 1.2.2.0.3. 验证
```bash
[root@localhost openssl-fips-2.0.16]# gcc -v
使用内建 specs。
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-redhat-linux/4.8.5/lto-wrapper
目标：x86_64-redhat-linux
配置为：../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --with-linker-hash-style=gnu --enable-languages=c,c++,objc,obj-c++,java,fortran,ada,go,lto --enable-plugin --enable-initfini-array --disable-libgcj --with-isl=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/isl-install --with-cloog=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/cloog-install --enable-gnu-indirect-function --with-tune=generic --with-arch_32=x86-64 --build=x86_64-redhat-linux
线程模型：posix
gcc 版本 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 


[root@localhost openssl-fips-2.0.16]# g++ -v
使用内建 specs。
COLLECT_GCC=g++
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-redhat-linux/4.8.5/lto-wrapper
目标：x86_64-redhat-linux
配置为：../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --with-linker-hash-style=gnu --enable-languages=c,c++,objc,obj-c++,java,fortran,ada,go,lto --enable-plugin --enable-initfini-array --disable-libgcj --with-isl=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/isl-install --with-cloog=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/cloog-install --enable-gnu-indirect-function --with-tune=generic --with-arch_32=x86-64 --build=x86_64-redhat-linux
线程模型：posix
gcc 版本 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 


[root@localhost openssl-fips-2.0.16]# rpm -qa|grep gcc
gcc-c++-4.8.5-39.el7.x86_64
gcc-4.8.5-39.el7.x86_64
libgcc-4.8.5-39.el7.x86_64
[root@localhost openssl-fips-2.0.16]# 
```
         
### 1.2.3. openssl
#### 1.2.3.1. 下载
```bash
yumdownloader --resolve --destdir mkdir /usr/local/src/openssl openssl
yumdownloader --resolve --destdir mkdir /usr/local/src/openssl openssl-devel
```
       
#### 1.2.3.2. 安装
```bash
## 将所有rpm整理到同一路径下
[root@localhost openssl]# rpm -Uvh *.rpm --nodeps --force
```
        
### 1.2.4. svn
#### 1.2.4.1. 下载
[官网下载](https://archive.apache.org/dist/subversion/)
      
#### 1.2.4.2. 安装
存放安装包的路径为`/home/centos/svn`，安装目录为`/home/centos/subversion`
```bash
## 版本名称
[root@localhost svn]# ll
总用量 12244
-rw-r--r--. 1 root root 7516368 10月 16 15:19 subversion-1.6.16.tar.gz
-rw-r--r--. 1 root root 5014005 10月 16 15:19 subversion-deps-1.6.16.tar.gz

## 解压
[root@localhost svn]# tar -zxvf subversion-1.6.16.tar.gz 
[root@localhost svn]# tar -zxvf subversion-deps-1.6.16.tar.gz

[root@localhost svn]# ll
总用量 12248
drwxr-xr-x. 16 centos centos    4096 2月  23 2011 subversion-1.6.16
-rw-r--r--.  1 root   root   7516368 10月 16 15:19 subversion-1.6.16.tar.gz
-rw-r--r--.  1 root   root   5014005 10月 16 15:19 subversion-deps-1.6.16.tar.gz

## 安装
[root@localhost svn]# cd subversion-1.6.16 # 进入安装目录  
[root@localhost subversion-1.6.16]# ./configure --prefix=/home/centos/subversion --without-berkeley-db # 安装到/home/centos/subversion 目录下，不使用bdb数据库存储数据
[root@localhost subversion-1.6.16]# make  
[root@localhost subversion-1.6.16]# make install

## 验证
[root@localhost /]# cd /home/centos/subversion/bin/
[root@localhost bin]# ./svn --version
svn，版本 1.6.16 (r1073529)
   编译于 Oct 16 2020，16:04:37

版权所有 (C) 2000-2009 CollabNet。
Subversion 是开放源代码软件，请参阅 http://subversion.apache.org/ 站点。
此产品包含由 CollabNet (http://www.Collab.Net/) 开发的软件。

可使用以下的版本库访问模块: 

* ra_neon : 通过 WebDAV 协议使用 neon 访问版本库的模块。
  - 处理“http”方案
* ra_svn : 使用 svn 网络协议访问版本库的模块。
  - 处理“svn”方案
* ra_local : 访问本地磁盘的版本库模块。
  - 处理“file”方案
* ra_serf : 通过 WebDAV 协议使用 serf 访问版本库的模块。
  - 处理“http”方案
  - 处理“https”方案
```
        
#### 1.2.4.3. 配置环境变量
`/etc/profile`添加以下内容：
        
```bash
## svn
PATH=$PATH:/home/centos/subversion/bin
```
         
执行`source /etc/profile`。
        
## 1.3. 配置
### 1.3.1. 创建仓库
```bash
[root@localhost /]# svnadmin create /home/centos/test
```
      
### 1.3.2. 其他配置
#### 1.3.2.1. 账户和密码
```bash
## 进入到上一步创建的仓库路径
[root@localhost subversion]# cd /home/centos/test/conf
[root@localhost conf]# vim passwd 
```
       
创建一些测试的用户。
       
```html
### This file is an example password file for svnserve.
### Its format is similar to that of svnserve.conf. As shown in the
### example below it contains one section labelled [users].
### The name and password for each user follow, one account per line.

[users]
# harry = harryssecret
# sally = sallyssecret
## 添加两个账号
admin = admin
test = test
```
            
#### 1.3.2.2. 权限
```bash
[root@localhost conf]# vim authz 
```
       
修改上一步创建的用户的权限。
      
```html
### This file is an example authorization file for svnserve.
### Its format is identical to that of mod_authz_svn authorization
### files.
### As shown below each section defines authorizations for the path and
### (optional) repository specified by the section name.
### The authorizations follow. An authorization line can refer to:
###  - a single user,
###  - a group of users defined in a special [groups] section,
###  - an alias defined in a special [aliases] section,
###  - all authenticated users, using the '$authenticated' token,
###  - only anonymous users, using the '$anonymous' token,
###  - anyone, using the '*' wildcard.
###
### A match can be inverted by prefixing the rule with '~'. Rules can
### grant read ('r') access, read-write ('rw') access, or no access
### ('').

[aliases]
# joe = /C=XZ/ST=Dessert/L=Snake City/O=Snake Oil, Ltd./OU=Research Institute/CN=Joe Average

[groups]
# harry_and_sally = harry,sally
# harry_sally_and_joe = harry,sally,&joe

## 创建两个组
admin = admin
developer = test

# [/foo/bar]
# harry = rw
# &joe = r
# * =

# [repository:/baz/fuz]
# @harry_and_sally = rw
# * = r


## 读写权限配置
[/]
admin = rw
test = r
```
      
#### 1.3.2.3. 服务svnserve.conf配置
```bash
[root@localhost conf]# vim svnserve.conf 
```
       
添加以下内容：
       
```html
[general]
#匿名访问的权限，可以是read,write,none,默认为read
anon-access=none
#使授权用户有写权限 
auth-access=write
#密码数据库的路径 
password-db=passwd
#访问控制文件 
authz-db=authz
#认证命名空间，subversion会在认证提示里显示，并且作为凭证缓存的关键字 
realm=/data/svn/repositories
```
         
## 1.4. 启动svn
```bash
## 查看svn的服务是否启动
[root@localhost /]# ps -ef|grep svn
root      89681   1922  0 16:39 pts/0    00:00:00 grep --color=auto svn

## 启动
[root@localhost /]# svnserve -d -r /home/centos/test  --listen-port=3690 

## 开放端口
[root@localhost /]# netstat -tunlp | grep 3690
tcp        0      0 0.0.0.0:3690            0.0.0.0:*               LISTEN      92966/svnserve      
[root@localhost /]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since 五 2020-10-16 15:45:25 CST; 56min ago
     Docs: man:firewalld(1)
 Main PID: 797 (firewalld)
    Tasks: 2
   CGroup: /system.slice/firewalld.service
           └─797 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

10月 16 15:45:24 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
10月 16 15:45:25 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
10月 16 15:45:26 localhost.localdomain firewalld[797]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure configuration option. It will b...g it now.
Hint: Some lines were ellipsized, use -l to show in full.
[root@localhost /]# firewall-cmd --zone=public --add-port=3690/tcp --permanent
success
[root@localhost /]# firewall-cmd --reload
success
[root@localhost /]# firewall-cmd --zone=public --query-port=3690/tcp
yes
```
       
## 1.5. 连接
使用 TortoiseSVN , 输入地址`svn://ip/`即可 , 再输入用户名和密码就能访问了
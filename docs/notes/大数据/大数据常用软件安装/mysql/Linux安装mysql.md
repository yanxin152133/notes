# 1. Linux安装mysql
>系统版本：CentOS Linux release 7.8.2003 (Core)  
      
>mysql：5.6.49
     
## 1.1. 检测
安装前，我们可以检测系统是否自带安装 MySQL:
    
```bash
rpm -qa | grep mysql
```
        
如果你系统有安装，那可以选择进行卸载:
       
```bash
rpm -e mysql　　// 普通删除模式
rpm -e --nodeps mysql　　// 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
```
         
## 1.2. 添加MySQL`Yum`源
添加MySQLYum源到系统源列表中：
- MySQLYum源下载页： http://dev.mysql.com/downloads/repo/yum/
          

![](https://live.staticflickr.com/65535/50297288191_5acd24b250_k.jpg)
         
本教程下载的版本为：`mysql80-community-release-el7-3.noarch.rpm`，安装命令如下：
       
```bash
sudo rpm -ivh mysql80-community-release-el7-3.noarch.rpm
```
        
## 1.3. 选择一个发行系列
在`MySQLYum`源的内部，不同的发行系列对应了MySQL社区服务器的不同资源子节点。子节点默认为最新的正式版本(当前为 5.8)，而其它资源子节点(如5.5､5.6等)默认是不可用的。
          
可以通过以下命令，查询子资源是否可用(在dnf-enabled的系统中使用dnf命令替代yum)：
        
```bash
yum repolist all | grep mysql
```
       
运行该命令：
      
```bash
[root@localhost hadoop]# yum repolist all | grep mysql
mysql-cluster-7.5-community/x86_64  MySQL Cluster 7.5 Community     禁用
mysql-cluster-7.5-community-source  MySQL Cluster 7.5 Community - S 禁用
mysql-cluster-7.6-community/x86_64  MySQL Cluster 7.6 Community     禁用
mysql-cluster-7.6-community-source  MySQL Cluster 7.6 Community - S 禁用
mysql-cluster-8.0-community/x86_64  MySQL Cluster 8.0 Community     禁用
mysql-cluster-8.0-community-source  MySQL Cluster 8.0 Community - S 禁用
mysql-connectors-community/x86_64   MySQL Connectors Community      启用:    165
mysql-connectors-community-source   MySQL Connectors Community - So 禁用
mysql-tools-community/x86_64        MySQL Tools Community           启用:    115
mysql-tools-community-source        MySQL Tools Community - Source  禁用
mysql-tools-preview/x86_64          MySQL Tools Preview             禁用
mysql-tools-preview-source          MySQL Tools Preview - Source    禁用
mysql55-community/x86_64            MySQL 5.5 Community Server      禁用
mysql55-community-source            MySQL 5.5 Community Server - So 禁用
mysql56-community/x86_64            MySQL 5.6 Community Server      禁用
mysql56-community-source            MySQL 5.6 Community Server - So 禁用
mysql57-community/x86_64            MySQL 5.7 Community Server      禁用
mysql57-community-source            MySQL 5.7 Community Server - So 禁用
mysql80-community/x86_64            MySQL 8.0 Community Server      启用:    193
mysql80-community-source            MySQL 8.0 Community Server - So 禁用
```
         
本次安装的版本为`mysql5.6`。
        
如果要安装最新的发行版，则不需要其它配置。而要指定安装一个早期版本，则需要在安装前禁用最新发生版，并启用指定要安装的版本。可以通过修改`/etc/yum.repos.d/mysql-community.repo`文件实现，该文件是一个指定子资源的入口：
         
启用5.6版本的源，就需要将如下所示的5.6版本源设置为enabled=1并将5.8版本源设置为enabled=0： 
       
```html
# Enable to use MySQL 5.5
[mysql55-community]
name=MySQL 5.5 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.5-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

# Enable to use MySQL 5.6
[mysql56-community]
name=MySQL 5.6 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

# Enable to use MySQL 5.7
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-connectors-community]
name=MySQL Connectors Community
baseurl=http://repo.mysql.com/yum/mysql-connectors-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-tools-community]
name=MySQL Tools Community
baseurl=http://repo.mysql.com/yum/mysql-tools-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-tools-preview]
name=MySQL Tools Preview
baseurl=http://repo.mysql.com/yum/mysql-tools-preview/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-cluster-7.5-community]
name=MySQL Cluster 7.5 Community
baseurl=http://repo.mysql.com/yum/mysql-cluster-7.5-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-cluster-7.6-community]
name=MySQL Cluster 7.6 Community
baseurl=http://repo.mysql.com/yum/mysql-cluster-7.6-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql-cluster-8.0-community]
name=MySQL Cluster 8.0 Community
baseurl=http://repo.mysql.com/yum/mysql-cluster-8.0-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```
        
再次查询子资源是否可用(在dnf-enabled的系统中使用dnf命令替代yum)：
       
```bash
[root@localhost hadoop]# yum repolist all | grep mysql
mysql-cluster-7.5-community/x86_64  MySQL Cluster 7.5 Community     禁用
mysql-cluster-7.5-community-source  MySQL Cluster 7.5 Community - S 禁用
mysql-cluster-7.6-community/x86_64  MySQL Cluster 7.6 Community     禁用
mysql-cluster-7.6-community-source  MySQL Cluster 7.6 Community - S 禁用
mysql-cluster-8.0-community/x86_64  MySQL Cluster 8.0 Community     禁用
mysql-cluster-8.0-community-source  MySQL Cluster 8.0 Community - S 禁用
mysql-connectors-community/x86_64   MySQL Connectors Community      启用:    165
mysql-connectors-community-source   MySQL Connectors Community - So 禁用
mysql-tools-community/x86_64        MySQL Tools Community           启用:    115
mysql-tools-community-source        MySQL Tools Community - Source  禁用
mysql-tools-preview/x86_64          MySQL Tools Preview             禁用
mysql-tools-preview-source          MySQL Tools Preview - Source    禁用
mysql55-community/x86_64            MySQL 5.5 Community Server      禁用
mysql55-community-source            MySQL 5.5 Community Server - So 禁用
mysql56-community/x86_64            MySQL 5.6 Community Server      启用:    547
mysql56-community-source            MySQL 5.6 Community Server - So 禁用
mysql57-community/x86_64            MySQL 5.7 Community Server      禁用
mysql57-community-source            MySQL 5.7 Community Server - So 禁用
mysql80-community/x86_64            MySQL 8.0 Community Server      禁用
mysql80-community-source            MySQL 8.0 Community Server - So 禁用
```
       
## 1.4. 使用`Yum`安装MySQL
```bash
sudo yum install mysql-community-server
```
       
以上命令用于安装MySQL的服务端模块，安装其它模块命令格式也类似。
       
## 1.5. 启动MySQL服务器
使用以下命令启动MySQL服务器：
```bash
sudo service mysqld start
```
        
对于基于EL7和EL8的平台，这是首选命令：
     
```bahs
sudo systemctl start mysqld.service
```
      
您可以使用以下命令检查MySQL服务器的状态：
       
```bash
sudo service mysqld status
```
       
对于基于EL7和EL8的平台，这是首选命令：
      
```bash
sudo systemctl status mysqld.service
```
       
### 1.5.1. mysql5.6
mysql5.6直接执行以下命令：
       
```bash
mysql -uroot -p     ## 直接回车，不需要输入密码
use mysql;
update user set password=PASSWORD("这里输入root用户密码") where User='root';
flush privileges;  ## 刷新权限
```
       
### 1.5.2. mysql5.7及更高版本
MySQL服务器初始化（`自MySQL 5.7起`）：在服务器初次启动时，假定服务器的数据目录为空，则会发生以下情况：
        
- 服务器已初始化。
- SSL证书和密钥文件在数据目录中生成。
- 该`validate_password`插件安装并启用。
             
- `'root'@'localhost'`创建 一个超级用户帐户。设置超级用户的密码并将其存储在错误日志文件中。要显示它，请使用以下命令：
      
```bash
sudo grep 'temporary password' /var/log/mysqld.log
```
       
通过使用生成的临时密码登录并尽快更改root密码，并为超级用户帐户设置自定义密码：
        
```bash
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
        
>注意        
>MySQL的`validate_password`插件默认安装。这将要求密码至少包含一个`大写字母`，一个`小写字母`，一位`数字`和一个`特殊字符`，并且密码总长度至少为`8`个字符。
           
## 1.6. utf-8 编码设置
输入以下命令：
      
```bash
sudo vim /etc/my.cnf
```
       
将以下内容添加到该文件中：
      
```html
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
collation-server=utf8_general_ci
character-set-server=utf8
init-connect='SET NAMES utf8'
```
         
重启`mysql`服务：
      
```bash
sudo service mysqld restart
```
         
查看相关命令输出的信息：
      
```html
status;
show variables like'char%';
```
           
## 1.7. 设置远程连接
```html
use mysql;
grant all privileges on *.* to 'root'@'%' identified by '输入远程连接的密码' with grant option; //为root添加远程连接的能力
flush privileges;
select host,user from user; //查看修改是否成功。
```
      
## 1.8. 开放端口
在`Centos 7`中防火墙由`firewalld`来管理，而不是`iptables`。
            
```bash
systemctl status firewalld    ## 查看 firewalld 状态
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent   ## 开放3306端口
sudo firewall-cmd --reload    ## 重新加载
sudo firewall-cmd --zone=public --query-port=3306/tcp   ## 查看修改
```
        
## 1.9. 加固MySQL安全(仅MySQL5.6适用)
mysql_secure_installation程序可以保证一些重要操作的安全性，如：修改root用户的密码、删除匿名用户等。如果安装是MySQL 5.6，应该保证始终运行该程序：

```bash
mysql_secure_installation
```
        
参考：
      
```bash
[root@server1 ~]# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL

SERVERS IN PRODUCTION USE! PLEASE READ EACH STEP CAREFULLY!

In order to log into MySQL to secure it, we'll need the current

password for the root user. If you've just installed MySQL, and

you haven't set the root password yet, the password will be blank,

so you should just press enter here.

Enter current password for root (enter for none):<–初次运行直接回车

OK, successfully used password, moving on…

Setting the root password ensures that nobody can log into the MySQL

root user without the proper authorisation.

Set root password? [Y/n] <– 是否设置root用户密码，输入y并回车或直接回车

New password: <– 设置root用户的密码

Re-enter new password: <– 再输入一次你设置的密码

Password updated successfully!

Reloading privilege tables..

… Success!

By default, a MySQL installation has an anonymous user, allowing anyone

to log into MySQL without having to have a user account created for

them. This is intended only for testing, and to make the installation

go a bit smoother. You should remove them before moving into a

production environment.

Remove anonymous users? [Y/n]  <– 是否删除匿名用户,生产环境建议删除，所以直接回车

… Success!

Normally, root should only be allowed to connect from 'localhost'. This

ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n]   <–是否禁止root远程登录,根据自己的需求选择Y/n并回车,建议禁止

… Success!

By default, MySQL comes with a database named 'test' that anyone can

access. This is also intended only for testing, and should be removed

before moving into a production environment.

Remove test database and access to it? [Y/n]  <– 是否删除test数据库,直接回车

- Dropping test database…

… Success!

- Removing privileges on test database…

… Success!

Reloading the privilege tables will ensure that all changes made so far

will take effect immediately.

Reload privilege tables now? [Y/n] <– 是否重新加载权限表，直接回车

… Success!

Cleaning up…

All done! If you've completed all of the above steps, your MySQL

installation should now be secure.

Thanks for using MySQL!

```

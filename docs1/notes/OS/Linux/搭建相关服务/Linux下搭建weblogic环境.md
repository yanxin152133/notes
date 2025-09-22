# 1. Linux下搭建weblogic环境
>系统版本：CentOS Linux release 7.8.2003 (Core)    
      
>JDK版本：jdk 8u261
       
>weblogic：wls10.3.6
        
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
      
# 3. weblogic
## 3.1. 下载
官网下载：https://www.oracle.com/middleware/technologies/weblogic-server-downloads.html
         
![weblogic下载](https://live.staticflickr.com/65535/50474696236_fdd3576528_k.jpg)
      
## 3.2. 创建用户以及相关目录
输入以下命令：
         
```bash
[root@localhost /]# mkdir -p /u01/
[root@localhost /]# groupadd weblogic
[root@localhost /]# useradd -g weblogic -d /u01/weblogic weblogic
[root@localhost /]# passwd weblogic
#更改用户 weblogic 的密码。
[root@localhost /]# chown -R weblogic:weblogic /u01
```
        
## 3.3. 安装
```bash
[weblogic@localhost u01]$ ls        ## 查看是否上传成功
weblogic  wls1036_generic.jar

## 执行以下命令开始安装
[weblogic@localhost u01]$ java -jar wls1036_generic.jar 
无法实例化 GUI, 默认进入控制台模式。
Extracting 0%....................................................................................................100%





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

欢迎使用:
-------------

此安装程序将引导您完成 WebLogic 10.3.6.0 的安装。键入 "Next" 或按 Enter 键继续下一个提示。如果您希望更改以前输入的数据, 
请键入 "Previous"。您可以随时通过键入 "Exit" 退出安装程序。




输入 [退出][下一步]>           ## 回车进行下一步



<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

选择中间件主目录:
-------------------------

    "中间件主目录" = [输入新值或使用默认值 "/u01/weblogic/Oracle/Middleware"]




输入新值 中间件主目录 或 [退出][上一步][下一步]>/u01/weblogic     ## 输入新的路径

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

选择中间件主目录:
-------------------------

    "中间件主目录" = [/u01/weblogic]

使用以上值或选择另一选项:
    1 - 输入新值 中间件主目录
    2 - 更改为默认值 [/u01/weblogic/Oracle/Middleware]




输入要选择的选项编号 或 [退出][上一步][下一步]>      ## 回车进行下一步

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

选择中间件主目录:
-------------------------

    警告

/u01/weblogic目录非空。是否继续安装?




输入 [退出][上一步][下一步]>      ## 回车进行下一步

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

注册安全更新:
-------------------

请提供用于接收安全更新的电子邮件地址以及 以启动配置管理器。

   1|电子邮件:[]
   2|My Oracle Support 口令:[]
   3|接收安全更新:[Yes]



输入要选择的索引号 或 [退出][上一步][下一步]> 3     ## 输入3，然后回车





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

注册安全更新:
-------------------

请提供用于接收安全更新的电子邮件地址以及 以启动配置管理器。

    "接收安全更新:" = [输入新值或使用默认值 "Yes"]



输入 [Yes][No]? No     ## 输入No，然后回车





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

注册安全更新:
-------------------

请提供用于接收安全更新的电子邮件地址以及 以启动配置管理器。

    "接收安全更新:" = [输入新值或使用默认值 "Yes"]


    ** 是否希望绕过配置管理器的启动过程并且
    **  不接收配置中存在严重安全问题的通知?


输入 [Yes][No]? Yes    ## 输入Yes，然后回车





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

注册安全更新:
-------------------

请提供用于接收安全更新的电子邮件地址以及 以启动配置管理器。

   1|电子邮件:[]
   2|My Oracle Support 口令:[]
   3|接收安全更新:[No]



输入要选择的索引号 或 [退出][上一步][下一步]>     ## 回车，进行下一步

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

选择安装类型:
-------------------

选择您要执行的安装类型。 

 ->1|典型
    |  安装以下产品和组件:
    | - WebLogic Server
    | - Oracle Coherence

   2|定制
    |  选择要安装的软件产品和组件并执行可选配置。





输入要选择的索引号 或 [退出][上一步][下一步]> 2      ## 输入2，然后回车

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

选择产品和组件:
----------------------

    发行版 10.3.6.0
    |_____WebLogic Server [1] x
    |    |_____Core Application Server [1.1] x
    |    |_____Administration Console [1.2] x
    |    |_____Configuration Wizard and Upgrade Framework [1.3] x
    |    |_____Web 2.0 HTTP Pub-Sub Server [1.4] x
    |    |_____WebLogic SCA [1.5] x
    |    |_____WebLogic JDBC Drivers [1.6] x
    |    |_____Third Party JDBC Drivers [1.7] x
    |    |_____WebLogic Server Clients [1.8] x
    |    |_____WebLogic Web Server Plugins [1.9] x
    |    |_____UDDI and Xquery Support [1.10] x
    |    |_____Server Examples [1.11] 
    |    |_____Evaluation Database [1.12] x
    |_____Oracle Coherence [2] x
         |_____Coherence Product Files [2.1] x
         |_____Coherence Examples [2.2] 

    *安装预计所需的大小: 690.2 MB




输入与方括号中完全相同的数字以切换选择 或 [退出][上一步][下一步]>     ## 回车进行下一步

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

JDK 选择 (所有 * 都指示 Oracle 提供的 VM):
----------------------------------------------------

将安装所选 JDK。如果已安装, 默认值将 用于脚本字符串替换。

   1|添加本地 JDK
   2|/usr/jdk1.8.0_261[x]



   *安装预计所需的大小: 690.2 MB


输入 1 以添加, 或者输入 >= 2 可切换选定内容  或 [退出][上一步][下一步]>     ## 回车进行下一步

<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

选择产品安装目录:
-------------------------

中间件主目录: [/u01/weblogic]

产品安装目录:


   1|WebLogic Server: [/u01/weblogic/wlserver_10.3]
   2|Oracle Coherence: [/u01/weblogic/coherence_3.7]




输入要选择的索引号 或 [退出][上一步][下一步]>    ## 回车进行下一步


将安装下列产品和 JDK:
-----------------------------

    WebLogic Platform 10.3.6.0
    |_____WebLogic Server
    |    |_____Core Application Server
    |    |_____Administration Console
    |    |_____Configuration Wizard and Upgrade Framework
    |    |_____Web 2.0 HTTP Pub-Sub Server
    |    |_____WebLogic SCA
    |    |_____WebLogic JDBC Drivers
    |    |_____Third Party JDBC Drivers
    |    |_____WebLogic Server Clients
    |    |_____WebLogic Web Server Plugins
    |    |_____UDDI and Xquery Support
    |    |_____Evaluation Database
    |_____Oracle Coherence
         |_____Coherence Product Files

    *安装预计所需的大小: 690.3 MB




输入 [退出][上一步][下一步]>      ## 回车进行下一步
十月 14, 2020 9:31:04 上午 java.util.prefs.FileSystemPreferences$1 run
信息: Created user preferences directory.





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

正在安装文件...

0%          25%          50%          75%          100%
[------------|------------|------------|------------]
[***************************************************]


正在执行字符串替换...





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

正在配置 OCM...

0%          25%          50%          75%          100%
[------------|------------|------------|------------]
[***************************************************]


正在创建域...





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

安装完成


祝贺您! 安装完成。


按 [Enter] 键继续或键入 [退出]>     ## 回车结束安装





<-------------------- Oracle Installer - WebLogic 10.3.6.0 ------------------->

清除过程正在进行中...
```
       
## 3.4. 创建Weblogic域
输入以下命令：
      
```bash
[weblogic@localhost /]$ cd /u01/weblogic/wlserver_10.3/common/bin/
[weblogic@localhost bin]$ ./config.sh -mode=console
```
         
执行以下步骤进行配置：
       
```bash
[weblogic@localhost bin]$ ./config.sh -mode=console
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=128m; support was removed in 8.0
Java HotSpot(TM) 64-Bit Server VM warning: You have loaded library /u01/weblogic/wlserver_10.3/common/lib/libjni.so which might have disabled stack guard. The VM will try to fix the stack guard now.
It's highly recommended that you fix the library with 'execstack -c <libfile>', or link it with '-z noexecstack'.





<--------------------------- Fusion Middleware 配置向导 -------------------------->

欢迎使用:
-------------

在创建和扩展域之间选择。根据您的选择,  配置向导将引导您完成生成新域或扩展现有域的步骤。

 ->1|创建新的 WebLogic 域
    |    在您的项目目录中创建 WebLogic 域。  

   2|扩展现有的 WebLogic 域
    |    使用此选项可以向现有域添加新组件以及修改配置设置。 





输入要选择的索引号 或 [退出][下一步]> 1       ## 输入1，然后回车





<--------------------------- Fusion Middleware 配置向导 -------------------------->

选择域源:
-------------

选择要从中创建域的源。可以通过 在所需的组件中选择或在现有域模板列表中选择来创建域。

 ->1|选择 Weblogic Platform 组件
    |    您可以选择希望在域中支持的 Weblogic 组件。 

   2|选择定制模板
    |    如果要使用现有模板, 请选择此选项。 此模板可以是使用模板构建器创建的定制模板。 
输入要选择的索引号 或 [退出][上一步][下一步]> 1    ## 输入1，然后回车





<--------------------------- Fusion Middleware 配置向导 -------------------------->

应用程序模板选择:
-------------------------

 

    可用模板
    |_____Basic WebLogic Server Domain - 10.3.6.0 [wlserver_10.3]x
    |_____Basic WebLogic SIP Server Domain - 10.3.6.0 [wlserver_10.3] [2] 
    |_____WebLogic Advanced Web Services for JAX-RPC Extension - 10.3.6.0 [wlserver_10.3] [3] 
    |_____WebLogic Advanced Web Services for JAX-WS Extension - 10.3.6.0 [wlserver_10.3] [4] 



输入与方括号中完全相同的数字以切换选择 或 [退出][上一步][下一步]>     ## 回车进行下一步

<--------------------------- Fusion Middleware 配置向导 -------------------------->

编辑域信息:
----------------

    | Name |    Value    |
   _|______|_____________|
   1| *名称: | base_domain |




输入以下内容的值 "名称" 或 [退出][上一步][下一步]>     ## 回车进行下一步

<--------------------------- Fusion Middleware 配置向导 -------------------------->

为此域选择目标域目录:
-------------------------------

    "目标位置" = [输入新值或使用默认值 "/u01/weblogic/user_projects/domains"]




输入新值 目标位置 或 [退出][上一步][下一步]>    ## 回车进行下一步





<--------------------------- Fusion Middleware 配置向导 -------------------------->

配置管理员用户名和口令:
----------------------------------

创建一个要分配到管理员角色的用户。 此用户是用于启动开发模式服务器的默认管理员。

    |   Name   |                  Value                  |
   _|__________|_________________________________________|
   1|   *名称:   |                weblogic                 |
   2|  *用户口令:  |                                         |
   3| *确认用户口令: |                                         |
   4|   说明:    | This user is the default administrator. |

使用以上值或选择另一选项:
    1 - 修改 "名称"
    2 - 修改 "用户口令"
    3 - 修改 "确认用户口令"
    4 - 修改 "说明"




输入要选择的选项编号 或 [退出][上一步][下一步]>  2    ## 输入2，然后回车





<--------------------------- Fusion Middleware 配置向导 -------------------------->

配置管理员用户名和口令:
----------------------------------

创建一个要分配到管理员角色的用户。 此用户是用于启动开发模式服务器的默认管理员。

    "*用户口令:" = []




输入新值 *用户口令: 或 [退出][重置][接受]>xxxxx  ## 输入自己设置的密码，然后回车




<--------------------------- Fusion Middleware 配置向导 -------------------------->

配置管理员用户名和口令:
----------------------------------

创建一个要分配到管理员角色的用户。 此用户是用于启动开发模式服务器的默认管理员。

    |   Name   |                  Value                  |
   _|__________|_________________________________________|
   1|   *名称:   |                weblogic                 |
   2|  *用户口令:  |                  *****                  |
   3| *确认用户口令: |                                         |
   4|   说明:    | This user is the default administrator. |

使用以上值或选择另一选项:
    1 - 修改 "名称"
    2 - 修改 "用户口令"
    3 - 修改 "确认用户口令"
    4 - 修改 "说明"
    5 - 放弃更改




输入要选择的选项编号 或 [退出][上一步][下一步]> 3      ## 输入3，然后回车






<--------------------------- Fusion Middleware 配置向导 -------------------------->

配置管理员用户名和口令:
----------------------------------

创建一个要分配到管理员角色的用户。 此用户是用于启动开发模式服务器的默认管理员。

    "*确认用户口令:" = []




输入新值 *确认用户口令: 或 [退出][重置][接受]> xxxxx    ## 输入上一步设置的密码，然后回车





<--------------------------- Fusion Middleware 配置向导 -------------------------->

配置管理员用户名和口令:
----------------------------------

创建一个要分配到管理员角色的用户。 此用户是用于启动开发模式服务器的默认管理员。

    |   Name   |                  Value                  |
   _|__________|_________________________________________|
   1|   *名称:   |                weblogic                 |
   2|  *用户口令:  |                  *****                  |
   3| *确认用户口令: |                  *****                  |
   4|   说明:    | This user is the default administrator. |

使用以上值或选择另一选项:
    1 - 修改 "名称"
    2 - 修改 "用户口令"
    3 - 修改 "确认用户口令"
    4 - 修改 "说明"
    5 - 放弃更改




输入要选择的选项编号 或 [退出][上一步][下一步]>       ## 回车进行下一步

<--------------------------- Fusion Middleware 配置向导 -------------------------->

域模式配置:
----------------

为此域启用开发或生产模式。 

 ->1|开发模式

   2|生产模式




输入要选择的索引号 或 [退出][上一步][下一步]> 2    ## 输入2，然后回车

<--------------------------- Fusion Middleware 配置向导 -------------------------->

Java SDK 选择:
----------------

 ->1|Sun SDK 1.8.0_261 @ /usr/jdk1.8.0_261
   2|其他 Java SDK




输入要选择的索引号 或 [退出][上一步][下一步]> 1     ## 输入1，然后回车

<--------------------------- Fusion Middleware 配置向导 -------------------------->

选择可选配置:
-------------------

   1|管理服务器 [ ]
   2|受管服务器, 集群和计算机 [ ]
   3|RDBMS 安全存储 [ ]



输入要选择的索引号 或 [退出][上一步][下一步]> 1     ## 输入1，然后回车

<--------------------------- Fusion Middleware 配置向导 -------------------------->

选择可选配置:
-------------------

   1|管理服务器 [x]
   2|受管服务器, 集群和计算机 [ ]
   3|RDBMS 安全存储 [ ]



输入要选择的索引号 或 [退出][上一步][下一步]>       ## 回车进行下一步

<--------------------------- Fusion Middleware 配置向导 -------------------------->

配置管理服务器:
----------------------

每个 WebLogic Server 域都必须有一个管理服务器。 该管理服务器用于执行管理任务。

    |       Name       |        Value        |
   _|__________________|_____________________|
   1|       *名称:       |     AdminServer     |
   2| *Listen address: | All Local Addresses |
   3|   Listen port:   |        7001         |
   4|    SSL 监听端口:     |         N/A         |
   5|     已启用 SSL:     |        false        |

使用以上值或选择另一选项:
    1 - 修改 "名称"
    2 - 修改 "Listen address"
    3 - 修改 "Listen port"
    4 - 修改 "已启用 SSL"




输入要选择的选项编号 或 [退出][上一步][下一步]>        ## 不需要修改的话直接回车




<--------------------------- Fusion Middleware 配置向导 -------------------------->

正在创建域...

0%          25%          50%          75%          100%
[------------|------------|------------|------------]
[***************************************************]


**** 域创建成功! ****
```
       
## 3.5. 启动Weblogic
```bash
[weblogic@localhost /]$ cd /u01/weblogic/user_projects/domains/base_domain/bin/
[weblogic@localhost bin]$ ls
nodemanager  server_migration  service_migration  setDomainEnv.sh  startManagedWebLogic.sh  startWebLogic.sh  stopManagedWebLogic.sh  stopWebLogic.sh
[weblogic@localhost bin]$ ./startWebLogic.sh 
## 之后输入账号密码，账号：weblogic   密码：配置中自己设置的密码
```
        
## 3.6. 防火墙设置
```bash
[root@localhost /]# netstat -tunlp | grep 7001    ## 查看端口占用
tcp6       0      0 fe80::e35a:eb45:31:7001 :::*                    LISTEN      7449/java           
tcp6       0      0 127.0.0.1:7001          :::*                    LISTEN      7449/java           
tcp6       0      0 172.16.36.130:7001      :::*                    LISTEN      7449/java           
tcp6       0      0 ::1:7001                :::*                    LISTEN      7449/java           
[root@localhost /]# systemctl status firewalld    ## 查看防火墙状态
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since 一 2020-10-12 10:16:47 CST; 1 day 23h ago
     Docs: man:firewalld(1)
 Main PID: 798 (firewalld)
    Tasks: 2
   CGroup: /system.slice/firewalld.service
           └─798 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

10月 12 10:16:42 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
10月 12 10:16:47 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
10月 12 10:16:48 localhost.localdomain firewalld[798]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure configuration option. It will b...g it now.
Hint: Some lines were ellipsized, use -l to show in full.
[root@localhost /]# firewall-cmd --zone=public --add-port=7001/tcp --permanent    ## 开放端口
success
[root@localhost /]# firewall-cmd --reload   ## 重载防火墙
success
[root@localhost /]# firewall-cmd --zone=public --query-port=7001/tcp   ## 查看是否修改成功
yes
```
          
# 4. web页面
网址为：`ip:70001/console`，如下：
           
![weblogic](https://live.staticflickr.com/65535/50475119391_44573d904d_k.jpg)
           

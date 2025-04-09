# bond配置
## 第一种方式(物理机、虚拟机centos7.X版本已检验，redhat版本测试未成功)
### 系统版本
     
```shell script
[root@localhost ~]# cat /etc/centos-release
CentOS Linux release 7.8.2003 (Core)
[root@localhost ~]# uname -r
3.10.0-1127.el7.x86_64
```
       
### 查看网卡信息
       
```shell script
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens32: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:56:32:22:7b brd ff:ff:ff:ff:ff:ff
    inet 172.16.36.131/24 brd 172.16.36.255 scope global noprefixroute dynamic ens32
       valid_lft 1800sec preferred_lft 1800sec
    inet6 fe80::4b5:27bb:4a2:1d0f/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:72:66:60 brd ff:ff:ff:ff:ff:ff
    inet 172.16.36.130/24 brd 172.16.36.255 scope global noprefixroute dynamic ens33
       valid_lft 1175sec preferred_lft 1175sec
    inet6 fe80::d10c:6844:3a6:526d/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
          
+ 其中**ens32**和**ens33**为物理网卡。
          
### 备份网卡配置文件
       
```shell script
[root@localhost ~]# cd /etc/sysconfig/network-scripts/
[root@localhost network-scripts]# ls
ifcfg-ens33  ifdown-isdn      ifdown-tunnel  ifup-isdn    ifup-Team
ifcfg-lo     ifdown-post      ifup           ifup-plip    ifup-TeamPort
ifdown       ifdown-ppp       ifup-aliases   ifup-plusb   ifup-tunnel
ifdown-bnep  ifdown-routes    ifup-bnep      ifup-post    ifup-wireless
ifdown-eth   ifdown-sit       ifup-eth       ifup-ppp     init.ipv6-global
ifdown-ippp  ifdown-Team      ifup-ippp      ifup-routes  network-functions
ifdown-ipv6  ifdown-TeamPort  ifup-ipv6      ifup-sit     network-functions-ipv6
[root@localhost network-scripts]# mkdir /tmp/net_back
[root@localhost network-scripts]# cp ifcfg-* /tmp/net_back/
[root@localhost network-scripts]# ls /tmp/net_back/
ifcfg-ens33  ifcfg-lo
```
       
### 使用nmcli命令配置bond
      
```shell script
[root@localhost network-scripts]# nmcli connection add type bond ifname bond0 mode 0
连接 "bond-bond0" (d4a88b99-e578-44e3-bd9b-a52c5c92fd79) 已成功添加。
[root@localhost network-scripts]# nmcli connection add type bond-slave ifname ens32 master bond0
连接 "bond-slave-ens32" (3287496b-92dc-427a-bd55-0d602f318ce9) 已成功添加。
[root@localhost network-scripts]# nmcli connection add type bond-slave ifname ens33 master bond0
连接 "bond-slave-ens33" (6c5129a7-00b9-410b-a54c-d10e36a005d0) 已成功添加。
[root@localhost network-scripts]# ls ifcfg-bond-*   //查看生成的bond配置文件
ifcfg-bond-bond0  ifcfg-bond-slave-ens32  ifcfg-bond-slave-ens33
```
        
#### bond模式     
+ balance-rr (0) –轮询模式，负载均衡（bond默认的模式）
+ active-backup (1) –主备模式（常用）
+ balance-xor (2)
+ broadcast (3）
+ 802.3ad (4) –聚合模式
+ balance-tlb (5)
+ balance-alb (6) 

### 配置IP
       
编辑**ifcfg-bond-bond0**：      
      
```shell script
BONDING_OPTS=mode=balance-rr
TYPE=Bond
BONDING_MASTER=yes
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static   ## 修改BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=bond-bond0
UUID=d4a88b99-e578-44e3-bd9b-a52c5c92fd79
DEVICE=bond0
ONBOOT=yes     ## 修改ONBOOT=yes
## 添加
IPADDR=172.16.36.131
NETMASK=255.255.255.0
GATEWAY=172.16.36.2
```
        
### 重启网络
      
```shell script
[root@localhost network-scripts]# service network restart
Restarting network (via systemctl):                        [  OK  ]
```
       
### 查看bond状态信息
       
```shell script
[root@localhost network-scripts]# cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: load balancing (round-robin)
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: ens32
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 00:50:56:32:22:7b
Slave queue ID: 0

Slave Interface: ens33
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 00:0c:29:72:66:60
Slave queue ID: 0
```
         
### 测试
        
```shell script
[root@localhost network-scripts]# ping 172.16.36.2
PING 172.16.36.2 (172.16.36.2) 56(84) bytes of data.
64 bytes from 172.16.36.2: icmp_seq=1 ttl=128 time=0.422 ms
64 bytes from 172.16.36.2: icmp_seq=1 ttl=128 time=0.449 ms (DUP!)
64 bytes from 172.16.36.2: icmp_seq=2 ttl=128 time=0.338 ms
64 bytes from 172.16.36.2: icmp_seq=2 ttl=128 time=0.367 ms (DUP!)
64 bytes from 172.16.36.2: icmp_seq=3 ttl=128 time=0.350 ms
64 bytes from 172.16.36.2: icmp_seq=3 ttl=128 time=0.380 ms (DUP!)
64 bytes from 172.16.36.2: icmp_seq=4 ttl=128 time=0.323 ms
64 bytes from 172.16.36.2: icmp_seq=4 ttl=128 time=0.354 ms (DUP!)
^C
--- 172.16.36.2 ping statistics ---
4 packets transmitted, 4 received, +4 duplicates, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 0.323/0.372/0.449/0.047 ms

## 将一个网卡禁用
[root@localhost network-scripts]# ifdown ens33
Device 'ens33' successfully disconnected.

[root@localhost network-scripts]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens32: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master bond0 state UP group default qlen 1000
    link/ether 00:50:56:32:22:7b brd ff:ff:ff:ff:ff:ff
3: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:72:66:60 brd ff:ff:ff:ff:ff:ff
7: bond0: <BROADCAST,MULTICAST,MASTER,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 00:50:56:32:22:7b brd ff:ff:ff:ff:ff:ff
    inet 172.16.36.131/24 brd 172.16.36.255 scope global noprefixroute bond0
       valid_lft forever preferred_lft forever
    inet6 fe80::4649:2ba8:388c:6ab9/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever


[root@localhost network-scripts]# cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: load balancing (round-robin)
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: ens32
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 00:50:56:32:22:7b
Slave queue ID: 0


## 测试bond是否配置成功
[root@localhost network-scripts]# ping 172.16.36.2
PING 172.16.36.2 (172.16.36.2) 56(84) bytes of data.
64 bytes from 172.16.36.2: icmp_seq=1 ttl=128 time=0.176 ms
64 bytes from 172.16.36.2: icmp_seq=2 ttl=128 time=3.76 ms
64 bytes from 172.16.36.2: icmp_seq=3 ttl=128 time=0.767 ms
64 bytes from 172.16.36.2: icmp_seq=4 ttl=128 time=2.06 ms
64 bytes from 172.16.36.2: icmp_seq=5 ttl=128 time=0.418 ms
64 bytes from 172.16.36.2: icmp_seq=6 ttl=128 time=2.68 ms
^C
--- 172.16.36.2 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5009ms
rtt min/avg/max/mdev = 0.176/1.646/3.764/1.304 ms

```
       
## 第二种方式（虚拟机centos7已检验，实体机centos7和redhat6.5、7测试未成功）
### 查看网卡信息
```shell script
[root@localhost ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens32: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:50:56:31:7a:67 brd ff:ff:ff:ff:ff:ff
    inet 172.16.36.132/24 brd 172.16.36.255 scope global noprefixroute dynamic ens32
       valid_lft 1707sec preferred_lft 1707sec
    inet6 fe80::c35b:2eac:84d6:4b84/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:72:66:60 brd ff:ff:ff:ff:ff:ff
    inet 172.16.36.130/24 brd 172.16.36.255 scope global noprefixroute dynamic ens33
       valid_lft 1706sec preferred_lft 1706sec
    inet6 fe80::d10c:6844:3a6:526d/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
      
### 检测是否支持bonding
      
![](../../pict/bonding1.png)
       
![](../../pict/bonding2.png)

### 配置ifcfg-bond0文件
```shell script
[root@localhost ~]# cd /etc/sysconfig/network-scripts/       ## 进入到该目录下

[root@localhost network-scripts]# vi ifcfg-bond0          ## 创建ifcfg-bond0

## 修改ifcfg-bond0 文件

BOOTPROTO=static
DEVICE=bond0
NAME=bond0
TYPE=Bond
BONDING_MASTER=yes
ONBOOT=yes
IPADDR=172.16.36.132        ##IP地址
NETMASK=255.255.255.0       ##子网掩码
GATEWAY=172.16.36.2         ##网关
BONDING_OPTS="miimon=100 mode=6"   ##mode=6为bond模式
```

### 修改网卡配置文件
**ifcfg-ens32**
       
```shell script
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none      ##将dhcp改为none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens32
UUID=684e3a7e-272c-48f8-b56f-19e8c20706ac
DEVICE=ens32
ONBOOT=yes     ##将no改为yes
## 添加
MASTER=bond0   
SLAVE=yes
```
        
**ifcfg-ens33**
          
```shell script
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none    ##将dhcp改为none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=e192e831-9cd0-4b6e-8c72-6d171ac16fe9
DEVICE=ens33
ONBOOT=yes     ##将no改为yes
## 添加
MASTER=bond0
SLAVE=yes
```

### 重启网络
```shell script
[root@localhost network-scripts]# service network restart
Restarting network (via systemctl):                        [  OK  ]
```
          
### 查看bond状态信息
```shell script
[root@localhost network-scripts]# cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: adaptive load balancing
Primary Slave: None
Currently Active Slave: ens32
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: ens32
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 00:50:56:31:7a:67
Slave queue ID: 0

Slave Interface: ens33
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 00:0c:29:72:66:60
Slave queue ID: 0
```
         
### 测试
```shell script
[root@localhost network-scripts]# ping 172.16.36.2
PING 172.16.36.2 (172.16.36.2) 56(84) bytes of data.
64 bytes from 172.16.36.2: icmp_seq=1 ttl=128 time=0.294 ms
64 bytes from 172.16.36.2: icmp_seq=2 ttl=128 time=0.305 ms
64 bytes from 172.16.36.2: icmp_seq=3 ttl=128 time=0.391 ms
64 bytes from 172.16.36.2: icmp_seq=4 ttl=128 time=0.267 ms
64 bytes from 172.16.36.2: icmp_seq=5 ttl=128 time=0.306 ms
64 bytes from 172.16.36.2: icmp_seq=6 ttl=128 time=0.188 ms
^C
--- 172.16.36.2 ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5004ms
rtt min/avg/max/mdev = 0.188/0.291/0.391/0.063 ms

[root@localhost network-scripts]# ifdown ens33
Device 'ens33' successfully disconnected.

[root@localhost ~]# cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: adaptive load balancing
Primary Slave: None
Currently Active Slave: ens32
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: ens32
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 00:50:56:31:7a:67
Slave queue ID: 0

[root@localhost ~]# ping 172.16.36.2
PING 172.16.36.2 (172.16.36.2) 56(84) bytes of data.
64 bytes from 172.16.36.2: icmp_seq=1 ttl=128 time=0.244 ms
64 bytes from 172.16.36.2: icmp_seq=2 ttl=128 time=0.224 ms
64 bytes from 172.16.36.2: icmp_seq=3 ttl=128 time=0.291 ms
64 bytes from 172.16.36.2: icmp_seq=4 ttl=128 time=0.316 ms
^C
--- 172.16.36.2 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 0.224/0.268/0.316/0.041 ms
```
          
## 第三种方式（物理机centos版本未进行测试，物理机redhat版本已检验，虚拟机redhat未进行测试，虚拟机centos已检验）
配置视频如下：
         
[使用nmtui配置bond](https://www.bilibili.com/video/BV1J5411s7BH/)

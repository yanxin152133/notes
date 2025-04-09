# 1. rsyslog
## 1.1. rsyslog.conf
编辑`/etc/rsyslog.conf`

```conf
#  /etc/rsyslog.conf    Configuration file for rsyslog.
#
#                       For more information see
#                       /usr/share/doc/rsyslog-doc/html/rsyslog_conf.html
#
#  Default logging rules can be found in /etc/rsyslog.d/50-default.conf


#################
#### MODULES ####
#################

module(load="imuxsock") # provides support for local system logging
#module(load="immark")  # provides --MARK-- message capability

# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")

# provides kernel logging support and enable non-kernel klog messages
module(load="imklog" permitnonkernelfacility="on")

###########################
#### GLOBAL DIRECTIVES ####
###########################

#
# Use traditional timestamp format.
# To enable high precision timestamps, comment out the following line.
#
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat

# Filter duplicated messages
$RepeatedMsgReduction on

#
# Set the default permissions for all log files.
#
$FileOwner syslog
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022
$PrivDropToUser syslog
$PrivDropToGroup syslog

#
# Where to place spool and state files
#
$WorkDirectory /home/openwrtlog

#
# Include all config files in /etc/rsyslog.d/
#
$IncludeConfig /etc/rsyslog.d/*.conf
```

## 1.2. openwrt 日志保存
### 1.2.1. 相关配置文件
新建文件`/etc/rsyslog.d/openwrt.conf`

```conf
:fromhost-ip, isequal, "192.168.2.1" /home/openwrtlog/openwrt.log
&~
```

## 1.3. 启动rstslog
```bash
service rsyslog restart
```

## 1.4. status
```bash
root@jellyfin:/etc/rsyslog.d# service rsyslog status
● rsyslog.service - System Logging Service
   Loaded: loaded (/lib/systemd/system/rsyslog.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2023-07-27 22:05:46 CST; 42min ago
     Docs: man:rsyslogd(8)
           http://www.rsyslog.com/doc/
 Main PID: 7818 (rsyslogd)
    Tasks: 10 (limit: 2312)
   CGroup: /system.slice/rsyslog.service
           └─7818 /usr/sbin/rsyslogd -n

Jul 27 22:05:46 jellyfin systemd[1]: Stopped System Logging Service.
Jul 27 22:05:46 jellyfin systemd[1]: Starting System Logging Service...
Jul 27 22:05:46 jellyfin rsyslogd[7818]: warning: ~ action is deprecated, consider using the 'stop' statement instead [v8.32.0 try http://www.rsysloJul 27 22:05:46 jellyfin systemd[1]: Started System Logging Service.
Jul 27 22:05:46 jellyfin rsyslogd[7818]: imuxsock: Acquired UNIX socket '/run/systemd/journal/syslog' (fd 3) from systemd.  [v8.32.0]
Jul 27 22:05:46 jellyfin rsyslogd[7818]: rsyslogd's groupid changed to 106
Jul 27 22:05:46 jellyfin rsyslogd[7818]: rsyslogd's userid changed to 102
Jul 27 22:05:46 jellyfin rsyslogd[7818]:  [origin software="rsyslogd" swVersion="8.32.0" x-pid="7818" x-info="http://www.rsyslog.com"] start
lines 1-18/18 (END)
```

# Linux 下 python 安装
>系统版本：CentOS Linux release 7.8.2003 (Core)
     
>python版本：Python-3.6.8
       
## 环境依赖
```bash
yum install gcc -y
yum install zlib -y
yum install zlib-devel -y
yum install openssl-devel -y
```

## 下载
```bash
[root@localhost usr]# wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tgz
```
        
## 解压编译
```bash
[root@localhost usr]# tar -zxvf Python-3.6.8.tgz
[root@localhost Python-3.6.8]# cd Python-3.6.8/
[root@localhost Python-3.6.8]# ./configure --prefix=/usr/app/python3.6
[root@localhost Python-3.6.8]# make && make install
```
           
## 环境变量配置
```bash
[root@localhost python3.6]# vim /etc/profile
```
        
在该文件最后添加以下内容：   
```html
export PYTHON_HOME=/usr/app/python3.6
export PATH=${PYTHON_HOME}/bin:$PATH
```
        
使配置生效： 
      
```bash
[root@localhost python3.6]# source /etc/profile
```
         
## 验证
```bash
[root@localhost python3.6]# python3
Python 3.6.8 (default, Aug 13 2020, 17:52:19)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 1+1
2
>>> exit()
```
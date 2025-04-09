# 1. ESXI 7.0 安装
## 1.1. 下载
前往 https://my.vmware.com/web/vmware/ 进行注册。

1. 登录到 VMware Customer Connect。
2. 导航到产品和帐户 > 所有产品。
3. 找到 VMware vSphere，然后单击查看下载组件。
4. 从选择版本下拉菜单中选择 VMware vSphere 版本。
5. 选择 VMware vSphere Hypervisor (ESXi) 的一个版本，然后单击转到下载。[版本区别](http://www.thope.com.tw/dl/vsphere/VMware+vSphere+%E7%89%88%E6%9C%AC%E5%8A%9F%E8%83%BD%E6%AF%94%E8%BC%83.pdf)
6. 下载 ESXi ISO 映像。

## 1.2. U盘启动盘
使用`rufus`制作启动盘。

## 1.3. 安装
参考[虚拟机VMware vSphere ESXi 7.0安装配置详细教程(附下载)](https://www.jb51.net/softjc/717737_all.html)

## 1.4. 核显直通
![](https://live.staticflickr.com/65535/51654439584_a94e4c168b_k.jpg)

![](https://live.staticflickr.com/65535/51654439579_65fcbc0564_k.jpg)

ssh登录执行`esxcli system settings kernel set -s vga -v FALSE`


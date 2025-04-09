# vmware局域网访问
点击上方工具栏中的`编辑`。
![1](https://live.staticflickr.com/65535/50263356151_a7c4012873_h.jpg)
       
点击`虚拟网络编辑器`。
![2](https://live.staticflickr.com/65535/50262697798_3da963cdfe_h.jpg)
        
点击`更改设置`。
![3](https://live.staticflickr.com/65535/50263355661_98b912a31c_h.jpg)
     
点击上方的`VMnet8 NAT 模式`，然后点击下方的`NAT设置`
![4](https://live.staticflickr.com/65535/50263355106_9212f91af7_h.jpg)
     
点击`添加`
![5](https://live.staticflickr.com/65535/50263539607_7ae03534b6_h.jpg)
      
填写规范如下：
- 主机端口：设置为自己想要的端口，如8888
- 类型：TCP
- 虚拟机IP地址：自己安装的虚拟机的所使用的IP
- 虚拟机端口：ssh连接就设置为22,其他的自行百度
        
![6](https://live.staticflickr.com/65535/50263538112_32f6a2a604_h.jpg)
      
在防火墙入站规则中添加对应的端口，防止局域网其他设备连接无响应。
![7](https://live.staticflickr.com/65535/50263354266_d500e47365_h.jpg)
          
局域网其他设备ssh连接方式
![8](https://live.staticflickr.com/65535/50263538397_c29f2ab5eb_h.jpg)
         
- ip:主机IP
- 端口：vmware设置的主机端口
       
命令方式登录：`MacBook-Pro ~ % ssh -P 8888 hadoop001@192.168.2.157`
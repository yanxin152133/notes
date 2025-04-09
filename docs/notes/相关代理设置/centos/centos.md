# 1. centos设置http代理
1. 打开配置文件

```html
vi /etc/profile
```

2. 添加

```html
export all_proxy="socks5://192.168.111.1:10808"
export http_proxy="http://192.168.111.1:10809"
export https_proxy=$http_proxy
```

3. 执行生效

```html
source /etc/profile
```
4. 查看ip变化

```html
curl ifconfig.io
```
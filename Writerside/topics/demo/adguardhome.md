# 1. adguardhome
## 1.1. 在线服务器
139.9.148.226

## 1.2. 运行
```html
docker run --name adguardhome --restart=always  -v /home/adguardhome/work:/opt/adguardhome/work -v /home/adguardhome/conf:/opt/adguardhome/conf -p 53:53/tcp -p 53:53/udp -p 3000:3000/tcp -it -d adguard/adguardhome
```
## 1.3. ubuntu 53 端口
[Port 53 already in use](https://github.com/AdguardTeam/AdGuardHome/issues/4283#issuecomment-1037397445)

## 1.4. DNS设置
### 1.4.1. 上游DNS服务器

```html
114.114.114.114
223.5.5.5
223.6.6.6
119.29.29.29
8.8.8.8
8.8.4.4
```

### 1.4.2. Bootstrap DNS 服务器
```html
114.114.114.114
223.5.5.5
223.6.6.6
119.29.29.29
8.8.8.8
8.8.4.4
```

### 1.4.3. DNS服务配置
- 启动EDNS客户端子网
- 启动DNSSEC
- 拦截模式为`默认`

### 1.4.4. DNS缓存配置
- 覆盖最小TTL值：`2`
- 覆盖最大TTL值：`3600`
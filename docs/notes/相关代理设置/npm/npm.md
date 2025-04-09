# 1. npm设置代理
## 1.1. http代理
```html
# 假设本地代理端口为8002
npm config set proxy http://127.0.0.1:8002
npm config set https-proxy http://127.0.0.1:8002
```

## 1.2. 查看代理
```html
npm config get
```

## 1.3. 清除代理
```html
npm config delete proxy
npm config delete https-proxy
```
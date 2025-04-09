# 1. git设置代理
## 1.1. 设置代理
```html
git config --global http.proxy http://127.0.0.1:1080

git config --global https.proxy https://127.0.0.1:1080

```

## 1.2. socks5代理
```html
git config --global http.proxy 'socks5://121.62.60.36:5003'
git config --global https.proxy 'socks5://121.62.60.36:5003'
```

## 1.3. 取消全局代理
```html
git config --global --unset http.proxy
git config --global --unset https.proxy
```

# 1. node和npm的更新与清除缓存
## 1.1. 清除缓存
```html
npm cache clean -f
```

## 1.2. 更新
### 1.2.1. npm
```html
npm install npm@latest -g
```

### 1.2.2. node
```html
npm cache clean -f  ## 先清理缓存

npm install -g n    ## 安装n模块

n stable            ## 安装最新的稳定版本

n 8.1.2             ## 自定义版本
```


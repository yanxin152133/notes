# Bootstrap简介与快速部署

## 简介

Bootstrap是一个功能强大、功能丰富的前端工具。在几分钟内构建任何东西-从原型到生产。

## 快速部署

### 通过<script>标签引入

通过<script>标签引入css和js文件。

```html

<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.8/dist/umd/popper.min.js"
        integrity="sha384-I7E8VVD/ismYTF4hNIPjVp/Zjvgyol6VFvRkX/vR+Vc4jQkC+hVqc2pM8ODewa9r"
        crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/js/bootstrap.min.js"
        integrity="sha384-7qAoOXltbVP82dhxHAUje59V5r2YsVfBafyUDxEdApLPmcdhBPg1DKg1ERo0BZlK"
        crossorigin="anonymous"></script>
```

完整代码如下：

```HTML
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/css/bootstrap.min.css" rel="stylesheet"
          integrity="sha384-LN+7fdVzj6u52u30Kp6M/trliBMCMKTyK833zpbD+pXdCLuTusPj697FH4R/5mcr" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.8/dist/umd/popper.min.js"
            integrity="sha384-I7E8VVD/ismYTF4hNIPjVp/Zjvgyol6VFvRkX/vR+Vc4jQkC+hVqc2pM8ODewa9r"
            crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/js/bootstrap.min.js"
            integrity="sha384-7qAoOXltbVP82dhxHAUje59V5r2YsVfBafyUDxEdApLPmcdhBPg1DKg1ERo0BZlK"
            crossorigin="anonymous"></script>
</head>
<body>
<h1>Hello, world!</h1>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-ndDqU0Gzau9qJ1lfW4pNLlhNTkCfHzAVBReH9diLvGRem5+R9g2FzA8ZGN954O5Q"
        crossorigin="anonymous"></script>
</body>
</html>
```

### 下载离线文件

#### 编译CSS和JS

[Bootstrap v5.3.7 的即用型编译代码](https://bootstrap.nodejs.cn/docs/5.3/getting-started/download/#)

#### 源文件

[源 Sass、JavaScript 和文档文件](https://bootstrap.nodejs.cn/docs/5.3/getting-started/download/#)

#### 下载示例

[Examples](https://github.com/twbs/bootstrap/releases/download/v5.3.7/bootstrap-5.3.7-examples.zip)

### 包管理器

#### npm

使用npm包在Node.js支持的应用中安装Bootstrap:

```bash
npm install bootstrap@5.3.7
```

#### yarn

使用yarn包在Node.js支持的应用中安装Bootstrap：

```Bash
yarn add bootstrap@5.3.7
```

## 插件扩展

![IntelliSense扩展](https://bootstrap.nodejs.cn/docs/5.3/assets/img/bootstrap-intellisense-autocomplete.png){
thumbnail="true" }

[IntelliSense扩展下载](https://marketplace.visualstudio.com/items?itemName=hossaini.bootstrap-intellisense)
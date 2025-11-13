# pip

## 确定哪个pip是哪个Python

```bash
C:\Users\Razer>pip -V
pip 24.0 from C:\Users\Razer\AppData\Local\Programs\Python\Python312\Lib\site-packages\pip (python 3.12)
```

## 安装pip

- 使用easy_install安装： 各种进入到`easy_install`脚本的目录下，然后运行`easy_inatall pip`
- 使用get-pip.py安装： 在下面的url下载get-pip.py脚本 `curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py` 然后运行：
  `python get-pip.py` 这个脚本会同时安装setuptools和wheel工具。
- 在linux下使用包管理工具安装pip： 例如，ubuntu下：`sudo apt-get install python-pip`。Fedora系下：
  `sudo yum install python-pip`
- 在windows下安装pip： 在C:\python27\scirpts下运行`easy_install pip`进行安装。

## 升级pip

1、在`Linux`或`masOS`中：`pip install -U pip `
2、在`windows`中：`python -m pip install -U pip`

## 基础使用

### 普通安装

```bash
pip install xxx     # xxx为需要安装的模块
```

### 指定版本安装

安装特定版本的package，通过使用`==`, `>=`, `<=`,` >`, `<`来指定一个版本号。

```bash
pip install 'Markdown<2.0'
pip install 'Markdown>2.0,<2.0.3'
```

### 卸载已安装的库

```bash
pip uninstall pillow
```

### 列出已经安装的库

```bash
pip list
```

### 将已经安装的库列表保存到文本文件中

```bash
pip freeze > requirements.txt
```

### 根据依赖文件批量安装库

```bash
pip install -r requirements.txt
```

## pip国内源

### 临时

```bash
#清华源
pip install markdown -i https://pypi.tuna.tsinghua.edu.cn/simple

# 阿里源
pip install markdown -i https://mirrors.aliyun.com/pypi/simple/

# 腾讯源
pip install markdown -i http://mirrors.cloud.tencent.com/pypi/simple

# 豆瓣源
pip install markdown -i http://pypi.douban.com/simple/

```

### 永久

```bash
# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# 阿里源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

# 腾讯源
pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple

# 豆瓣源
pip config set global.index-url http://pypi.douban.com/simple/

# 换回默认源
pip config unset global.index-url

```

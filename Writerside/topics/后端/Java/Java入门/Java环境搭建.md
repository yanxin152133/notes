# Java运行环境搭建

## JDK 与 JRE

- JRE（Java Runtime Environment）：是Java程序的运行时环境，包含JVM和运行时所需要的核心类库
- JDK（Java Development Kit）：是Java程序开发工具包，包含JRE和开发人员使用的工具

## Windows

### Windows JDK

[jdk8](https://www.oracle.com/webapps/redirect/signon?nexturl=https://download.oracle.com/otn/java/jdk/8u391-b13/b291ca3e0c8548b5a51d5a5f50063037/jdk-8u391-windows-x64.exe)

### 安装

双击安装

### 配置环境变量

鼠标右键`我的电脑`→`属性`→`高级系统设置`→`环境变量`→`系统变量`→`新建`。

![JAVA_HOME](https://farm8.staticflickr.com/7873/40480682053_903c92b01b_b.jpg)

在`系统变量`的`path`中添加 `%JAVA_HOME%\bin`、 `%JAVA_HOME%\jre\bin`。

### 验证

如下图所示：

![java](https://farm8.staticflickr.com/7860/40480681993_9a639b1dc2_b.jpg)

## Linux

### 下载 Java jdk

- [jdk 8u261](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

### 解压

```bash
[root@localhost usr]# tar -zxvf jdk-8u261-linux-x64.tar.gz
```

### 设置环境变量

```bash
[root@localhost jdk1.8.0_261]# vim /etc/profile
```

在该文件最后加上以下内容：

```html
export JAVA_HOME=/usr/jdk1.8.0_261      ## 具体路径根据本机jdk安装路径进行更改
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
```

执行`source`命令，使配置生效：

```bash
[root@localhost jdk1.8.0_261]# source /etc/profile
```

### 验证

```bash
[root@localhost jdk1.8.0_261]# java -version
java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)
```

显示以上内容则为安装成功。

## JAVA_HOME的bin目录

- java：这个可执行程序其实就是JVM，运行Java程序，就是启动JVM，然后让JVM执行指定的编译后的代码；
- javac：这是Java的编译器，它用于把Java源码文件（以.java后缀结尾）编译为Java字节码文件（以.class后缀结尾）；
- jar：用于把一组.class文件打包成一个.jar文件，便于发布；
- javadoc：用于从Java源码中自动提取注释并生成文档；
- jdb：Java调试器，用于开发阶段的运行调试。
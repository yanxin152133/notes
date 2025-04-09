# 1. IDEA 创建Spring Boot HelloWorld项目
## 1.1. IDEA 配置 maven
![IDEA 配置maven](https://live.staticflickr.com/7899/47400537232_35485ca322_o.png)

## 1.2. IDEA 创建Spring Boot项目
参考：[IDEA创建Spring Boot项目](https://c.biancheng.net/spring_boot/example.html)

## 1.3. HelloWorld 相关代码
新建`helloWorldController.java`文件：

```java
package com.example.helloworld.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
@Controller
public class helloWorldController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }
}

```
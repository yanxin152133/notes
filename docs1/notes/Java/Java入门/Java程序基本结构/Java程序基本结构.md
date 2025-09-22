# 1. Java 程序基本结构
```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

>Java是面向对象的语言，一个程序的基本单位就是`class`，`class`是关键字，`Hello`是`class`的名字。
>`public`是访问修饰符，表示该`class`是公开的；同时也可以修饰方法。
>关键字`static`是另一个修饰符，表示静态方法。
>`class`内部，定义了一个`main`主方法，返回值是`void`，表示没有返回值。
>Java入口程序规定的方法必须是静态方法，方法名必须为`main`，括号内的参数必须是String 数组。

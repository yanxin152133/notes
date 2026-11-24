# Java 程序基本结构

## 基本结构：

```java
/**
 *  可以用来自动创建文档的注释
 */
public class Hello { // 类型是Hello
    public static void main(String[] args) {
        // 向屏幕输出文本
        System.out.println("Hello, world!");
        /* 多行注释开始
                注释内容
        多行注释结束*/
    }
} // class定义结束
```

- `class 关键字`：用于在 Java 中声明一个类。
- `public 关键字`：一个表示可见性的访问修饰符。
- `static 关键字`：我们可以用它来声明任何一个方法，被 static 修饰后的方法称之为静态方法。静态方法不需要为其创建对象就能调用。
- `void 关键字`：表示该方法不返回任何值。
- `main 关键字`：表示该方法为主方法，也就是程序运行的入口。main() 方法由 Java 虚拟机执行，配合上 static 关键字后，可以不用创建对象就可以调用，可以节省不少内存空间。
- `String [] args`：main() 方法的参数，类型为 String 数组，参数名为 args。
- `System.out.println()`：一个 Java 语句，一般情况下是将传递的参数打印到控制台。System 是 java.lang 包中的一个 final 类，该类提供的设施包括标准输入，标准输出和错误输出流等等。out 是 System 类的静态成员字段，类型为 PrintStream，它与主机的标准输出控制台进行映射。println 是 PrintStream 类的一个方法，通过调用 print 方法并添加一个换行符实现的。

### 类名、方法名命名规则

**类名要求：**

- 类名必须以英文字母开头，后接字母、数字和下划线的组合
- 习惯以大写字母开头

**方法名要求：**

- 类名必须以英文字母开头，后接字母、数字和下划线的组合
- 首字母小写

### Java 三种注释

```Java
/**
 *  文档注释
 */

// 单行注释

/*
    多行注释
 */

```
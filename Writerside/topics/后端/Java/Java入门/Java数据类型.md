# Java数据类型

## 数据类型分类

![java数据类型分类](https://live.staticflickr.com/65535/48287266636_32ed239f62_z.jpg)

## 基本数据类型

### 整数类型

| 序号 | 数据类型      | 大小/位 | 可表示的数据范围                           |
|----|-----------|------|------------------------------------|
| 1  | byte(位)   | 8    | -2<sup>7</sup>到（2<sup>6</sup>-1）   |
| 2  | short(整型) | 16   | -2<sup>15</sup>到（2<sup>15</sup>-1） |
| 3  | int(整型)   | 32   | -2<sup>31</sup>到（2<sup>31</sup>-1） |
| 4  | long(长整型) | 64   | -2<sup>63</sup>到（2<sup>63</sup>-1） |

#### demo

```java
package com.java.chap03;

/**
 * @author Yan
 * @date 2019/7/16 13:52
 */
public class Demo1 {
    public static void main(String[] args) {
        //定义一个int类型的变量
        int a;
        //给变量a赋值
        a=1;
        System.out.println(a);

        //定义一个int类型的变量a2
        int a2=1;
        System.out.println("a2="+a2);

        //定义一个byte类型的变量b
        byte b=3;
        System.out.println("b="+b);

        //定义一个short类型的变量
        short s=4;
        System.out.println("s="+s);


        //定义一个long类型的变量l
        long l=5;
        System.out.println("l="+l);

        int a11=1;
        int a22=2;
        int a3=a11+a22;
        System.out.println("a1+a2="+a3);
    }
}
```

### 浮点类型

| 序号 | 数据类型        | 大小/位 | 可表示的数据范围                                                         |
|----|-------------|------|------------------------------------------------------------------|
| 1  | float(单精度)  | 32   | -3.4E38(-3.4x10<sup>38</sup>) 到 3.4E38(3.4x10<sup>38</sup>))     |
| 2  | double(双精度) | 64   | -1.7E308(-1.7x10<sup>308</sup>) 到 1.7E308(1.7x10<sup>308</sup>)) |

> 对于`float`类型，需要加上`f`后缀。

#### demo

```java
package com.java.chap03;

/**
 * @author Yan
 * @date 2019/7/16 13:53
 */
public class Demo2 {
    public static void main(String[] args) {
        //定义一个float类型的变量f
        //小数默认是double类型，所以必须加一个f,来表示float类型
        float f=1.1f;
        System.out.println("f="+f);

        //定义一个double类型变量d
        double d=1.2;
        System.out.println("d="+d);

        //获取float的最大值
        float maxF=Float.MAX_VALUE;
        System.out.println("float最大值："+maxF);
        //获取float的最小值
        float minF=Float.MIN_VALUE;
        System.out.println("float最小值："+minF);
    }
}

```

### 字符型

> 字符型常量有3种表示形式：直接通过单个字符来指定字符型常量，如‘A’，‘b’，‘5’；通过转义字符表示特殊字符型常量，如‘\n’,‘\\’；直接使用Unicode值来表示字符型常量，如‘\u66f9’，‘\yu950b’。

| 转义字符   | 说明  |
|--------|-----|
| \b     | 退格  |
| \n     | 换行  |
| \t     | 制表符 |
| \"     | 双引号 |
| \'     | 单引号 |
| \\|反斜杠 |
| \r     | 回车符 |

> `char`类型使用`单引号`，且仅有一个字符要和`双引号"`的字符串类型区分开。

#### demo

```java
package com.java.chap03;

/**
 * @author Yan
 * @date 2019/7/16 13:53
 */
public class Demo3 {
    public static void main(String[] args) {
        //定义一个单个字符
        char c1='A';
        System.out.println("c1="+c1);


        //定义一个反斜杠字符
        char c2='\\';
        System.out.println("c2="+c2);


        //用Unicode编码输出自己的名字
        char c3='\u66f9';
        char c4='\u950b';
        System.out.println("c3="+c3);
        System.out.println("c4="+c4);
    }
}
```

### 布尔类型

> 布尔类型的变量只有`true(真)`和`false(假)`两种。

> Java语言对布尔类型的存储并没有做规定，因为理论上存储布尔类型只需要1 bit，但是通常JVM内部会把boolean表示为4字节整数。

#### demo

```java
package com.java.chap03;

/**
 * @author Yan
 * @date 2019/7/16 13:53
 */
public class Demo4 {
    public static void main(String[] args) {
        //定义一个布尔类型变量b1
        boolean b1=true;
        System.out.println("b1="+b1);
        //定义一个布尔类型变量b2
        boolean b2=false;
        System.out.println("b2="+b2);
    }
}
```

## 引用数据类型

> 引用数据类型建立在基本数据类型的基础上，包括数组、类和接口。引用数据类型是由用户自定义，用来限制其他数据的类型。Java语言中不支持C++中的指针类型、结构类型、联合类型和枚举类型。

> 引用数据类型就是对一个对象的引用，对象包括实例和数组两种，实际上引用数据类型就是一个指针，只是Java语言里不再使用指针这个说法。

> 所有引用类型的默认值都是null。

> 一个引用变量可以用来引用任何与之兼容的类型。

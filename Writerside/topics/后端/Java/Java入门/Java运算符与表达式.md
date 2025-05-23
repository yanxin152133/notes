# Java运算符与表达式

## 赋值运算符

| 赋值运算符 |               说明               |
|:-----:|:------------------------------:|
|   =   |    简单的赋值运算符，将右操作数的值赋给左侧操作数     |
|  +=   |  加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数  |
|  -=   |  减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数  |
|  *=   |  乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数  |
|  /=   |  除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数  |
|  %=   | 取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数 |
|  <<=  |            左移位赋值运算符            |
|  >>=  |            右移位赋值运算符            |
|  &=   |            按位与赋值操作符            |
|  ^=   |           按位异或赋值操作符            |
| \| =  |            按位或赋值操作符            |

```java
package com.java.chap04;

/**
 * @author Yan
 * @date 2019/7/16 13:01
 */
public class Demo1 {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        int c = 0;
        c = a + b;
        System.out.println("c = a + b = " + c );
        c += a ;
        System.out.println("c += a  = " + c );
        c -= a ;
        System.out.println("c -= a = " + c );
        c *= a ;
        System.out.println("c *= a = " + c );
        a = 10;
        c = 15;
        c /= a ;
        System.out.println("c /= a = " + c );
        a = 10;
        c = 15;
        c %= a ;
        System.out.println("c %= a  = " + c );
        c <<= 2 ;
        System.out.println("c <<= 2 = " + c );
        c >>= 2 ;
        System.out.println("c >>= 2 = " + c );
        c >>= 2 ;
        System.out.println("c >>= 2 = " + c );
        c &= a ;
        System.out.println("c &= a  = " + c );
        c ^= a ;
        System.out.println("c ^= a   = " + c );
        c |= a ;
        System.out.println("c |= a   = " + c );
    }
}
```

## 算数运算符

> 操作符：+（加），-（减），*（乘），/（除），%（取余），自增（++），自减（--）。

```java
package com.java.chap04;

/**
 * @author Yan
 * @date 2019/7/16 13:07
 */
public class Demo2 {
    public static void main(String[] args) {
        int a=10;
        int b=3;

        //+运算符
        System.out.println(a+"+"+b+"="+(a+b));

        //-运算符
        System.out.println(a+"-"+b+"="+(a-b));

        //*运算符
        System.out.println(a+"*"+b+"="+(a*b));

        // /运算符
        System.out.println(a+"/"+b+"="+(a/b));

        //%运算符
        System.out.println(a+"%"+b+"="+(a%b));
    }
}
```

### 自增与自减运算符

> 操作符：++（自增），--（自减）

> 基本含义：让一个变量涨一个数字，或者让一个变量降一个数字。

> 使用格式：写在变量名称之前，或者写在变量名称之后。例如：a++,++a。


使用方式：

1. 单独使用：不和其他任何操作混合，自己独立成为一个步骤
2. 混合使用：和其他操作混合，例如与赋值混合。或者与打印操作混合，等。

使用区别：

1. 在单独使用的时候，前++和后++没有任何区别。
2. 在混合的时候，有区别
	1. 如果是前++，那么变量立刻马上+1，然后拿着变量进行使用。（先加后用）
	2. 如果是后++，那么首先使用变量本来的数值，然后再让变量+1。（先用后加）

### a++和++a的区别

```java
public class Day04 {
    public static void main(String[] args) {
        int a1 = 1;
        //a++表示先做赋值操作，然后自增

        int b1=a1++;
        System.out.println("b1="+b1);
        System.out.println("a1="+a1);


        //++a表示先自增，然后赋值操作
        int a2 = 1;
        int b2 = ++a2;
        System.out.println("b2=" + b2);
        System.out.println("a2=" + a2);

    }
}

```

## 位运算符

| 位运算符 |                    说明                     |
|:----:|:-----------------------------------------:|
|  &   |           如果相对应位都是1，则结果为1，否则为0            |
|  \|  |          如果相对应位都是 0，则结果为 0，否则为 1          |
|  ^   |           如果相对应位值相同，则结果为0，否则为1            |
|  〜   |       按位取反运算符翻转操作数的每一位，即0变成1，1变成0。        |
|  <<  |        按位左移运算符。左操作数按位左移右操作数指定的位数。         |
|  >>  |        按位右移运算符。左操作数按位右移右操作数指定的位数。         |
| >>>  | 按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。 |

```java
public class Test {
  public static void main(String[] args) {
     int a = 60; /* 60 = 0011 1100 */ 
     int b = 13; /* 13 = 0000 1101 */
     int c = 0;
     c = a & b;       /* 12 = 0000 1100 */
     System.out.println("a & b = " + c );
 
     c = a | b;       /* 61 = 0011 1101 */
     System.out.println("a | b = " + c );
 
     c = a ^ b;       /* 49 = 0011 0001 */
     System.out.println("a ^ b = " + c );
 
     c = ~a;          /*-61 = 1100 0011 */
     System.out.println("~a = " + c );
 
     c = a << 2;     /* 240 = 1111 0000 */
     System.out.println("a << 2 = " + c );
 
     c = a >> 2;     /* 15 = 1111 */
     System.out.println("a >> 2  = " + c );
  
     c = a >>> 2;     /* 15 = 0000 1111 */
     System.out.println("a >>> 2 = " + c );
  }
} 
```

## 逻辑运算符

> 与运算和或运算是短路运算。

| 逻辑运算符 |                        说明                        |
|:-----:|:------------------------------------------------:|
|  &&   |           称为逻辑与运算符。当且仅当两个操作数都为真,条件才为真。           |
| \|\|  |          称为逻辑或操作符。如果任何两个操作数任何一个为真，条件为真。          |
|   ！   | 称为逻辑非运算符。用来反转操作数的逻辑状态。如果条件为true，则逻辑非运算符将得到false。 |

```java
package com.java.chap04;

/**
 * @author Yan
 * @date 2019/7/16 13:53
 */
public class Demo4 {
    public static void main(String[] args) {
        // && 与  前后两个操作数必须都是true才返回true，否则返回false
        boolean b1 = (5 < 3) && (4 > 5);
        System.out.println("b1 = " + b1);

        //& 不短路与
        boolean b2 = (5 < 3) & (4 > 5);
        System.out.println("b2 = " + b2);

        //一般都使用 &&
        //原因：效率高

        // || 或 只要两个操作数中有一个是true，就返回true，否则返回false
        boolean b3 = (2 < 3) || (4 > 5);
        System.out.println("b3 = " + b3);

        // | 不短路或
        boolean b4 = (2 < 3) | (4 > 5);
        System.out.println("b4 = " + b4);

        // ! 非，如果操作数为true，返回false，否则返回true
        boolean b5 = !(3 < 4);
        System.out.println("b5 = " + b5);

        // ^ 异或 当两个操作数不相同时，返回true，否则返回false
        boolean b6 = (5 > 4) ^ (4 > 5);
        System.out.println("b6 = " + b6);
    }
}
```

## 关系运算符

| 关系运算符 | 说明                                     |
|:-----:|:---------------------------------------|
|  ==   | 比较符号两边数据是否相等，相等结果是true                 |
|   <   | 比较符号左边的数据是否小于右边的数据，如果小于结果是true         |
|   >   | 比较符号左边的数据是否大于右边的数据，如果大于结果是true         |
|  <=   | 比较符号左边的数据是否小于或者等于右边的数据，如果小于或者等于结果是true |
|  >=   | 比较符号左边的数据是否大于或者等于右边的数据，如果大于或者等于结果是true |
|  !=   | 不等于符号，如果符号两边的数据不相等，结果是true             |

```java
package com.java.chap04;

/**
 * @author Yan
 * @date 2019/7/16 14:22
 */
public class Demo5 {
    public static void main(String[] args) {
        int a = 2;
        int b = 3;


        // > 大于
        System.out.println(a + ">" + b + ":" + (a > b));

        // < 小于
        System.out.println(a + "<" + b + ":" + (a < b));

        // >= 大于等于
        System.out.println(a + ">=" + b + ":" + (a >= b));

        // <= 小于等于
        System.out.println(a + "<=" + b + ":" + (a <= b));

        // == 等于
        System.out.println(a + "==" + b + ":" + (a == b));

        // != 不等于
        System.out.println(a + "!=" + b + ":" + (a != b));
    }
}
```

## 三目运算符

> 格式：（表达式）？表达式为true返回值A：表达式为false返回值B

> 三目运算符` ? x : y`后面的类型必须相同，三目运算符也是“短路运算”，只计算`x`或`y`。

```java
package com.java.chap04;

/**
 * @author Yan
 * @date 2019/7/16 14:26
 */
public class Demo6 {
    public static void main(String[] args) {
        //三目运算符
        String s=2>3?"表达式为真":"表达式为假";
        System.out.println("s = " + s);
    }
}

```

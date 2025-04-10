# Java流程控制

## 概述

在一个程序执行的过程中，各条语句的执行顺序对程序的结果是有直接影响的。也就是说，程序的流程对运行结果有直接的影响。所以，我们必须清楚每条语句的执行流程。而且，很多时候我们要通过控制语句的执行顺序来实现我们要完成的功能。

## 顺序结构

顺序执行，根据编写的顺序，从上到下执行。

## 判断语句

1. if 语句
2. if...else 语句
3. if...else if...else 语句

```java
package com.java.chap05;

/**
 * @author Yan
 * @date 2019/7/16 15:16
 */
public class Demo1 {
    public static void main(String[] args) {
        int a=-1;
        // if语句
        if (a>0){
            System.out.println(a+"是正数");
        }

        //if...else语句
        if (a>0){
            System.out.println(a+"是正数");
        }else {
            System.out.println(a+"不是正数");
        }

        //if...else if...else
        if (a>0){
            System.out.println(a+"是正数");
        }else if (a<0){
            System.out.println(a+"是负数");
        }else{
            System.out.println(a+"是0");
        }
    }
}
```

## 选择语句

### switch

```java
package com.java.chap05;

import java.util.Scanner;

/**
 * @author Yan
 * @date 2019/7/16 15:22
 */
public class Demo2 {
    public static void main(String[] args) {
        System.out.println("请输入一个数字");
        //定义一个系统输入对象
        Scanner scanner=new Scanner(System.in);
        int n=scanner.nextInt();
        //System.out.println(n);
        switch (n){
            case 1:{
                System.out.println("用户输入的是1");
                break;
            }
            case 2:{
                System.out.println("用户输入的是2");
                break;
            }
            default:{
                System.out.println("用户输入的是其他数字");
            }
        }
    }
}
```

#### 注意事项

1. 多个case后面的数值不能重复
2. switch后面的小括号当中只能是下列数据类型
	1. 基本数据类型：byte/short/int/char
	2. 引用数据类型：String字符串、enum枚举
3. switch语句格式可以很灵活，前后顺序可以颠倒，而且break语句可以省略。匹配哪个case就从哪个位置向下执行，直到遇到了break或者整体结束为止。

## 循环结构

### 概述

循环语句可以在满足循环条件的情况下，反复执行某一段代码，这段被重复执行的代码被称为循环体语句，当反复执行这个循环体时，需要在合适的时候把循环判断条件修改为false，从而结束循环，否则循环将一直执行下去，形成死循环。

### 基本组成

1. 初始化语句：在循环开始最初执行，而且只做唯一一次
2. 条件判断：如果成立，则循环继续 ；如果不成立，则循环退出
3. 循环体：重复要做的事情内容，若干行语句
4. 步进语句：每次循环之后都要进行的扫尾工作，每次循环结束之后都要执行一次

### 相关语句

2. while 循环
3. do...while 循环
4. for 循环
5. for 循环的嵌套

```java
package com.java.chap05;

/**
 * @author Yan
 * @date 2019/7/16 15:31
 */
public class Demo3 {
    public static void main(String[] args) {
        //在控制台输出1到10
        //while 循环语句
        int i = 1;
        while (i < 11) {
            System.out.print(i + "  ");
            i++;
        }

        System.out.println("\n-------------------");


        // do...while 循环语句
        int j = 1;
        do {
            System.out.print(j + "    ");
            j++;
        } while (j < 11);

        System.out.println("\n-------------------");


        //while和do...while的区别
        //while是先判断后执行，do...while是先执行后判断


        // for 循环
        for (int k = 1; k < 11; k++) {
            System.out.printf(k + "   ");
        }

        System.out.println("\n-------------------");


        // for循环的嵌套
        for (int m = 0; m < 10; m++) {
            for (int n = 0; n < 10; n++) {
                System.out.print("m=" + m + "n=" + n+"   ");
            }
            System.out.println();
        }
    }
}
```

### 区别

1. 如果条件从来没有满足过，那么for循环和while循环将会执行0次，但是do-while循环会执行一次
2. for循环的变量在小括号当中定义，只有循环内部才可以使用，while循环和do-while循环初始化语句本来就在外面，所以出来循环之后还可以继续使用

### 循环结构的结束

#### break

break关键字的用法常见的有两种：

1. 可以在switch语句当中，一旦执行，整个switch语句立刻结束
2. 还可以用在循环语句当中，一旦执行，整个循环语句立刻结束，打断循环

```java
package com.java.chap05;

/**
 * @author Yan
 * @date 2019/7/16 15:52
 */
public class Demo5 {
    public static void main(String[] args) {
        for (int i=0;i<10;i++){
            for (int j=0;j<10;j++){
                if (i==1){
                    break;
                }
                System.out.print("i="+i+"   j="+j+"  ");

            }
            System.out.println();
        }
    }
}
```

#### continue

continue语句：一旦执行，立刻跳过当前此次循环剩余内容，马上开始下一次循环

```java
package com.java.chap05;

/**
 * @author Yan
 * @date 2019/7/16 15:57
 */
public class Demo7 {
    public static void main(String[] args) {
        for (int i=0;i<10;i++){
            if (i==4){
                continue;
            }
            System.out.print("i="+i+"  ");
        }
    }
}
```

#### return

```java
package com.java.chap05;

/**
 * @author Yan
 * @date 2019/7/16 15:59
 */
public class Demo8 {
    public static void main(String[] args) {
        for (int i=0;i<10;i++){
            for (int j=0;j<10;j++){
                if (i==1){
                    return;
                }
                System.out.print("i="+i+"   j="+j+"  ");

            }
            System.out.println();
        }
        System.out.println("执行到这里了");
    }
}
```


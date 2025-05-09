# Java常量

## 定义

常量：是指在Java程序运行期间固定不变的数据。

## 分类

|  类型   |          含义          |    数据举例     |
|:-----:|:--------------------:|:-----------:|
| 整数常量  |         所有整数         |   0，1，100   |
| 浮点数常量 |         所有小数         |  0.0，-0.1   |
| 字符常量  | 单引号引起来，只能写一个字符，必须有内容 |   'a','好'   |
| 字符串常量 | 双引号引起来，可以写多个字符，也可以不写 | "A","Hello" |
| 布尔常量  |        只有两个值         | true,false  |
|  空常量  |        只有一个值         |    null     |

## 常量的打印输出

```java
public class Day01 {
    public static void main(String[] args) {
        // 字符串常量
        System.out.println("ABC");
        System.out.println("");
        System.out.println("XYZ");

        // 整数常量
        System.out.println(30);
        System.out.println(-500);

        // 浮点数常量（小数）
        System.out.println(3.14);
        System.out.println(-2.5);

        // 字符常量
        System.out.println('A');
        System.out.println('6');
        // System.out.println('');
        // System.out.println('AB');

        // 布尔常量
        System.out.println(true);
        System.out.println(false);

        // 空常量（不能直接打印输出）
        //System.out.println(null);
    }
}

```
# Java数据类型转换

> Java程序中要求参与的计算的数据，必须要保持数据类型的一致性，如果数据类型不一致将发生类型的转换。

## 自动转换

> 自动类型转换（隐式）将'取值范围小的类型'自动提升为'取值范围大的类型'。

- 特点：代码不需要进行特殊处理，自动完成
- 规则：数据范围从小到大

```java
public class Day03 {
    public static void main(String[] args) {
        // 自动转换
        System.out.println(1024); // int
        System.out.println(3.14); // double

        // 左边是long类型，右边是默认的int类型，左右不一样
        // 一个等号代表赋值，将右侧的int常量，交给左侧的long变量进行存储
        // int -- >long ，符合了数据范围从小到大的要求
        // 发生自动数据类型转换
        long num1 = 100L;
        System.out.println(num1);

        double num2 = 2.5F;
        System.out.println(num2);

        float num3 = 30L;
        System.out.println(num3);
    }
}
```

## 强制转换

> 强制类型转换（显式）

- 特点：代码需要进行特殊的格式处理，不能自动完成。
- 格式：范围小的类型 范围小的变量名= (范围小的类型) 原本范围大的数据

```java
public class Day03 {
    public static void main(String[] args) {
        
        // 强制类型转换
        int num4 = (int) 100L;
        System.out.println(num4);

        byte num5 = 40;
        byte num6 = 60;
        int result = num5 + num6;
        System.out.println(result);
    }
}
```

## 注意事项

1. 强制类型转换一般不推荐使用，因为有可能发生精度损失、数据溢出。
2. byte/short/char这三种类型都可以发生数学运算。
3. byte/short/char这三种类型在运算的时候，都会被首先提升成为int类型，然后再计算。
4. boolean类型不能发生数据类型转换。
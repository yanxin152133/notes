# Java面向对象

## 面向对象的基本概念

定义：以基于对象的思维去分析和解决问题，万物皆对象；     
三大特性：封装，继承，多态；

## 类与对象

### 类与对象的关系

### 类的定义

### 类的创建及使用

```java
package com.java.chap07.sec01;

/**
 * @author Yan
 * @date 2019/7/18 13:39
 * Person类
 */
public class Person {
    String name;    //在类中，定义一个姓名name字符串属性
    int age;         //在类中，定义一个年龄age属性


    public void speak(){
        System.out.println("我叫"+name+"我今年"+age+"岁了");
    }

    public static void main(String[] args) {
        //定义一个Person类的对象zhangsan
        Person zhangsan;
        //实例化对象
        zhangsan=new Person();
        //给对象的name属性赋值
        zhangsan.name="张三";
        zhangsan.age=23;
        zhangsan.speak();
    }
}


```

内存分析

![](https://live.staticflickr.com/65535/48312793817_34c3556633_z.jpg)

## 方法

### 方法的定义及简单使用

```java
package com.java.chap07.sec02;

/**
 * @author Yan
 * @date 2019/7/18 13:54
 */
public class People {
    /**
     * 最简单的一个方法定义
     */
    void speak(){
        System.out.println("我叫张三");
    }

    public static void main(String[] args) {
        People zhangsan=new People();
        zhangsan.speak();
    }
}

```

```java
package com.java.chap07.sec02;

/**
 * @author Yan
 * @date 2019/7/18 13:57
 */
public class People2 {

    //形参，入参
    void speak(String name){
        System.out.println("我叫"+name);
    }

    public static void main(String[] args) {
        People2 zhangsan=new People2();
        zhangsan.speak("张三");
    }
}

```

```java
package com.java.chap07.sec02;

/**
 * @author Yan
 * @date 2019/7/18 13:57
 */
public class People3 {

    //形参，入参
    void speak(String name, int age) {
        System.out.println("我叫" + name+"我今年"+age+"岁了");
    }

    public static void main(String[] args) {
        People3 zhangsan = new People3();
        zhangsan.speak("张三", 23);
    }
}

```

```java
package com.java.chap07.sec02;

/**
 * @author Yan
 * @date 2019/7/18 13:57
 */
public class People4 {

    //形参，入参,不固定参数
    void speak(String name, int age,String ...hobbies) {
        System.out.println("我叫" + name + "我今年" + age + "岁了");
        System.out.println("我的爱好：  ");
        for (String hobby:hobbies){
            System.out.print(hobby+"  ");
        }
    }

    public static void main(String[] args) {
        People4 zhangsan = new People4();
        zhangsan.speak("张三", 23,"游泳","唱歌");
    }
}

```

```java
package com.java.chap07.sec02;

/**
 * @author Yan
 * @date 2019/7/18 13:57
 */
public class People5 {

    //返回类型
    int speak(String name, int age,String ...hobbies) {
        System.out.println("我叫" + name + "我今年" + age + "岁了");
        System.out.println("我的爱好：  ");
        for (String hobby:hobbies){
            System.out.print(hobby+"  ");
        }

        //获取爱好的长度
        int totalHobbies=hobbies.length;
        return totalHobbies;
    }

    public static void main(String[] args) {
        People5 zhangsan = new People5();
        int n=zhangsan.speak("张三", 23,"游泳","唱歌");
        System.out.println("\n有"+n+"个爱好");
    }
}

```

### 方法的值传递和引用传递(重点)

```java
package com.java.chap07.sec02;

/**
 * 三围类
 * @author Yan
 * @date 2019/7/18 14:08
 */
class Sanwei{
    int b;   //胸围
    int w;   //腰围
    int h;   //臀围
}


public class People6 {


    /**
     * 报三围
     * @param age 年龄
     * @param sanwei  三围
     */
    void speak(int age,Sanwei sanwei){
        System.out.println("我今年"+age+"岁了,我的三围是"+sanwei.b+","+sanwei.w+","+sanwei.h);
        age=24;
        sanwei.b=80;
    }

    public static void main(String[] args) {
        People6 xiaoli=new People6();
        int age=23;
        Sanwei sanwei=new Sanwei();
        sanwei.b=90;
        sanwei.w=60;
        sanwei.h=90;
        //age传递的是值，sanwei传递的是引用（地址）,c里叫指针
        xiaoli.speak(age,sanwei);
        System.out.println(age);
        System.out.println(sanwei.b);
    }
}

```

### 方法的重载

方法重载定义：方法名称相同，但是参数的类型或者参数的个数不同。

```java
package com.java.chap07.sec03;

/**
 * @author Yan
 * @date 2019/7/18 14:22
 */
public class Demo {
    int add(int a,int b){
        System.out.print("方法一:");
        return a+b;
    }

    /**
     * 方法的重载，参数个数不一样
     * @param a
     * @param b
     * @param c
     * @return
     */
    int add(int a,int b,int c){
        System.out.print("方法二：");
        return a+b+c;
    }


    /**
     * 方法的重载，参数的类型不一样
     * @param a
     * @param b
     * @return
     */
    int add(int a,String b){
        System.out.print("方法三：");
        return a+Integer.parseInt(b);
    }
    

    public static void main(String[] args) {
        Demo demo=new Demo();
        System.out.println(demo.add(1,2));
        System.out.println(demo.add(1,2,3));
        System.out.println(demo.add(1,"3"));
    }
}

```

### static静态方法与普通方法

static方法：方法属于类本身；调用方式：1. 类名.方法;2. 对象.方法

普通方法：方法属于类的对象；调用方式：1. 对象.方法

```java
package com.java.chap07.sec03;

/**
 * @author Yan
 * @date 2019/7/18 15:33
 */
public class Demo2 {

    void fun1(){
        System.out.println("这是一个普通方法");
    }

    static void fun2(){
        System.out.println("这是一个静态方法");
    }

    public static void main(String[] args) {
        Demo2 demo2=new Demo2();
        //调用普通方法，对象.方法
        demo2.fun1();
        //调用静态方法，类名.方法名
        Demo2.fun2();
        //调用静态方法，对象.方法
        demo2.fun2();
    }
}
```

### 递归方法

求阶乘 1 * 2 * 3... * (n-1) * n    
原理：

```java
N=5 F(n-1) * 5
N=4 F(n-1) * 4
N=3 F(n-1) * 3
N=2 F(n-1) * 2
N=1 1   
```

```java
package com.java.chap07.sec03;

/**
 * @author Yan
 * @date 2019/7/18 15:38
 */
public class Demo3 {


    /**
     * 非递归
     * @param n
     * @return
     */
    static long notDiGui(int n){
        long result=1;
        for (int i=1;i<=n;i++){
            result=result*i;
        }
        return result;
    }

    /**
     * 递归
     * @param n
     * @return
     */
    static long DiGUi(int n){
        if (n==1){
            return 1;
        }
        return DiGUi(n-1)*n;
    }



    public static void main(String[] args) {
        System.out.println("非递归："+Demo3.notDiGui(5));
        System.out.println("递归:"+Demo3.DiGUi(5));
    }
}
```

## 构造方法，this关键字

### 构造方法

构造器是一个特殊的方法，这个特殊方法用于创建实例时可执行初始化；    
假如没有构造方法，系统会自动生成一个默认的无参构造方法；假如有构造方法，系统不会自动生成构造方法；

```java
package com.java.chap07.sec04;

/**
 * @author Yan
 * @date 2019/7/18 15:56
 */
public class People {

    // String 类属性默认值是null
    private String name;
    //int 类属性默认值是0
    private int age;


    /**
     * 默认构造方法
     */
    People(){
        System.out.println("默认构造方法");
    }

    /**
     * 有参数的构造方法   构造方法的重载
     */
    People(String name2,int age2){
        name=name2;
        age=age2;
        System.out.println("有参数的构造方法");
    }

    public void say(){
        System.out.println("我叫："+name+"，我今年："+age+"岁了");
    }

    public static void main(String[] args) {
        //People people=new People();
        People people2=new People("张三",23);
        people2.say();
    }
}

```

### this关键字

this表示当前对象

1. 使用this调用本类中的属性；
2. 使用this调用构造方法；

```java
package com.java.chap07.sec04;

/**
 * @author Yan
 * @date 2019/7/18 15:56
 */
public class People2 {

    // String 类属性默认值是null
    private String name;
    //int 类属性默认值是0
    private int age;


    /**
     * 默认构造方法
     */
    People2(){
        System.out.println("默认构造方法");
    }

    /**
     * 有参数的构造方法   构造方法的重载
     */
    People2(String name2, int age2){
        this();
        this.name=name2;
        this.age=age2;
        System.out.println("有参数的构造方法");
    }

    public void say(){
        System.out.println("我叫："+name+"，我今年："+age+"岁了");
    }

    public static void main(String[] args) {
        //People people=new People();
        People2 people2=new People2("张三",23);
        people2.say();
    }
}
```

## 访问控制权限及package import关键字

### 访问控制权限

private(私有)   get,set方法     
package（包访问权限）     
protected(子类访问权限)     
public（公共访问权限）

|       | private | package | protected | public |
|-------|---------|---------|-----------|--------|
| 同一个类中 | √       | √       | √         | √      |
| 同一个包中 |         | √       | √         | √      |
| 子类中   |         |         | √         | √      |
| 全局范围  |         |         |           | √      |

**Demo1.java**

```java
package com.java.chap07.sec05;

/**
 * @author Yan
 * @date 2019/7/18 16:16
 */
public class Demo1 {
    /**
     * 定义一个私有的属性a
     */
    private int a;


    public int getA() {
        return a;
    }

    public void setA(int a) {
        this.a = a;
    }
}

```

**TestDemo1.java**

```java
package com.java.chap07.sec05;

/**
 * @author Yan
 * @date 2019/7/18 16:17
 */
public class TestDemo1 {
    public static void main(String[] args) {
        Demo1 demo1=new Demo1();
        demo1.setA(2);
        int a=demo1.getA();
        System.out.println(a);
    }
}

```

### package import 关键字

package 包定义      
import 导入相关类

```java
package com.java.chap07.sec05;


import com.java.chap07.sec02.People;

/**
 * @author Yan
 * @date 2019/7/18 16:19
 */
public class Demo2 {
    public static void main(String[] args) {

        //不同包，则需要导入相关类
        People people=new People();
        //在同一个包中，则不需要导入相关类
        Demo1 demo1=new Demo1();
    }
}

```

## 内部类

内部类定义：在类的内部定义类；     
内部类优点：可以方便的使用外部类的属性；     
内部类缺点：破环类的基本结构；

**Demo1.java**

```java
package com.java.chap07.sec06;

/**
 * @author Yan
 * @date 2019/7/18 16:38
 */
public class Outer {
    private int a=1;

    /**
     * 定义内部类
     */
    class Inner{
        public void show(){
            System.out.println(a);
        }
    }


    public void show(){
        Inner inner=new Inner();
        inner.show();
    }

    public static void main(String[] args) {
        Outer outer=new Outer();
        outer.show();
    }
}
```

**Demo2.java**

```java
package com.java.chap07.sec06;

/**
 * @author Yan
 * @date 2019/7/18 16:38
 */
public class Outer2 {
    private int a = 1;

    /**
     * 定义内部类
     */
    class Inner {
        public void show() {
            System.out.println(a);
        }
    }


    public static void main(String[] args) {
        Outer2 outer2 = new Outer2();   //实例化外部类对象
        Outer2.Inner inner = outer2.new Inner();    //实例化内部类对象
        inner.show();
    }
}

```

## 代码块

1. 普通代码块

```java
package com.java.chap07.sec07;

/**
 * @author Yan
 * @date 2019/7/18 16:49
 */
public class Demo1 {
    public static void main(String[] args) {
        int a=1;
        /**
         * 普通代码块
         */
        {
            a=2;
            System.out.println("普通代码块");
        }
        System.out.println("a="+a);
    }
}

```

1. 构造块

```java
package com.java.chap07.sec07;

/**
 * @author Yan
 * @date 2019/7/18 16:52
 */
public class Demo2 {
    /**
     * 构造块
     * @param args
     */
    {
        System.out.println("通用构造块");
    }

    /**
     * 构造方法一
     */
    public Demo2(){
        System.out.println("构造方法一");
    }
    /**
     * 构造方法二
     */
    public Demo2(int i){
        System.out.println("构造方法二");
    }
    /**
     * 构造方法三
     */
    public Demo2(int i,int j){
        System.out.println("构造方法三");
    }


    public static void main(String[] args) {
        new Demo2();    //实例化一个对象  匿名类
        new Demo2(1);
        new Demo2(1,2);
    }
}

```

1. 静态代码块

```java
package com.java.chap07.sec07;

/**
 * @author Yan
 * @date 2019/7/18 16:55
 */
public class Demo3 {
    /**
     * 构造块
     */
    {
        System.out.println("通用构造块");
    }

    /**
     * 静态代码块
     */
    static {
        System.out.println("静态代码块");
    }

    /**
     * 构造方法一
     */
    public Demo3(){
        System.out.println("构造方法一");
    }
    /**
     * 构造方法二
     */
    public Demo3(int i){
        System.out.println("构造方法二");
    }
    /**
     * 构造方法三
     */
    public Demo3(int i,int j){
        System.out.println("构造方法三");
    }

    public static void main(String[] args) {
        new Demo3();
        new Demo3(1);
        new Demo3(1,2);

    }
}
```

## String 类

### 实例化String对象

方法一：

```java
String name1="张三";
```

方法二：

```java
String name2=new String("李四");
```

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 21:32
 */
public class Demo1 {
    public static void main(String[] args) {
        //实例化String的方式一
        String name1="张三";
        System.out.println("name1:"+name1);

        //实例化String的方式二
        String name2=new String("李四");
        System.out.println("name2:"+name2);
    }
}
```

### "==" VS "equals方法"

1. “==”，比较的是引用，“equals方法”比较的是具体内容

![](https://live.staticflickr.com/65535/48314990292_7cf024cfee_z.jpg)

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 21:39
 */
public class Demo2 {
    public static void main(String[] args) {
        String name1="张三";    //直接赋值方式
        String name2=new String("张三");   //new 的方式
        String name3=name2;     // 传递引用

        //==比较的是引用
        System.out.println("name1==name2:"+(name1==name2));
        System.out.println("name1==name3:"+(name1==name3));
        System.out.println("name2==name3:"+(name2==name3));

        System.out.println("-------------");


        //equals比较的是内容
        System.out.println("name1.equals(name2):"+(name1.equals(name2)));
        System.out.println("name1.equals(name3):"+(name1.equals(name3)));
        System.out.println("name2.equals(name3):"+(name2.equals(name3)));
    }
}
```

### String 两种实例化方式的区别

1. 直接赋值方式，创建的对象存放到字符串对象池里，假如存在的，就不会创建；
2. new对象方式，每次都创建一个新的对象；

![](https://live.staticflickr.com/65535/48315050387_192d66d693_z.jpg)

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 21:46
 */
public class Demo3 {
    public static void main(String[] args) {
        String name1="张三";
        String name2="张三";
        String name3=new String("张三");
        String name4=new String("张三");

        System.out.println("name1==name2:"+(name1==name2));
        System.out.println("name1==name3:"+(name1==name3));
        System.out.println("name3==name4:"+(name3==name4));
    }
}
```

### 字符串的内容不可变性

字符串的特性：不能改变字符串的内容；只能通过指向一个新的内存地址；

![](https://live.staticflickr.com/65535/48314968091_7ac4026543_z.jpg)

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 21:51
 */
public class Demo4 {
    public static void main(String[] args) {
        String name="张";
        name+="三";
        System.out.println(name);
    }
}

```

### String类常用方法及基本使用

1. char charAt(int index)返回指定索引处的char值。

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 22:00
 */
public class Demo5 {
    public static void main(String[] args) {
        String name="张三";
        char ming=name.charAt(1);
        System.out.println(ming);

        String srt="我是中国人";

        //遍历字符串
        for (int i=0;i<srt.length();i++){
            System.out.println(srt.charAt(i));
        }
    }
}

```

1. int length()返回此字符串的长度。
2. int indexOf() 返回指定字符在此字符中第一次出现处的索引。

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 22:03
 */
public class Demo6 {
    public static void main(String[] args) {
        //indexOf方法使用示例
        String str="abcdefghdijklmnopqrstuvwxyz";
        System.out.println("d在字符串str中第一次出现的索引位置："+str.indexOf("d"));
        System.out.println("d在字符串str中第一次出现的索引位置,从索引4开始："+str.indexOf("d",4));
    }
}

```

1. String substring(int beginIndex)返回一个新的字符串，它是此字符串的一个子字符串。该子字符串从指定索引处的字符开始，直到此字符串末尾。

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 22:08
 */
public class Demo7 {
    public static void main(String[] args) {
        //subString方法使用
        String str="不开心每一天,不可能";
        String newStr=str.substring(1);
        String newStr2=str.substring(1,6);
        System.out.println(str);
        System.out.println(newStr);
        System.out.println(newStr2);
    }
}

```

1. String toUpperCase() 使用默认语言环境的规则将此String中的所有字符都转换为大写。

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 22:12
 */
public class Demo8 {
    public static void main(String[] args) {
        String str="I'm a boy!";
        String upStr=str.toUpperCase();   //转换成大写
        System.out.println("str:"+str);
        System.out.println("upStr:"+upStr);


        String lowerStr=upStr.toLowerCase();  //转换成小写
        System.out.println("lowerStr:"+lowerStr);
    }
}

```

1. 综合实例   
   编程输入一个字符串，要求去掉前后的空格，然后分别统计其中英文字母，空格，数字和其他字符的个数。

```java
package com.java.chap07.sec08;

/**
 * @author Yan
 * @date 2019/7/18 22:18
 */
public class Demo9 {
    public static void main(String[] args) {
        String str="  aB23 2&*  &*   s2  ";
        //去掉前面和后面的空白
        String newStr=str.trim();
        System.out.println("str:"+str);
        System.out.println("newStr:"+newStr);


        int yingWen=0;
        int kongGe=0;
        int shuZi=0;
        int qiTa=0;

        for (int i=0;i<newStr.length();i++){
            char c=newStr.charAt(i);
            if (c>='a'&&c<='z'||(c>='A'&&c<='Z')){
                //判断英文字符
                yingWen++;
                System.out.println("英文字符："+c);
            }else if (c>='0'&&c<='9'){
                //判断数字
                shuZi++;
                System.out.println("数字："+c);
            }else if (c==' '){
                //判断空格
                kongGe++;
                System.out.println("空格："+c);
            }else {
                //判断其他
                qiTa++;
                System.out.println("其他："+c);
            }
        }
        System.out.println();
        System.out.println("英文总数："+yingWen);
        System.out.println("数字总数："+shuZi);
        System.out.println("空格总数："+kongGe);
        System.out.println("其他总数："+qiTa);
    }
}
```

## Java类的继承

1. 继承定义以及基本使用
   定义：子类能够继承父类的属性和方法；      
   **注意点**：Java中只支持单继承；私有方法不能继承。

**Animal.java**

```java
package com.java.chap07.sec09;

/**
 * 动物类
 *
 * @author Yan
 * @date 2019/7/19 15:32
 */
public class Animal {
    //属性姓名
    private String name;
    //属性年龄
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }


    //方法say
    public void say() {
        System.out.println("我是一个动物，我叫：" + this.getName() + "我的年龄是：" + this.getAge());
    }
}

```

**Dog.java**

```java
package com.java.chap07.sec09;

/**
 *
 * 定义Dog类，继承自Animal类
 * @author Yan
 * @date 2019/7/19 17:25
 */
public class Dog extends Animal{


    public static void main(String[] args) {
        Dog dog=new Dog();
        dog.setName("Pick");
        dog.setAge(1);
        dog.say();
    }
}

```

1. 方法重写

**Animal.java**

```java
package com.java.chap07.sec09;

/**
 * 动物类
 *
 * @author Yan
 * @date 2019/7/19 15:32
 */
public class Animal {
    //属性姓名
    private String name;
    //属性年龄
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }


    //方法say
    public void say() {
        System.out.println("我是一个动物，我叫：" + this.getName() + "我的年龄是：" + this.getAge());
    }
}

```

**Cat.java**

```
package com.java.chap07.sec09;

/**
 * @author Yan
 * @date 2019/7/19 17:32
 */
public class Cat extends Animal {

    /**
     * 重写父类的say方法
     */
    //方法say
    public void say() {
        System.out.println("我是一只猫，我叫：" + this.getName() + "我的年龄是：" + this.getAge());
    }

    public static void main(String[] args) {
        Cat cat=new Cat();
        cat.setName("Mini");
        cat.setAge(2);
        cat.say();
    }
}

```

1. 对象实例化过程以及super关键字

**Animal.java**

```java
package com.java.chap07.sec09;

/**
 * 动物类
 *
 * @author Yan
 * @date 2019/7/19 15:32
 */
public class Animal {
    //属性姓名
    private String name;
    //属性年龄
    private int age;


    /**
     * 无参构造方法
     */
    public Animal(){
        System.out.println("无参构造方法");
    }

    /**
     * 有参父类构造方法
     * @param name 名字
     * @param age 年龄
     */
    public Animal(String name,int age){
        System.out.println("有参父类构造方法");
        this.name=name;
        this.age=age;
    }


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }


    //方法say
    public void say() {
        System.out.println("我是一个动物，我叫：" + this.getName() + "我的年龄是：" + this.getAge());
    }
}

```

**Cat.java**

```java
package com.java.chap07.sec09;

/**
 * @author Yan
 * @date 2019/7/19 17:32
 */
public class Cat extends Animal {


    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Cat() {
        super();
        System.out.println("子类无参构造方法");
    }

    public Cat(String name, int age,String address) {
        super(name, age);
        this.address=address;
        System.out.println("子类有参构造方法");
    }

    /**
     * 重写父类的say方法
     */
    //方法say
    public void say() {
        //调用父类的say方法
        super.say();
        System.out.println("我是一只猫，我叫：" + this.getName() + "我的年龄是：" + this.getAge()+"我来自："+this.address);
    }

    public static void main(String[] args) {
        Cat cat=new Cat("Mini",1,"火星");
        //cat.setName("Mini");
        //cat.setAge(2);
        cat.say();
    }
}

```

## final关键字

> 使用final声明的类不能被继承
> 使用final声明的方法不能被子类覆盖
> 使用final声明的变量不能被修改，即为常量

1. final 修饰类

代码示例:
**JiangShi.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 21:04
 */
public final class JiangShi {

}

```

**test.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 21:05
 */
public class Test extends JiangShi{

}

```

1. final 修饰方法

**People.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 21:55
 */
public class People {



    public final void action(){
        System.out.println("做一个良好公民！");
    }



}

```

**Test.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 21:05
 */
public class Test extends People{


    public void action(){
        System.out.println("做一个坏蛋");
    }
}

```

1. final 修饰的变量

**People.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 21:55
 */
public class People {

    private final int a=1;

    public void action(){
        a=2;
        System.out.println("做一个良好公民！");
    }
}

```

**Common.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 22:25
 */
public class Common {
    /**
     * 静态常量
     */
    public static final String SOMETILE="中国的首都是北京";
}

```

**Test.java**

```java
package com.java.chap07.sec10;

/**
 * @author Yan
 * @date 2019/7/19 21:05
 */
public class Test extends People{


    public void action(){
        System.out.println("做一个坏蛋");
    }

    public static void main(String[] args) {
        System.out.println(Common.SOMETILE);
    }
}
```

## 抽象类

定义：在Java中，含有抽象方法的类称为抽象类，同样不能生成对象。     
**注意点**：

1. 包含一个抽象方法的类是抽象类
2. 抽象类和抽象方法都要用abstract 关键字声明
3. 抽象方法只需要声明而不需要实现
4. 抽象类必须被子类（假如不是抽象类）必须重写抽象类中的全部抽象方法
5. 抽象类不能被实例化

**People.java**

```java
package com.java.chap07.sec11;

/**
 * 定义一个抽象类People
 * @author Yan
 * @date 2019/7/20 15:25
 */
public abstract class People {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void say(){
        System.out.println("我的姓名是："+this.getName());
    }


    /**
     *
     * 定义一个抽象方法职业
     */
    public abstract void profession();

}

```

**Student.java**

```java
package com.java.chap07.sec11;

/**
 * @author Yan
 * @date 2019/7/20 15:29
 */
public class Student extends People{
    @Override
    public void profession() {
        System.out.println("职业是：学生");
    }
}

```

**Teacher.java**

```java
package com.java.chap07.sec11;

/**
 * @author Yan
 * @date 2019/7/20 15:30
 */
public class Teacher extends People{
    @Override
    public void profession() {
        System.out.println("职业是：老师");
    }
}

```

**Test.java**

```java
package com.java.chap07.sec11;

/**
 * @author Yan
 * @date 2019/7/20 15:28
 */
public class Test {
    public static void main(String[] args) {
        //抽象类不能被实例化
        //People p=new People();
        Student student=new Student();
        student.profession();


        Teacher teacher=new Teacher();
        teacher.profession();
    }
}

```

## 接口

定义：一种特殊的“抽象类”，没有普通方法，有全局常量和公共的抽象方法所组成。

1. 接口的定义

**A.java**

```java
package com.java.chap07.sec12;

/**
 * 定义一个接口A
 * @author Yan
 * @date 2019/7/20 15:40
 */
public interface A {


    /**
     * 全局常量
     */
    public static final String TITLE="www.baidu.com";


    /**
     * 定义一个抽象方法 abstract可以省略
     */
    public void a();
}

```

2. 实现接口
   可以实现一个或者多个接口

实现一个接口
**A.java**

```java
package com.java.chap07.sec12;

/**
 * 定义一个接口A
 * @author Yan
 * @date 2019/7/20 15:40
 */
public interface A {


    /**
     * 全局常量
     */
    public static final String TITLE="www.baidu.com";


    /**
     * 定义一个抽象方法 abstract可以省略
     */
    public void a();
}

```

**Test.java**

```java
package com.java.chap07.sec12;

/**
 * @author Yan
 * @date 2019/7/20 16:04
 */
public class Test implements A{
    @Override
    public void a() {
        System.out.println("a方法");
    }

    public static void main(String[] args) {
        Test test=new Test();
        test.a();
        System.out.println(Test.TITLE);
    }
}

```

实现多个接口

**A.java**

```java
package com.java.chap07.sec12;

/**
 * 定义一个接口A
 * @author Yan
 * @date 2019/7/20 15:40
 */
public interface A {

    /**
     * 全局常量
     */
    public static final String TITLEA="www.baidu.com";


    /**
     * 定义一个抽象方法 abstract可以省略
     */
    public void a();
}

```

**B.java**

```java
package com.java.chap07.sec12;

/**
 * 定义一个接口A
 * @author Yan
 * @date 2019/7/20 15:40
 */
public interface B {


    /**
     * 全局常量
     */
    public static final String TITLEB="www.google.com";


    /**
     * 定义一个抽象方法 abstract可以省略
     */
    public void b();
}

```

**Test.java**

```java
package com.java.chap07.sec12;

/**
 * @author Yan
 * @date 2019/7/20 16:04
 */
public class Test implements A, B {
    @Override
    public void a() {
        System.out.println("a方法");
    }

    @Override
    public void b() {
        System.out.println("b方法");
    }

    public static void main(String[] args) {
        Test test = new Test();
        test.a();
        test.b();
        System.out.println(Test.TITLEA);
        System.out.println(Test.TITLEB);
    }


}

```

1. 继承类和实现接口
   先继承，后实现接口

**C.java**

```java
package com.java.chap07.sec12;

/**
 * @author Yan
 * @date 2019/7/20 16:12
 */
public class C {
    public void c(){
        System.out.println("c方法");
    }
}

```

**Test.java**

```java
package com.java.chap07.sec12;

/**
 * @author Yan
 * @date 2019/7/20 16:04
 */
public class Test extends C implements A, B {
    @Override
    public void a() {
        System.out.println("a方法");
    }

    @Override
    public void b() {
        System.out.println("b方法");
    }

    public static void main(String[] args) {
        Test test = new Test();
        test.a();
        test.b();
        test.c();
        System.out.println(Test.TITLEA);
        System.out.println(Test.TITLEB);
    }


}
```

1. 接口的继承
   接口可以多继承

**D.java**

```java
package com.java.chap07.sec12;

/**
 * 定义接口D,继承A，B接口
 * @author Yan
 * @date 2019/7/20 16:15
 */
public interface D extends A,B{
    /**
     * 定义一个抽象方法 abstract可以省略
     */
    public void d();
}

```

**Test2.java**

```java
package com.java.chap07.sec12;

/**
 * @author Yan
 * @date 2019/7/20 16:04
 */
public class Test2 extends C implements D {
    @Override
    public void a() {
        System.out.println("a方法");
    }

    @Override
    public void b() {
        System.out.println("b方法");
    }

    @Override
    public void d() {
        System.out.println("d方法");
    }

    public static void main(String[] args) {
        Test2 test = new Test2();
        test.a();
        test.b();
        test.c();
        test.d();
        System.out.println(Test2.TITLEA);
        System.out.println(Test2.TITLEB);
    }


}
```

## 对象多态性

Java中多态性体现：

1. 方法的重载和重写；
2. 可以用父类的引用指向子类的具体实现，而且可以随时更换为其他子类的具体实现；

**Animal.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:23
 */
public class Animal {

    public void say(){
        System.out.println("我是一个动物");
    }
}
```

**Cat.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:24
 */
public class Cat extends Animal{
    public void say(){
        System.out.println("我是一个猫");
    }

}

```

**Dog.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:24
 */
public class Dog extends Animal{
    public void say(){
        System.out.println("我是一个狗");
    }

}

```

**Test.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:25
 */
public class Test {
    public static void main(String[] args) {
        /*
        Dog dog=new Dog();
        dog.say();

        Cat cat=new Cat();
        cat.say();
        */

        //父类引用指向Dog类的具体实现
        Animal animal = new Dog();
        animal.say();

        animal=new Cat();
        animal.say();
    }
}

```

对象的转型：      
向上转型：子类对象->父类对象  **安全**
向下转型：父类对象->子类对象  **不安全**

向下转型

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:25
 */
public class Test {
    public static void main(String[] args) {
        /*
        Dog dog=new Dog();
        dog.say();

        Cat cat=new Cat();
        cat.say();
        */

        //父类引用指向Dog类的具体实现
        Animal animal = new Dog();
        animal.say();


        //向下转型
        Dog dog= (Dog) animal;
        dog.say();


        //向下转型2 不安全
        Cat cat= (Cat) animal;
        cat.say();


        /*animal=new Cat();
        animal.say();
         */
    }
}

```

**接口方式**

**Animal.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:23
 */
public interface Animal2 {


    public void say2();
}

```

**Dog2.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:34
 */
public class Dog2 implements Animal2{
    @Override
    public void say2() {
        System.out.println("我是一只狗");
    }
}

```

**Cat2.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:24
 */
public class Cat2 implements Animal2 {
    public void say2() {
        System.out.println("我是一个猫");
    }

}

```

**Test2.java**

```java
package com.java.chap07.sec13;

/**
 * @author Yan
 * @date 2019/7/20 16:25
 */
public class Test2 {
    public static void main(String[] args) {


        //父类引用指向Dog类的具体实现  向上转型
        Animal2 animal2 = new Dog2();
        animal2.say2();


        animal2=new Cat2();
        animal2.say2();


        //向下转型
        Dog2 dog2= (Dog2) animal2;
        dog2.say2();


        //向下转型2 不安全
        Cat2 cat2= (Cat2) animal2;
        cat2.say2();

    }
}

```

## Object类

> Object类是所有类的父类；

```java
package com.java.chap07.sec14;

/**
 * @author Yan
 * @date 2019/7/20 21:02
 */
public class A extends Object{

    /**
     * Object 类是所有类的父类
     */
    public A(){
        super();
    }
}

```

### Object类的常用方法

1.

```java
public String toString();    //返回该对象的字符串表示
```

代码示例：

```java
package com.java.chap07.sec14;

/**
 * @author Yan
 * @date 2019/7/20 21:04
 */
public class People {

    private String name;

    public People(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }


    @Override
    public String toString() {
        return this.getName();
    }

    public static void main(String[] args) {
        People people=new People("张三");
        System.out.println(people);
        System.out.println(people.toString());
    }
}

```

1.

```java
public boolean equals(Object obj);  //指示其他某个对象是否与此对象“相等”
```

代码示例：

```java
package com.java.chap07.sec14;

/**
 * @author Yan
 * @date 2019/7/20 21:04
 */
public class People {

    private String name;

    public People(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }


    @Override
    public String toString() {
        return this.getName();
    }

    @Override
    public boolean equals(Object obj) {
        String name=((People)obj).getName();
        System.out.println(name);
        return this.name==name;
    }

    public static void main(String[] args) {
        People people=new People("张三");
        People people1=new People("张三");
        People people3=new People("李四");
        System.out.println("people.equals(people1)"+people.equals(people1));
        System.out.println("people1.equals(people3)"+people1.equals(people3));
        System.out.println(people);
        System.out.println(people.toString());
    }
}

```

## instanceof关键字

作用：判断一个对象是否属于一个类

格式： 对象 instanceof类 返回布尔类型

向下转型作判断；

代码示例：

**Animal.java**

```java
package com.java.chap07.sec15;

/**
 * @author Yan
 * @date 2019/7/20 21:17
 */
public class Animal {
    public void say(){
        System.out.println("我是一个动物");
    }
}

```

**Cat.java**

```java
package com.java.chap07.sec15;

/**
 * @author Yan
 * @date 2019/7/20 21:17
 */
public class Cat extends Animal {
    public void say(){
        System.out.println("我是一只猫");
    }


    /**
     * 子类方法
     */
    public void f2(){
        System.out.println("我喜欢吃鱼");
    }
}

```

**Dog.java**

```java
package com.java.chap07.sec15;

/**
 * @author Yan
 * @date 2019/7/20 21:17
 */
public class Dog extends Animal {
    public void say(){
        System.out.println("我是一只狗");
    }


    /**
     * 子类方法
     */
    public void f2(){
        System.out.println("我的名字是jack");
    }
}

```

**Test.java**

```java
package com.java.chap07.sec15;

/**
 * @author Yan
 * @date 2019/7/20 21:18
 */
public class Test {

    public static void doSomeThing(Animal animal){
        animal.say();
        if (animal instanceof Dog){
            ((Dog)animal).f2();
        }
        if (animal instanceof Cat){
            ((Cat)animal).f2();
        }
    }

    public static void main(String[] args) {
        Animal dog=new Dog();
        System.out.println("dog对象是否属于Animal类："+(dog instanceof Animal));
        System.out.println("dog对象是否属于Dog类："+(dog instanceof Dog));
        System.out.println("dog对象是否属于Cat类："+(dog instanceof Cat));

        doSomeThing(new Dog());
        doSomeThing(new Cat());
    }
}

```

## 匿名内部类

作用：假如某个类只使用一次，则可以使用匿名内部类。

代码示例：

**A.java**

```java
package com.java.chap07.sec16;

/**
 * @author Yan
 * @date 2019/7/20 21:34
 */
public interface A {

    public void a();
}

```

**B.java**

```java
package com.java.chap07.sec16;

/**
 * @author Yan
 * @date 2019/7/20 21:35
 */
public class B implements A{
    @Override
    public void a() {
        System.out.println("只使用一次");
    }
}

```

**Test.java**

```java
package com.java.chap07.sec16;

/**
 * @author Yan
 * @date 2019/7/20 21:35
 */
public class Test {
    public void test(A a){
        a.a();
    }

    public static void main(String[] args) {
        Test test=new Test();
        test.test(new B());

        //匿名内部类
        test.test(new A() {
            @Override
            public void a() {
                System.out.println("匿名内部类，一次性使用");
            }
        });
    }
}

```

## 包装类

每个基本类型都有一个对应的类；

| 序号 | 基本类型    | 包装类       |
|----|---------|-----------|
| 1  | int     | Integer   |
| 2  | char    | Character |
| 3  | short   | Short     |
| 4  | long    | Long      |
| 5  | float   | Float     |
| 6  | double  | Double    |
| 7  | boolean | Boolean   |
| 8  | byte    | Byte      |

1. 装箱和拆箱

```java
package com.java.chap07.sec17;

/**
 * @author Yan
 * @date 2019/7/20 22:02
 */
public class Demo1 {
    public static void main(String[] args) {
        int a=1;
        Integer i=new Integer(a);    //装箱  把基本变量变成对象变量
        int b=i.intValue();   //拆箱  把对象变量变成基本变量
        System.out.println("a="+a+",i="+i+",b="+b);
    }
}

```

1. 自动装箱和拆箱

```java
package com.java.chap07.sec17;

/**
 * @author Yan
 * @date 2019/7/20 22:05
 */
public class Demo2 {
    public static void main(String[] args) {
        Integer i=1;     //自动装箱的过程  自动把基本数据转换成对象
        int i2=i;   //自动拆箱   自动把对象转换成基本数据
        System.out.println("i="+i+",i2="+i2);
    }
}

```

1. 包装类的作用

```java
package com.java.chap07.sec17;

/**
 * @author Yan
 * @date 2019/7/20 22:07
 */
public class Demo3 {
    public static void main(String[] args) {
        String a="1";
        String b="2";

        int m=Integer.parseInt(a);
        int n=Integer.parseInt(b);
        System.out.println("a+b="+(m+n));
    }
}

```

## 设计模式

1. 单例模式
   在Java应用中，单例对象能保证在一个JVM中，该对象只有一个实例存在。

饿汉式

**Singleton1.java**

```java
package com.java.chap07.sec18;

/**
 * @author Yan
 * @date 2019/7/21 14:05
 */
public class Singleton1 {

    /**
     * 构造方法私有
     */
    private Singleton1(){

    }

    /**
     * 饿汉式单例实现
     */
    private static final Singleton1 single1=new Singleton1();


    /**
     * 静态工厂方式
     */
    public static Singleton1 getInstance(){
        return single1;
    }
}

```

**TestSingleton.java**

```java
package com.java.chap07.sec18;

/**
 * @author Yan
 * @date 2019/7/21 14:06
 */
public class TestSingleton {
    public static void main(String[] args) {
        //Singleton1 singleton1=new Singleton1();

        Singleton1 singleton1=Singleton1.getInstance();
        Singleton1 singleton2=Singleton1.getInstance();

        System.out.println("饿汉式："+(singleton1==singleton2));

        TestSingleton testSingleton=new TestSingleton();
        TestSingleton testSingleton2=new TestSingleton();
        System.out.println(testSingleton==testSingleton2);
    }
}

```

懒汉式

**Singleton2.java**

```java
package com.java.chap07.sec18;

/**
 * @author Yan
 * @date 2019/7/21 14:05
 */
public class Singleton2 {

    /**
     * 构造方法私有
     */
    private Singleton2(){

    }

    /**
     * 懒汉式单例实现    在第一次调用的时候实例化
     */
    private static Singleton2 single;

    /**
     * 工厂
     */
    public synchronized static Singleton2 getInstance(){
        if (single==null){
            //第一次调用的时候实例化
            System.out.println("第一次调用的时候实例化");
            single=new Singleton2();
        }
        return single;
    }
}

```

**TestSingleton.java**

```java
package com.java.chap07.sec18;

/**
 * @author Yan
 * @date 2019/7/21 14:06
 */
public class TestSingleton {
    public static void main(String[] args) {
        Singleton2 singleton3=Singleton2.getInstance();
        Singleton2 singleton4=Singleton2.getInstance();
        System.out.println("懒汉式："+(singleton3==singleton4));
    }
}
```
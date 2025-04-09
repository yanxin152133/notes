# 1. Java泛型
## 1.1. 泛型引入
定义：使用泛型可以指代任意对象类型
      
代码示例：     
       
**C1.java**
       
```java
package com.java.chap10.sec01;

/**
 * @author Yan
 * @date 2019/7/22 16:00
 */
public class C1 {
    private Integer a;

    public C1(Integer a) {
        super();
        this.a = a;
    }

    public Integer getA() {
        return a;
    }

    public void setA(Integer a) {
        this.a = a;
    }


    /**
     * 打印a的类型
     */
    public void print(){
        System.out.println("a的类型是："+a.getClass().getName());
    }
}

```

**C2.java**
                     
```java
package com.java.chap10.sec01;

/**
 * @author Yan
 * @date 2019/7/22 16:02
 */
public class C2 {
    private String a;

    public C2(String a) {
        super();
        this.a = a;
    }

    public String getA() {
        return a;
    }

    public void setA(String a) {
        this.a = a;
    }

    /**
     * 打印a的类型
     */
    public void print(){
        System.out.println("a的类型是："+a.getClass().getName());
    }
}

```
       
**C12.java**
       
```java
package com.java.chap10.sec01;

/**
 * @author Yan
 * @date 2019/7/22 16:09
 */
public class C12 {
    private Object object;

    public C12(Object object) {
        super();
        this.object = object;
    }

    public Object getObject() {
        return object;
    }

    public void setObject(Object object) {
        this.object = object;
    }

    /**
     * 打印object的类型
     */
    public void print(){
        System.out.println("a的类型是："+object.getClass().getName());
    }
}

```
         
**CC.java**
       
```java
package com.java.chap10.sec01;

/**
 * 定义泛型类
 * @author Yan
 * @date 2019/7/24 15:56
 */
public class CC<T> {

    private T ob;

    public CC(T ob) {
        super();
        this.ob = ob;
    }

    public T getOb() {
        return ob;
    }

    public void setOb(T ob) {
        this.ob = ob;
    }


    /**
     * 打印T的类型
     */
    public void print(){
        System.out.println("T的实际类型"+ob.getClass().getName());;
    }
}

```
       
**Test.java**
      
```java
package com.java.chap10.sec01;

/**
 * @author Yan
 * @date 2019/7/22 16:03
 */
public class Test1 {

    public static void main(String[] args) {
        //begin test c1
        C1 c1=new C1(1);
        c1.print();

        int i=c1.getA();
        System.out.println("i="+i);

        //end test c1

        //begin test c2
        C2 c2=new C2("Hi");
        c2.print();
        String s1=c2.getA();
        System.out.println("s1="+s1);
        //end test c2


        //begin test c12
        C12 c12=new C12(1);   //向上转型
        c12.print();
        int i12= (Integer) c12.getObject();   //向下转型
        System.out.println("i12="+i12);

        C12 c121=new C12("你好");  //向上转型
        c121.print();
        String i121=(String) c121.getObject();  //向下转型
        System.out.println("i121="+i121);
        //end test c12


        //begin test CC
        CC<Integer> cc=new CC<Integer>(1);
        cc.print();
        int icc=cc.getOb();
        System.out.println("icc="+icc);
        //end test CC

        //begin test CC
        CC<String> cc1=new CC<String>("你好");
        cc1.print();
        String icc1=cc1.getOb();
        System.out.println("icc1="+icc1);
        //end test CC
    }
}

```
## 1.2. 限制泛型
       
代码示例：     
     
**Animal.java**
       
```java
package com.java.chap10.sec02;

/**
 * @author Yan
 * @date 2019/7/24 16:07
 */
public class Animal {
    public void print(){
        System.out.println("动物");
    }
}

```
      
**Cat.java**
      
```
package com.java.chap10.sec02;

/**
 * @author Yan
 * @date 2019/7/24 16:07
 */
public class Cat extends Animal{
    public void print(){
        System.out.println("Cat");
    }
}

```
      
**Dog.java**
      
```java
package com.java.chap10.sec02;

/**
 * @author Yan
 * @date 2019/7/24 16:07
 */
public class Dog extends Animal{
    public void print(){
        System.out.println("Dog");
    }
}


```
        
**Demo.java**
      
```java
package com.java.chap10.sec02;

/**
 * @author Yan
 * @date 2019/7/24 16:08
 */

public class Demo <T extends Animal>{
    private T ob;

    public Demo(T ob) {
        super();
        this.ob = ob;
    }

    public T getOb() {
        return ob;
    }

    public void setOb(T ob) {
        this.ob = ob;
    }

    public void print(){
        System.out.println("T的类型是： "+ob.getClass().getName());
    }
}

```
       
**Test.java**
      
```java
package com.java.chap10.sec02;

/**
 * @author Yan
 * @date 2019/7/24 16:11
 */
public class Test {
    public static void main(String[] args) {
        Demo<Dog> demo=new Demo<Dog>(new Dog());
        Dog dog=demo.getOb();
        dog.print();

        Demo<Cat> demo2=new Demo<Cat>(new Cat());
        Cat cat=demo2.getOb();
        cat.print();


        //Demo<Integer> demo3=new Demo<Integer>(2);
        Demo<Animal> demo3=new Demo<Animal>(new Animal());
    }
}

```
## 1.3. 通配符泛型
       
代码示例：     
     
**Test.java**
       
```java
package com.java.chap10.sec03;

import com.java.chap10.sec02.Animal;
import com.java.chap10.sec02.Cat;
import com.java.chap10.sec02.Demo;
import com.java.chap10.sec02.Dog;

/**
 * @author Yan
 * @date 2019/7/24 16:23
 */
public class Test {

    /**
     * 通配符泛型
     * @param animal
     */
    private static void take(Demo<?> animal){
        animal.print();
    }
    public static void main(String[] args) {
        Demo<Dog> dog=new Demo<Dog>(new Dog());
        take(dog);

        Demo<Cat> cat=new Demo<Cat>(new Cat());
        take(cat);

        Demo<Animal> animalDemo=new Demo<Animal>(new Animal());
        take(animalDemo);
    }
}

```
       
## 1.4. 泛型方法
       
代码示例：     
     
```java
package com.java.chap10.sec04;

/**
 * @author Yan
 * @date 2019/7/24 19:39
 */
public class Test {


    /**
     * 泛型方法
     * @param t
     * @param <T>
     */
    public static <T> void f(T t){
        System.out.println("T的类型是："+t.getClass().getName());
    }
    public static void main(String[] args) {
        f("");
        f(1);
        f(1.0);
        f(new Object());
    }
}

```
       
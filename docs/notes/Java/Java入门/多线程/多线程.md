# 1. Java多线程
## 1.1. 多线程的引入
定义：同时对多项任务加以控制
      
代码示例：     
      
**Demo.java**
      
```java
package com.java.chap12.sec01;

/**
 * @author Yan
 * @date 2019/7/27 13:15
 */
public class Demo1 {

    /**
     * 听音乐
     */
    private static void music(){
        for (int i=0;i<1000;i++){
            System.out.println("听音乐");
        }
    }

    /**
     * 吃饭
     *
     */
    private static void eat(){
        for (int i=0;i<1000;i++){
            System.out.println("吃饭");
        }
    }
    public static void main(String[] args) {
        //music();
        //eat();

        /**
         * 利用多线程实现一边吃饭一边听歌
         */
        Music musicThread=new Music();
        Eat eatThread=new Eat();
        musicThread.start();
        eatThread.start();
    }
}

```
       
**Eat.java**
      
```java
package com.java.chap12.sec01;

/**
 * @author Yan
 * @date 2019/7/27 13:18
 */
public class Eat extends Thread {
    @Override
    public void run() {
        for (int i=0;i<1000;i++){
            try {
                Thread.sleep(100);
                System.out.println("吃饭");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```
       
**Music.java**
      
```java
package com.java.chap12.sec01;

/**
 * @author Yan
 * @date 2019/7/27 13:19
 */
public class Music extends Thread {
    @Override
    public void run() {
        for (int i=0;i<1000;i++){
            try {
                Thread.sleep(100);
                System.out.println("听音乐");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```
        
## 1.2. Java多线程实现
1. 继承Thread类
       
代码示例：     
     
```java
package com.java.chap12.sec02;

/**
 * @author Yan
 * @date 2019/7/27 13:24
 */
public class Thread1 extends Thread {


    private int baoZi=1;
    private String threadName;

    public Thread1(String threadName) {
        super();
        this.threadName = threadName;
    }

    @Override
    public synchronized void run() {
        while (baoZi<=10){
            System.out.println(threadName+"吃第"+baoZi+"包子");
            baoZi++;
        }
    }

    public static void main(String[] args) {
        System.out.println("张三，李四一起吃包子，每人吃了10个");
        Thread1 t1=new Thread1("张三线程");
        Thread1 t2=new Thread1("李四线程");
        t1.start();
        t2.start();
    }
}

```
      
2. 实现Runnable接口
      
代码示例：    
     
```java
package com.java.chap12.sec02;

/**
 * @author Yan
 * @date 2019/7/27 13:29
 */
public class Thread2 implements Runnable {
    private int baoZi=1;
    private String threadName;

    public Thread2(String threadName) {
        super();
        this.threadName = threadName;
    }

    @Override
    public synchronized void run() {
        while (baoZi<=10){
            System.out.println(threadName+"吃第"+baoZi+"包子");
            baoZi++;
        }
    }

    public static void main(String[] args) {
        //System.out.println("张三，李四一起吃包子，每人吃了10个");
        //Thread2 t1=new Thread2("张三线程");
        //Thread2 t2=new Thread2("李四线程");
        //Thread t11=new Thread(t1);
        //Thread t12=new Thread(t2);
        //t11.start();
        //t12.start();

        Thread2 t1=new Thread2("超级张三线程");
        Thread t11=new Thread(t1);
        Thread t12=new Thread(t1);
        Thread t13=new Thread(t1);
        Thread t14=new Thread(t1);
        //实现资源共享
        t11.start();
        t12.start();
        t13.start();
        t14.start();
    }
}

```
      
## 1.3. 线程状态
      
![](https://live.staticflickr.com/65535/48385151952_855767c53c_z.jpg)
      
>1. 创建状态
在程序中用构造方法创建了一个线程对象后，新的线程对象便处于新建状态，此时，它已经有了相应的内存空间和其他资源，但还处于不可运行状态。新建一个线程对象可采用Thread类的构造方法来实现，例如，“Thread thread=new Thread();”。
      
>2. 就绪状态
新建线程对象后，调用该线程的start()方法就可以启动线程。当线程启动时，线程进入就绪状态。此时，线程将进入线程队列排队，等待CPU服务，这表明它已经具备了运行条件。
        
>3. 运行状态
当就绪状态的线程被调用并获得处理器资源时，线程就进入了运行状态。此时，自动调用该线程对象的run()方法。run()方法定义了该线程的操作和功能。
       
>4. 堵塞状态
一个正在运行的线程在某些特殊情况下，如被人为挂起或需要执行耗时的输入/输出操作时，将让出CPU并暂时中止自己的执行，进入堵塞状态，堵塞时，线程不能进入排队队列，只有当引起堵塞的原因被消除后，线程才可以转入就绪状态。
        
>5. 死亡状态
线程调用stop()方法时或run()方法执行结束后，即处于死亡状态。处于死亡状态的线程不具有继续运行的能力。
         
## 1.4. 线程常用方法
1. getName(); 返回该线程的名称。
2. currentThread(); 返回对当前正在执行的线程对象的引用。
3. isAlive(); 测试线程是否处于活动状态。
4. sleep(); 线程休眠。
5. setPriority(int newPriority); 更改线程的优先级。
6. yield(); 暂停当前正在执行的线程对象，并执行其他线程。

代码示例：     
           
**getName()、currentThread()**
      
```java
package com.java.chap12.sec04;

import javax.swing.plaf.TableHeaderUI;

/**
 * @author Yan
 * @date 2019/7/27 14:23
 */
public class Demo1 implements Runnable{

    @Override
    public void run() {
        for (int i=0;i<10;i++){
            //获取当前线程
            Thread t=Thread.currentThread();
            System.out.println(t.getName()+":"+i);  //返回线程的名称
        }
    }


    public static void main(String[] args) {
        Demo1 demo1=new Demo1();
        new Thread(demo1).start();
        new Thread(demo1).start();
        new Thread(demo1,"线程3").start();
    }
}

```
        
**isAlive()**
      
```java
package com.java.chap12.sec04;

/**
 * @author Yan
 * @date 2019/7/27 14:35
 */
public class Demo2 implements Runnable{
    @Override
    public void run() {
        for (int i=0;i<10;i++){
            //获取当前线程
            Thread t=Thread.currentThread();
            System.out.println(t.getName()+":"+i);  //返回线程的名称
        }
    }

    public static void main(String[] args) {
        Demo2 demo2=new Demo2();
        Thread t1=new Thread(demo2);
        System.out.println("t1是否活动："+t1.isAlive());
        t1.start();
        System.out.println("t1是否活动："+t1.isAlive());
    }
}

```
       
**sleep()**
     
```java
package com.java.chap12.sec04;

/**
 * @author Yan
 * @date 2019/7/27 14:35
 */
public class Demo3 implements Runnable{
    @Override
    public void run() {
        for (int i=0;i<10;i++){
            try {
                Thread.sleep(1000);
                //获取当前线程
                Thread t=Thread.currentThread();
                System.out.println(t.getName()+":"+i);  //返回线程的名称
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        Demo3 demo2=new Demo3();
        Thread t1=new Thread(demo2);
        t1.start();
    }
}
```
       
**setPriority(int newPriority)**
     
```java
package com.java.chap12.sec04;

/**
 * @author Yan
 * @date 2019/7/27 14:35
 */
public class Demo4 implements Runnable{
    @Override
    public void run() {
        for (int i=0;i<10;i++){
            try {
                Thread.sleep(1000);
                //获取当前线程
                Thread t=Thread.currentThread();
                System.out.println(t.getName()+":"+i);  //返回线程的名称
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        Demo4 demo4=new Demo4();
        Thread t1=new Thread(demo4,"线程A");
        Thread t2=new Thread(demo4,"线程B");
        Thread t3=new Thread(demo4,"线程C");
        t1.setPriority(Thread.MAX_PRIORITY);
        t2.setPriority(Thread.MIN_PRIORITY);
        t3.setPriority(Thread.NORM_PRIORITY);
        t1.start();
        t2.start();
        t3.start();
    }
}

```
       
**yield()**
       
```java
package com.java.chap12.sec04;

/**
 * @author Yan
 * @date 2019/7/27 14:35
 */
public class Demo5 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                Thread.sleep(1000);
                //获取当前线程
                Thread t = Thread.currentThread();
                System.out.println(t.getName() + ":" + i);  //返回线程的名称
                if (i==5){
                    System.out.println("线程礼让");
                    Thread.currentThread().yield();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        Demo5 demo5 = new Demo5();
        new Thread(demo5, "线程A").start();
        new Thread(demo5, "线程B").start();

    }
}

```
          
## 1.5. 线程同步
1. 同步方法
       
2. 同步锁
      
代码示例：     
     
**Demo2.java**
     
```java
package com.java.chap12.sec05;

/**
 * @author Yan
 * @date 2019/7/27 14:54
 */
public class Demo2 implements Runnable {

    private int baoZi=10;

    /**
     * 同步方法
     */
    @Override
    public synchronized void run() {
        while (baoZi>0){
            System.out.println(Thread.currentThread().getName()+"吃了第"+baoZi+"包子");
            baoZi--;
        }
    }


    public static void main(String[] args) {
        Demo2 demo1=new Demo2();
        new Thread(demo1,"张三").start();
        new Thread(demo1,"李四").start();
        new Thread(demo1,"王五").start();
    }
}

```
      
**Demo3.java**
      
```java
package com.java.chap12.sec05;

/**
 * @author Yan
 * @date 2019/7/27 14:54
 */
public class Demo3 implements Runnable {

    private int baoZi = 10;

    @Override
    public void run() {
        /**
         * 同步块
         */
        synchronized (this) {
            while (baoZi > 0) {
                System.out.println(Thread.currentThread().getName() + "吃了第" + baoZi + "包子");
                baoZi--;
            }

        }
    }


    public static void main(String[] args) {
        Demo3 demo1 = new Demo3();
        new Thread(demo1, "张三").start();
        new Thread(demo1, "李四").start();
        new Thread(demo1, "王五").start();
    }
}

```
       
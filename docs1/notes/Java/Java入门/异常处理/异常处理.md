# 1. Java异常处理
## 1.1. 异常的概念
      
代码示例：     
      
```java
package com.java.chap08.sec01;

/**
 * @author Yan
 * @date 2019/7/21 14:30
 */
public class ExceptionDemo {
    public static void main(String[] args) {
        String str="123a";
        int a=Integer.parseInt(str);
        System.out.println(a);
    }
}

```
## 1.2. 捕获和处理异常
在Java中，我们用try...catch...来捕获异常；   

```java
try...catch...finally
```
        
代码示例：     
      
```java
package com.java.chap08.sec02;

/**
 * @author Yan
 * @date 2019/7/21 14:32
 */
public class Demo1 {
    public static void main(String[] args) {
        String str="123a";
        try {
            int a=Integer.parseInt(str);
        }catch (NumberFormatException e){
            e.printStackTrace();
        }

        
        System.out.println("aaaa");
    }

}
```
      
```java
package com.java.chap08.sec02;

/**
 * @author Yan
 * @date 2019/7/21 15:59
 */
public class Demo2 {

    public static void testFinally(){
        String str="123a";
        try{
            int a=Integer.parseInt(str);
            System.out.println(a);
        }catch (Exception e){
            e.printStackTrace();
            System.out.println("exception");
            return;
        }finally {
            System.out.println("final end");
        }
        System.out.println("end");
    }

    public static void main(String[] args) {
        testFinally();
    }
}

```
## 1.3. throws和throw关键字
throws表示当前方法不处理异常，而是交给方法的调用处去处理。       
throw表示直接抛出一个异常
        
代码示例：      
      
**throws**
       
```java
package com.java.chap08.sec03;

/**
 * @author Yan
 * @date 2019/7/21 16:03
 */
public class Demo1 {
    /**
     * 把异常向外抛
     * @throws NumberFormatException
     */
    public static void testThrows() throws NumberFormatException {
        String str = "123a";
        int a = Integer.parseInt(str);
        System.out.println(a);
    }


    public static void main(String[] args) {
        try {

            testThrows();
        }catch (Exception e){
            System.out.println("在这里处理异常");
            e.printStackTrace();
        }
        System.out.println("I'm here");
    }
}

```
         
**throw**
        
```java
package com.java.chap08.sec03;

/**
 * @author Yan
 * @date 2019/7/21 16:07
 */
public class Demo2 {

    public static void testThrow(int a) throws Exception{
        if (a==1){

            //直接抛出一个异常类
            throw new Exception("有异常");

        }
        System.out.println(a);
    }

    public static void main(String[] args) {
        try {
            testThrow(1);
        } catch (Exception e) {
            e.printStackTrace();
        }

        try {
            testThrow(0);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```
## 1.4. Exception和RuntimeException区别
Exception 是检查型异常，例如Exception在程序中必须使用try...catch进行处理；

RuntimeException 是非检查型异常，例如NumberFormatException，可以不使用try...catch进行处理，但是如果产生异常，则异常将由JVM进行处理；       

RuntimeException 最好也用try...catch捕获。

代码示例：     
          
```java
package com.java.chap08.sec04;

/**
 * @author Yan
 * @date 2019/7/21 16:12
 */
public class Demo1 {

    /**
     * 运行时异常，编译时不检查，可以不适用try...catch捕获
     * @throws RuntimeException
     */
    public static void testRuntimeException() throws RuntimeException {
        throw new RuntimeException("运行时异常");
    }

    /**
     * Exception异常，编译时异常，必须使用try...catch捕获
     * @throws Exception
     */
    public static void testException() throws Exception{
        throw new Exception("Exception异常");
    }

    public static void main(String[] args) {
        try{
            testRuntimeException();

        }catch (Exception e){
            e.printStackTrace();
        }
        try {
            testException();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```
       
## 1.5. 自定义异常类
代码示例：    
      
**CustomerException.java**
       
```java
package com.java.chap08.sec05;

/**
 * 自定义异常，继承自Exception
 * @author Yan
 * @date 2019/7/21 16:18
 */
public class CustomerException extends Exception{


    public CustomerException(String message){
        super(message);
    }



}

```
       
**TestCustomerException.java**
       
```java
package com.java.chap08.sec05;

/**
 * @author Yan
 * @date 2019/7/21 16:19
 */
public class TestCustomerException {

    public static void test() throws CustomerException{
        throw new CustomerException("自定义异常");
    }

    public static void main(String[] args) {
        try {
            test();
        } catch (CustomerException e) {
            e.printStackTrace();
        }
    }
}

```
        
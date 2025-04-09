# Java IO流

## IO流简介

流是一组有顺序的，有起点和终点的字节集合，是对数据传输的总称或抽象。即数据再两设备间的传输称为流，流的本质是数据传输，根据数据传输特性将流抽象为各种类，方便更直观的进行数据操作。

![](https://live.staticflickr.com/65535/48411733786_ac1f14f1b1_b.jpg)

> IO 流的分类
> 根据处理数据类型的不同分类：字符流和字节流
> 根据数据流向不同分为：输入流和输出流

## 文件操作File类

1. public boolean mkdir()：创建此抽象路径名指定的目录
2. public boolean createNewFile():创建一个文件
3. public boolean delete():删除此抽象路径名表示的文件或目录。如果此路径名表示一个目录，则该目录必须为空才能删除
4. public boolean exists():测试此抽象路径名表示的文件或目录是否存在
5. public File[] listFiles():返回一个抽象路径名数组，这些路径名表示此抽象路径名表示的目录中的文件
6. public boolean idDirectory():测试此抽象路径名表示的文件是否是一个目录

代码示例：

**Demo1.java**

```java
package com.java.chap14.sec02;

import java.io.File;
import java.io.IOException;

/**
 * @author Yan
 * @date 2019/7/30 15:43
 */
public class Demo1 {
    public static void main(String[] args) throws IOException {
        File file=new File("D://java创建的目录");
        boolean b=file.mkdir();   //创建虚拟目录
        if (b){
            System.out.println("目录创建成功");
            file=new File("D://java创建的目录//java创建的文件.txt");
            boolean b2=file.createNewFile();   //创建文件
            if (b2){
                System.out.println("文件创建成功");
            }else {
                System.out.println("文件创建失败");
            }

        }else {
            System.out.println("目录创建失败");
        }
    }
}

```

**Demo2.java**

```java
package com.java.chap14.sec02;

import java.io.File;
import java.io.IOException;

/**
 * @author Yan
 * @date 2019/7/30 15:43
 */
public class Demo2 {
    public static void main(String[] args) throws IOException {
        File file=new File("D://java创建的目录//Java创建的文件.txt");
        if (file.exists()){  //假如目录存在
            boolean b=file.delete();    //删除文件
            if (b){
                System.out.println("删除文件成功");
            }else {
                System.out.println("删除文件失败");
            }
        }
        file=new File("D://java创建的目录");
        if (file.exists()){
            boolean b2=file.delete();   //删除目录
            if (b2){
                System.out.println("删除目录成功");
            }else {
                System.out.println("删除目录失败");
            }
        }
    }
}

```

**Demo3.java**

```java
package com.java.chap14.sec02;

import java.io.File;

/**
 * @author Yan
 * @date 2019/7/30 17:21
 */
public class Demo3 {
    public static void main(String[] args) {
        File file=new File("D://图书");
        File files[]=file.listFiles();    //遍历目录
        for (int i=0;i<files.length;i++){
            System.out.println(files[i]);
        }
    }
}

```

**Demo4.java**

```java
package com.java.chap14.sec02;

import java.io.File;

/**
 * @author Yan
 * @date 2019/7/30 17:45
 */
public class Demo4 {

    /**
     * 打印文件
     * @param file
     */
    public static void listFile(File file) {
        if (file != null) {
            if (file.isDirectory()) {  //是目录
                File files[]=file.listFiles();   //遍历目录
                if (files!=null){
                    for (int i=0;i<files.length;i++){
                        listFile(files[i]);   //递归调用
                    }
                }
            } else {    //是文件
                System.out.println(file);    //是文件，直接打印文件的路径
            }
        }
    }

    public static void main(String[] args) {
        File file=new File("D://实验报告");
        listFile(file);
    }
}

```

## 字节输入，输出流

1. InputStream 读取文件

代码示例：

**Demo1.java**

```java
package com.java.chap14.sec03;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;

/**
 * @author Yan
 * @date 2019/7/31 14:36
 */
public class Demo1 {

    public static void main(String[] args) throws Exception {
        File file=new File("D://测试文件.txt");
        InputStream inputStream=new FileInputStream(file);   //实例化FileInputStream
        byte b[]=new byte[1024];
        int len=inputStream.read(b);
        inputStream.read(b);
        inputStream.close();   //关闭输出流
        System.out.println("读取的内容："+new String(b,0,len));
    }
}

```

**Demo2.java**

```java
package com.java.chap14.sec03;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;

/**
 * @author Yan
 * @date 2019/7/31 14:36
 */
public class Demo2 {

    public static void main(String[] args) throws Exception {
        File file=new File("D://测试文件.txt");
        InputStream inputStream=new FileInputStream(file);   //实例化FileInputStream
        int fileLength= (int) file.length();
        byte b[]=new byte[fileLength];
        int len=inputStream.read(b);
        inputStream.read(b);
        inputStream.close();   //关闭输出流
        System.out.println("读取的内容："+new String(b));
    }
}

```

**Demo3.java**

```java
package com.java.chap14.sec03;

import java.io.File;
import java.io.FileInputStream;
import java.io.InputStream;

/**
 * @author Yan
 * @date 2019/7/31 14:36
 */
public class Demo3 {

    public static void main(String[] args) throws Exception {
        File file=new File("D://测试文件.txt");
        InputStream inputStream=new FileInputStream(file);   //实例化FileInputStream
        int fileLength= (int) file.length();
        byte b[]=new byte[fileLength];
        int temp=0;
        int len=0;
        while ((temp=inputStream.read())!=-1){
            //一个字节一个字节读取，放到b字节数组里
            b[len++]= (byte) temp;
        }
        inputStream.close();   //关闭输出流
        System.out.println("读取的内容："+new String(b));
    }
}

```

2. OutputStream 写入文件

代码示例：

**Demo4.java**

```java
package com.java.chap14.sec03;

import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;

/**
 * @author Yan
 * @date 2019/7/31 15:38
 */
public class Demo4 {
    public static void main(String[] args) throws Exception {
        File file=new File("D://测试文件.txt");
        OutputStream outputStream=new FileOutputStream(file);
        String str="你好，我好，大家好";
        byte b[]=str.getBytes();
        outputStream.write(b);   //将b字节数组写入到输出流中
        outputStream.close();   //关闭输出流
    }
}

```

**Demo5.java**

```java
package com.java.chap14.sec03;

import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStream;

/**
 * @author Yan
 * @date 2019/7/31 15:38
 */
public class Demo5 {
    public static void main(String[] args) throws Exception {
        File file=new File("D://测试文件.txt");
        OutputStream outputStream=new FileOutputStream(file,true);  //true追加
        String str="你好，我好，大家好";
        byte b[]=str.getBytes();
        outputStream.write(b);   //将b字节数组写入到输出流中
        outputStream.close();   //关闭输出流
    }
}

```

3. BufferedInputStream和BufferedOutputStream

代码示例：

```java
package com.java.chap14.sec03;

import java.io.*;

/**
 * @author Yan
 * @date 2019/7/31 22:31
 */
public class Demo6 {

    /**
     * 缓冲
     *
     * @throws Exception
     */
    public static void bufferStream() throws Exception {
        //定义一个带缓冲的字节输入流
        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("D://实验报告//android//Android应用开发实验指导书.doc"));
        //定义一个带缓冲的字节输出流
        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(new FileOutputStream("D://复制的文件2.doc"));
        int b = 0;
        Long startTime = System.currentTimeMillis();  //开始时间
        while ((b = bufferedInputStream.read()) != -1) {
            bufferedOutputStream.write(b);
        }
        bufferedInputStream.close();
        bufferedOutputStream.close();
        Long endTime = System.currentTimeMillis();   //结束时间
        System.out.println("缓冲花费的时间：" + (endTime - startTime));
    }


    /**
     * 非缓冲
     *
     * @throws Exception
     */
    public static void stream() throws Exception {
        InputStream inputStream = new FileInputStream("D://实验报告//android//Android应用开发实验指导书.doc");  //定义一个输入流
        OutputStream outputStream = new FileOutputStream("D://复制的文件.doc");
        int b = 0;
        Long startTime = System.currentTimeMillis();  //开始时间
        while ((b = inputStream.read()) != -1) {
            outputStream.write(b);
        }
        inputStream.close();
        outputStream.close();
        Long endTime = System.currentTimeMillis();   //结束时间
        System.out.println("非缓冲花费的时间：" + (endTime - startTime));
    }

    public static void main(String[] args) throws Exception {
        stream();

        bufferStream();
    }
}

```

4. 缓冲和非缓冲的区别及性能对比

## 字符输入、输出流

1. Reader读取文件

代码示例：

**Demo1.java**

```java
package com.java.chap14.sec04;

import java.io.File;
import java.io.FileReader;
import java.io.Reader;

/**
 * @author Yan
 * @date 2019/7/31 23:00
 */
public class Demo1 {
    public static void main(String[] args) throws Exception {
        File file = new File("D://测试文件.txt");
        Reader reader=new FileReader(file);
        char c[]=new char[1024];   //字符数组
        int len=reader.read(c);
        reader.close();   //关闭输入流
        System.out.println("读取的内容是："+new String(c,0,len));
    }
}

```

**Demo2.java**

```java
package com.java.chap14.sec04;

import java.io.File;
import java.io.FileReader;
import java.io.Reader;

/**
 * @author Yan
 * @date 2019/7/31 23:04
 */
public class Demo2 {
    public static void main(String[] args) throws Exception {
        File file = new File("D://测试文件.txt");
        Reader reader=new FileReader(file);
        char c[]=new char[1024];   //字符数组
        int temp=0;
        int len=0;
        while ((temp=reader.read())!=-1){
            c[len++]= (char) temp;
        }
        reader.close();   //关闭输入流
        System.out.println("读取的内容是："+new String(c,0,len));
    }
}

```

2. Writer写入文件

代码示例：

**Demo3.java**

```java
package com.java.chap14.sec04;


import java.io.*;

/**
 * @author Yan
 * @date 2019/7/31 23:06
 */
public class Demo3 {
    public static void main(String[] args) throws Exception {
        File file = new File("D://测试文件.txt");
        Writer out = new FileWriter(file);
        String str = "我爱中华";
        out.write(str);   //将字符串写入输出流
        out.close();   //关闭输出流
    }
}

```

**Demo4.java**

```java
package com.java.chap14.sec04;


import java.io.File;
import java.io.FileWriter;
import java.io.Writer;

/**
 * @author Yan
 * @date 2019/7/31 23:06
 */
public class Demo4 {
    public static void main(String[] args) throws Exception {
        File file = new File("D://测试文件.txt");
        Writer out = new FileWriter(file,true);
        String str = "我爱中华";
        out.write(str);   //将字符串写入输出流
        out.close();   //关闭输出流
    }
}

```

### 待续
       
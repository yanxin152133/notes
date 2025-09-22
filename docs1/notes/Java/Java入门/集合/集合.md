# 1. Java集合
## 1.1. Java集合的引入
      
代码示例：      
           
**Student.java**
       
```java
package com.java.chap11.sec01;

/**
 * @author Yan
 * @date 2019/7/25 14:40
 */
public class Student {

    private String name;
    private int age;

    public Student(String name, int age) {
        super();
        this.name = name;
        this.age = age;
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
}

```
       
**Demo.java**
      
```java
package com.java.chap11.sec01;

import java.util.LinkedList;

/**
 * @author Yan
 * @date 2019/7/25 14:41
 */
public class Demo {

    public static void main(String[] args) {
        Student student[]=new Student[3];
        Student student1=new Student("张三",1);
        Student student2=new Student("李四",3);
        Student student3=new Student("王五",4);


        LinkedList<Student> list=new LinkedList<Student>();
        list.add(student1);
        list.add(student2);
        list.add(student3);


        /**
         * 遍历集合
         */
        for (int i=0;i<list.size();i++){
            Student student4=list.get(i);
            System.out.println(student4.getName()+":"+student4.getAge());
        }
    }
}

```
            
## 1.2. List集合
是Collection接口的子接口，也是最常用的接口。此接口对Collection接口进行了大量的扩展，List集合里的元素是允许重复的。      
       
1. ArrayList实现类
       
代码实现：    
      
```java
package com.java.chap11.sec02;

import java.util.ArrayList;

/**
 * @author Yan
 * @date 2019/7/25 15:09
 */
public class TestArrayList {
    public static void printArrayList(ArrayList<String> arrayList){
        System.out.println("当前的集合元素");
        for (int i=0;i<arrayList.size();i++){
            System.out.print(arrayList.get(i)+" ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        ArrayList<String> arrayList=new ArrayList<String>();
        //添加元素
        arrayList.add("张三");
        arrayList.add("李四");

        printArrayList(arrayList);

        //将指定的元素插入此列表中的指定位置
        arrayList.add(1,"小张三");
        printArrayList(arrayList);

        //用指定的元素替代此列表指定位置上的元素
        arrayList.set(2,"小李四");
        printArrayList(arrayList);

        //移除此列表中指定位上的元素
        arrayList.remove(0);
        printArrayList(arrayList);
    }
}

```
2. LinkedList实现类
      
代码示例：    
     
```java
package com.java.chap11.sec02;

import java.util.LinkedList;

/**
 * @author Yan
 * @date 2019/7/25 15:36
 */
public class TestLinkedList {

    public static void printLinkedList(LinkedList<String> linkedList){
        System.out.println("当前元素集合：");
        for (int i=0;i<linkedList.size();i++){
            System.out.print(linkedList.get(i)+"  ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        LinkedList<String> linkedList=new LinkedList<String>();
        linkedList.add("张三");
        linkedList.add("李四");
        linkedList.add("王五");
        linkedList.add("李四");
        linkedList.add("赵六");

        printLinkedList(linkedList);

        //返回此列表中首次出现的指定元素的索引，如果此列表中不包括该元素，则返回-1；
        System.out.println(linkedList.indexOf("李四"));

        //获取但不移除此列表的第一个元素；如果此列表为空，则返回null
        System.out.println(linkedList.peekFirst());
        printLinkedList(linkedList);

        //获取但不移除此列表的最后一个元素；如果此列表为空，则返回null
        System.out.println(linkedList.peekLast());
        printLinkedList(linkedList);

        //获取并移除此列表的第一个元素；如果此列表为空，则返回null
        System.out.println(linkedList.pollFirst());
        printLinkedList(linkedList);

        //获取并移除此列表的最后一个元素；如果此列表为空，则返回null
        System.out.println(linkedList.pollLast());
        printLinkedList(linkedList);
    }
}

```
        
## 1.3. 集合的遍历
1. Iterator
      
代码示例：     
      
```java
package com.java.chap11.sec03;

import com.java.chap11.sec01.Student;

import java.util.Iterator;
import java.util.LinkedList;

/**
 * @author Yan
 * @date 2019/7/25 15:51
 */
public class TestIterator {

    public static void main(String[] args) {
        LinkedList<Student> list=new LinkedList<Student>();
        list.add(new Student("张三",10));
        list.add(new Student("李四",20));
        list.add(new Student("王五",30));


        /**
         * 用Iterator遍历集合
         */
        Iterator<Student> iterator=list.iterator();   //返回一个迭代器
        while(iterator.hasNext()){
            Student s=iterator.next();    //返回迭代的下一个元素
            System.out.println("姓名："+s.getName()+"   年龄："+s.getAge());
        }

    }
}

```
        
2. foreach
      
代码示例：     
     
```java
package com.java.chap11.sec03;

import com.java.chap11.sec01.Student;

import java.util.Iterator;
import java.util.LinkedList;

/**
 * @author Yan
 * @date 2019/7/25 15:57
 */
public class TestForeach {
    public static void main(String[] args) {
        LinkedList<Student> list=new LinkedList<Student>();
        list.add(new Student("张三",10));
        list.add(new Student("李四",20));
        list.add(new Student("王五",30));


        /**
         * 用Foreach遍历集合
         */
        for (Student s:list){
            System.out.println("姓名："+s.getName()+"   年龄："+s.getAge());

        }

    }
}

```
       
## 1.4. Set集合
是Collertion接口的子接口，没有对Collection接口进行扩展，里面不允许存重复的内容。     
       
1. HashSet类
        
代码示例：     
     
```java
package com.java.chap11.sec04;

import java.util.HashSet;
import java.util.Iterator;

/**
 * @author Yan
 * @date 2019/7/25 16:02
 */
public class TestHashSet {

    public static void main(String[] args) {
        /**
         * 1. HashSet是无序的
         * 2. 不允许有重复值
         */
        HashSet<String> hashSet=new HashSet<String>();
        hashSet.add("1");
        hashSet.add("2");
        hashSet.add("5");
        hashSet.add("4");
        hashSet.add("2");

        /**
         * 用Iterator遍历集合
         */
        Iterator<String> iterator=hashSet.iterator();
        while (iterator.hasNext()){
            String s=iterator.next();
            System.out.println(s+" ");
        }


    }
}

```

## 1.5. Map集合
是存放一对值的最大接口，即接口中的每一个元素都是一对，以key->value键值对的形式保存。     
      
1. HashMap类
      
代码示例：     
        
```java
package com.java.chap11.sec05;

import com.java.chap11.sec01.Student;

import java.util.HashMap;
import java.util.Iterator;

/**
 * @author Yan
 * @date 2019/7/25 16:08
 */
public class TestHashMap {
    public static void main(String[] args) {
        HashMap<String, Student> hashMap=new HashMap<String,Student>();
        hashMap.put("1号",new Student("张三",10));
        hashMap.put("2号",new Student("李四",20));
        hashMap.put("3号",new Student("王五",30));

        // 通过key获取value
        Student student=hashMap.get("1号");
        System.out.println(student.getName()+":"+student.getAge());

        Iterator<String> iterator=hashMap.keySet().iterator();  //获取key的集合，再获取迭代器
        while (iterator.hasNext()){
            String key=iterator.next();    //获取key
            Student student1=hashMap.get(key);   //通过key获取value
            System.out.println("key="+key+"value=["+student1.getName()+","+student1.getAge()+"]");
        }
    }
}

```
      
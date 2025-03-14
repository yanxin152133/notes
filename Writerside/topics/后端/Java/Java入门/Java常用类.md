# Java常用类

## Java日期处理类

1. Date类

```java
package com.java.chap09.sec01;

import java.util.Date;

public class TestDate {
    public static void main(String[] args) {
        Date date=new Date();
        System.out.println("当前日期："+date);
    }
}

```

1. Calendar类

```java
package com.java.chap09.sec01;

import java.util.Calendar;


public class TestCalendar {
    public static void main(String[] args) {
        Calendar calendar = Calendar.getInstance();
        System.out.println(calendar.get(Calendar.YEAR));
        System.out.println(calendar.get(Calendar.MONTH) + 1);
        System.out.println("现在是" + calendar.get(Calendar.YEAR) + "年"
                + (calendar.get(Calendar.MONTH) + 1) + "月"
                + calendar.get(Calendar.DAY_OF_MONTH) + "日"
                + calendar.get(Calendar.HOUR_OF_DAY) + "时"
                + calendar.get(Calendar.MINUTE) + "分"
                + calendar.get(Calendar.SECOND) + "秒");
    }
}

```

1. SimpleDateFormat

```java
package com.java.chap09.sec01;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TestSimpleDateFormat {

    /**
     * 将日期对象格式为指定格式的日期字符串
     * @param date 传入的日期对象
     * @param format  格式
     * @return
     */
    public static String formatDate(Date date,String format){
        String result="";
        SimpleDateFormat sdf=new SimpleDateFormat(format);
        if (date!=null){
            result=sdf.format(date);
        }
        return result;
    }

    /**
     * 将日期字符串转换成一个日期对象
     * @param dataStr 日期字符串
     * @param format 格式
     * @return
     */
    public static Date formatDate(String dataStr,String format) throws ParseException {
        SimpleDateFormat sdf=new SimpleDateFormat(format);

        return sdf.parse(dataStr);
    }

    public static void main(String[] args) throws ParseException {
        Date date=new Date();
        //SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        //System.out.println(sdf.format(date));

        String dataStr="2010-07-21 21:12:26";
        Date date1=formatDate(dataStr,"yyyy-MM-dd HH:mm:ss");

        System.out.println(formatDate(date,"yyyy-MM-dd HH:mm:ss"));


        System.out.println(formatDate(date1,"yyyy-MM-dd HH:mm:ss"));
    }
}

```

## String VS StringBuffer

String：对String类型的对象操作，等同于重新生成一个对象，然后将引用指向它；

StringBuffer：对StringBuffer类型的对象操作，操作的始终是用一个对象。

1. String

![](https://live.staticflickr.com/65535/48344498272_fa4a07241c_z.jpg)

代码示例：

```java
package com.java.chap09.sec02;

/**
 * @author Yan
 * @date 2019/7/22 14:00
 */
public class TestString {
    public static void main(String[] args) {
        String str="123";
        str+="abc";
        System.out.println(str);
    }
}

```

1. StringBuffer

![](https://live.staticflickr.com/65535/48344384271_961416d201_z.jpg)

代码示例：

```java
package com.java.chap09.sec02;

/**
 * @author Yan
 * @date 2019/7/22 14:01
 */
public class TestStringBuffer {
    public static void main(String[] args) {
        StringBuffer sb=new StringBuffer("123");
        sb.append("abc");
        System.out.println(sb);
    }
}

```

## Math类

> 1. max方法：求最大值
>2. min方法：求最小值
>3. round方法：四舍五入
>4. pow方法：求次幂

代码示例：

```java
package com.java.chap09.sec03;

/**
 * @author Yan
 * @date 2019/7/22 14:10
 */
public class TestMath {
    public static void main(String[] args) {
        System.out.println("最大值："+Math.max(1,2));
        System.out.println("最小值："+Math.min(1,2));
        System.out.println("四舍五入："+Math.round(12.4));
        System.out.println("次幂："+Math.pow(3,4));
    }
}

```

## Arrays类

> 1. toString()方法：返回指定数组内容的字符串表示形式
>2. sort()方法：对指定的类型数组按数字升序进行排序
>3. binarySearch()方法：使用二分搜索法来搜索指定类型数组，以获得指定的值
>4. fill()方法：将指定类型值分配给指定类型数组的每个元素

代码示例：

```java
package com.java.chap09.sec04;

import java.util.Arrays;

/**
 * @author Yan
 * @date 2019/7/22 14:36
 */
public class TestArrays {
    public static void main(String[] args) {
        int arr[]={1,7,3,8,2};
        System.out.println("以字符串形式输出数组："+Arrays.toString(arr));

        Arrays.sort(arr);   //给数组排序
        System.out.println("排序后的数组"+Arrays.toString(arr));

        System.out.println(Arrays.binarySearch(arr,7));

        Arrays.fill(arr,0);   //将指定内容填充到数组中
        System.out.println("填充数组后的字符串："+Arrays.toString(arr));
    }
}
```

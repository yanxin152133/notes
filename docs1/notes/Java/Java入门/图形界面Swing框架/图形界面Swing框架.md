# 1. Java图形界面Swing框架
## 1.1. Swing简介
1. Swing是Java的一个图形框架，继承自AWT；
2. Swing主要涉及到容器，组件，还有布局管理器；
3. Swing与用户交互的时候还涉及到事件概念
       
## 1.2. JFrame容器
1. public void setVisible(boolean b):根据参数b的值显示或隐藏此窗体
     
2. public void setSize(int width,int height):调整组件的大小，使其宽度为width，高度为height
             
3. public void setLocation(int x,.int y):将组件移到新位置。通过此组件父级坐标空间中的x和y参数来指定新位置的左上角
                  
4. public Container getContentPane():返回此窗体的contentPane对象
       
5. public void setBackground(Color c):设置组件的背景色
       
代码示例：     
      
```java
package com.java.chap13.sec02;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/27 15:38
 */
public class JFrameTest {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("JFrame窗体");
        /*Container c=jFrame.getContentPane();
        c.setBackground(Color.blue);*/
        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
    }
}

```
      
## 1.3. JButton组件
      
代码示例：     
      
```java
package com.java.chap13.sec03;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/27 16:18
 */
public class JButtonTest {

    public static void main(String[] args) {
        JFrame jFrame=new JFrame("JButton测试");
        JButton jButton=new JButton("这是一个按钮");
        jFrame.add(jButton);


        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

}

```
       
## 1.4. Swing布局管理器
1. FlowLayout流式布局
使用此种布局方式会使所有的组件像流水一样依次进行排列
      
代码示例：     
      
```java
package com.java.chap13.sec04;

import javax.swing.*;
import java.awt.*;

/**
 * FlowLayout流式布局
 * @author Yan
 * @date 2019/7/27 16:26
 */
public class FlowLayout {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("FlowLayout测试");

        //jFrame.setLayout(new java.awt.FlowLayout());
        //jFrame.setLayout(new java.awt.FlowLayout(java.awt.FlowLayout.LEFT));
        jFrame.setLayout(new java.awt.FlowLayout(java.awt.FlowLayout.LEFT,15,15));
        JButton jButton=null;
        for(int i=0;i<9;i++){
            jButton=new JButton("JButton"+i);
            jFrame.add(jButton);
        }

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
        
2. BorderLayout
使用此种布局方式将一个窗体的版面划分成东、西、南、北、中5个区域，可以直接将需要的组件放到这5个区域中
       
代码示例：     
       
```java
package com.java.chap13.sec04;

import javax.swing.*;
import java.awt.*;

/**
 *
 * @author Yan
 * @date 2019/7/28 15:35
 */
public class BorderLayoutTest {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("BorderLayout测试");


        //jFrame.setLayout(new BorderLayout());
        jFrame.setLayout(new BorderLayout(5,5));
        jFrame.add(new JButton("东"),BorderLayout.EAST);
        jFrame.add(new JButton("西"),BorderLayout.WEST);
        jFrame.add(new JButton("南"),BorderLayout.SOUTH);
        jFrame.add(new JButton("北"),BorderLayout.NORTH);
        jFrame.add(new JButton("中"),BorderLayout.CENTER);


        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
         
3. GridLayout表格布局
使用此种布局是以表格的形式进行布局管理的，在使用此布局管理器时必须设置显示的行数和列数
       
代码示例：      
       
```java
package com.java.chap13.sec04;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 15:39
 */
public class GridLayoutTest {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("GridLayout测试");


        jFrame.setLayout(new GridLayout(4,5,5,5));
        JButton jButton=null;
        for (int i=0;i<19;i++){
            jButton=new JButton("JButton"+i);
            jFrame.add(jButton);
        }


        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
       
4. 绝对布局
       
代码示例：     
      
```java
package com.java.chap13.sec04;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 15:44
 */
public class AbsoluteLayoutTest {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("绝对布局测试");

        jFrame.setLayout(null);
        JButton jButton1=new JButton("按钮1");
        JButton jButton2=new JButton("按钮2");
        jFrame.add(jButton1);
        jFrame.add(jButton2);
        jButton1.setBounds(50,10,100,20);
        jButton2.setBounds(70,40,200,30);


        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
          
## 1.5. JLable 组件
     
代码示例：    
      
```java
package com.java.chap13.sec05;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 16:02
 */
public class JLableTest {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("JLable测试");

        JLabel jLabel=new JLabel("JLable组件",JLabel.CENTER);
        jFrame.add(jLabel);
        

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
       
## 1.6. 文本框组件
1. JTextField 文本框
      
代码示例：     
      
```java
package com.java.chap13.sec06;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 16:17
 */
public class JTextFiledTest {
    public static void main(String[] args) {
        JFrame jFrame=new JFrame("JTextFiled单行文本框测试");

        jFrame.setLayout(new GridLayout(1,2,10,10));
        JLabel jLabel=new JLabel("用户名：");
        JTextField jTextField=new JTextField();
        jFrame.add(jLabel);
        jFrame.add(jTextField);

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300,200);   //设置容器的位置
        jFrame.setSize(500,500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
       
2. JPasswordField 密码框
      
代码示例：      
     
```java
package com.java.chap13.sec06;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 16:42
 */
public class JPasswordFiledTest {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("JPasswordFiled密码框测试");

        jFrame.setLayout(new GridLayout(2, 2, 10, 10));
        JLabel jLabel = new JLabel("用户名：");
        JTextField jTextField = new JTextField();
        JLabel jLabe2 = new JLabel("密码：");
        JPasswordField jPasswordField=new JPasswordField();
        jFrame.add(jLabel);
        jFrame.add(jTextField);
        jFrame.add(jLabe2);
        jFrame.add(jPasswordField);

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300, 200);   //设置容器的位置
        jFrame.setSize(500, 500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
       
3. JTextArea 文本域
       
代码示例：     
       
```java
package com.java.chap13.sec06;

import javax.swing.*;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 16:17
 */
public class JTextAreaTest {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("JTextArea文本域测试");

        jFrame.setLayout(new GridLayout(1, 2, 10, 10));
        JLabel jLabel = new JLabel("描述：");
        JTextArea jTextArea = new JTextArea();
        jFrame.add(jLabel);
        jFrame.add(jTextArea);

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300, 200);   //设置容器的位置
        jFrame.setSize(500, 500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
        
## 1.7. JPanel 轻量级容器
代码示例：     
      
```java
package com.java.chap13.sec07;

import javax.swing.*;
import javax.swing.border.EmptyBorder;
import java.awt.*;

/**
 * @author Yan
 * @date 2019/7/28 16:50
 */
public class JPanelTest {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("JPanel面板测试");

        JPanel jPanel=new JPanel();
        jPanel.setLayout(new GridLayout(3,2,10,10));
        jFrame.add(jPanel);
        jPanel.setBorder(new EmptyBorder(10,10,10,10));   //设置边距


        JLabel jLabel = new JLabel("用户名：");
        JTextField jTextField = new JTextField();
        JLabel jLabe2 = new JLabel("密码：");
        JPasswordField jPasswordField = new JPasswordField();
        JButton jButton=new JButton("登录");
        JButton jButton2=new JButton("重置");

        jPanel.add(jLabel);
        jPanel.add(jTextField);
        jPanel.add(jLabe2);
        jPanel.add(jPasswordField);
        jPanel.add(jButton);
        jPanel.add(jButton2);

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300, 200);   //设置容器的位置
        jFrame.setSize(500, 500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
       
## 1.8. Swing事件处理
      
代码示例：     
      
**demo1**
        
```java
package com.java.chap13.sec08;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

/**
 * @author Yan
 * @date 2019/7/28 22:11
 */

class JButtonListener implements ActionListener{
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println(e.getActionCommand());
        JOptionPane.showMessageDialog(null,"我被点击了");
    }
}

public class EventTest1 {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("Swing事件");
        JButton jButton=new JButton("我是一个按钮");

        JButtonListener jButtonListener=new JButtonListener();
        jButton.addActionListener(jButtonListener);  //添加/注册事件监听

        jFrame.add(jButton);


        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300, 200);   //设置容器的位置
        jFrame.setSize(500, 500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
       
**demo2**
       
```java
package com.java.chap13.sec08;

import javax.swing.*;
import java.awt.*;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

/**
 * @author Yan
 * @date 2019/7/28 22:17
 */

//class MyWindowListener implements WindowListener {
//
//    @Override
//    public void windowOpened(WindowEvent e) {
//        System.out.println("窗口被打开");
//    }
//
//    @Override
//    public void windowClosing(WindowEvent e) {
//        System.out.println("窗口关闭");
//    }
//
//    @Override
//    public void windowClosed(WindowEvent e) {
//        System.out.println("窗口被关闭");
//    }
//
//    @Override
//    public void windowIconified(WindowEvent e) {
//        System.out.println("窗口最小化");
//    }
//
//    @Override
//    public void windowDeiconified(WindowEvent e) {
//        System.out.println("窗口从最小化恢复");
//    }
//
//    @Override
//    public void windowActivated(WindowEvent e) {
//        System.out.println("窗口被选中");
//    }
//
//    @Override
//    public void windowDeactivated(WindowEvent e) {
//        System.out.println("窗口选中被取消");
//    }
//}

public class EventTest2 {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("Swing事件");

        //MyWindowListener myWindowListener=new MyWindowListener();
        //jFrame.addWindowListener(myWindowListener);

        jFrame.addWindowListener(new WindowListener() {
            @Override
            public void windowOpened(WindowEvent e) {
                System.out.println("窗口被打开");
            }

            @Override
            public void windowClosing(WindowEvent e) {
                System.out.println("窗口关闭");
            }

            @Override
            public void windowClosed(WindowEvent e) {
                System.out.println("窗口被关闭");
            }

            @Override
            public void windowIconified(WindowEvent e) {
                System.out.println("窗口最小化");
            }

            @Override
            public void windowDeiconified(WindowEvent e) {
                System.out.println("窗口从最小化恢复");
            }

            @Override
            public void windowActivated(WindowEvent e) {
                System.out.println("窗口被选中");
            }

            @Override
            public void windowDeactivated(WindowEvent e) {
                System.out.println("窗口选中被取消");
            }
        });

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300, 200);   //设置容器的位置
        jFrame.setSize(500, 500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
         
**demo3**
       
```java
package com.java.chap13.sec08;

import javax.swing.*;
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;


/**
 * @author Yan
 * @date 2019/7/28 22:24
 */

//class MyWindowAdapter extends WindowAdapter{
//    @Override
//    public void windowClosing(WindowEvent e) {
//        super.windowClosing(e);
//        System.out.println("窗口关闭。。。。。。。");
//    }
//}

public class EventTest3 {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("Swing事件");

        //MyWindowAdapter myWindowAdapter=new MyWindowAdapter();
        //jFrame.addWindowListener(myWindowAdapter);


        jFrame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                super.windowClosing(e);
                System.out.println("窗口关闭。。。。");
            }
        });

        jFrame.getContentPane().setBackground(Color.red);   //设置容器的背景颜色
        jFrame.setLocation(300, 200);   //设置容器的位置
        jFrame.setSize(500, 500);    //设置容器大小
        jFrame.setVisible(true);  //让容器显示
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}

```
        
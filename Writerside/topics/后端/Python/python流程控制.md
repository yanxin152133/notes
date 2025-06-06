# python流程控制
## 顺序执行
python代码在执行过程中，遵循的基本原则：
- 普通语句，直接执行
- 碰到函数，将函数体载入内存，并不直接执行
- 碰到类，执行类内部的普通语句，但是类的方法只载入，不执行
- 碰到if、for等控制语句，按相应控制流程执行
- 碰到@、break、continue等，按规定语法执行
- 碰到函数、方法调用等，转而执行函数内部代码，执行完毕继续执行原有顺序代码

其他请见：[顺序执行](https://liujiangblog.com/course/python/26)

## 条件语句
python条件语句是通过一条或多条语句的执行结果（true或者false）来决定执行的代码块。
![python条件语句](https://www.runoob.com/wp-content/uploads/2013/11/if-condition.jpg)

### 基本形式
python程序语言制定任何非0和非空（null）值为true，0或者null为false。
if语句用于控制程序的执行，基本形式为：
```python
if 判断条件：  # if 语句的判断条件可以用>（大于）、<(小于)、==（等于）、>=（大于等于）、<=（小于等于）来表示其关系。
    执行语句……
else：
    执行语句……
```

当判断条件为多个值时，可以使用以下形式:
```python
if 判断条件1:
    执行语句1……
elif 判断条件2:
    执行语句2……
elif 判断条件3:
    执行语句3……
else:
    执行语句4……
```

如果判断需要多个条件需同时判断时，可以使用`or （或）`，表示两个条件有一个成立时判断条件成功；使用`and （与）`时，表示只有两个条件同时成立的情况下，判断条件才成功。

当if有多个条件时可使用括号来区分判断的先后顺序，括号中的判断优先执行，此外 and 和 or 的优先级低于>（大于）、<（小于）等判断符号，即大于和小于在没有括号的情况下会比与或要优先判断。

### 基本原则
条件判断的使用原则：
- 每个条件后面要使用冒号（:）作为判断行的结尾，表示接下来是满足条件（结果为true）后要执行的语句块
- 除了if分支必须有，elif和else分支都可以根据情况省略
- 使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块
- 顺序判断每一个分支，任何一个分支首先被命中并执行，则其后面的所有分支被忽略，直接跳过！
- 在Python中没有switch-case语句

## 循环语句
![python循环语句](https://www.runoob.com/wp-content/uploads/2015/12/loop.png)

### while 循环
while 语句用于循环执行程序，即在某条件下，循环执行某段程序，以处理需要重复处理的相同任务。其基本形式为：
```python
while 判断条件(condition)：
    执行语句(statements)……
```
执行语句可以是当个语句或语句块。判断条件可以是任何表达式，任何非零、或非空的值均为true。

当判断条件为假false时，循环结束。

执行流程图为：    
![python while循环](https://www.runoob.com/wp-content/uploads/2013/11/886A6E10-58F1-4A9B-8640-02DBEFF0EF9A.jpg)

### for 循环
Python for循环可以遍历任何序列的项目，如一个列表或者一个字符串。

for循环的语法格式如下：
```python
for iterating_var in sequence:
   statements(s)
```

流程图：    
![python for循环](https://www.runoob.com/wp-content/uploads/2013/11/A71EC47E-BC53-4923-8F88-B027937EE2FF.jpg)

### 嵌套循环
Python 语言允许在一个循环体里面嵌入另一个循环。

Python for 循环嵌套语法：
```python
for iterating_var in sequence:
   for iterating_var in sequence:
      statements(s)
   statements(s)
Python while 循环嵌套语法：
```
```python
while expression:
   while expression:
      statement(s)
   statement(s)
```

### 循环控制语句
循环控制语句可以更改语句执行的顺序。

#### break 语句
Python break语句，就像在C语言中，打破了最小封闭for或while循环。

break语句用来终止循环语句，即循环条件没有False条件或者序列还没被完全递归完，也会停止执行循环语句。

break语句用在while和for循环中。

如果您使用嵌套循环，break语句将停止执行最深层的循环，并开始执行下一行代码。

流程图：    
![python break语句](https://www.runoob.com/wp-content/uploads/2014/09/E5A591EF-6515-4BCB-AEAA-A97ABEFC5D7D.jpg)

#### continue 语句
Python continue 语句跳出本次循环，而break跳出整个循环。

continue 语句用来告诉Python跳过当前循环的剩余语句，然后继续进行下一轮循环。

continue语句用在while和for循环中。

流程图：    
![python continue语句](https://www.runoob.com/wp-content/uploads/2014/09/8962A4F1-B78C-4877-B328-903366EA1470.jpg)

#### pass 语句
Python pass 是空语句，是为了保持程序结构的完整性。

pass 不做任何事情，一般用做占位语句。
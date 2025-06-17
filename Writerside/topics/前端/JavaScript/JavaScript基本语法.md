# JavaScript基本语法

## 语句
- JavaScript程序的执行单位为行（line），也就是一行一行地执行。一般情况下，每一行就是一个语句。
- 语句（statement）是为了完成某种任务而进行的操作。
- 语句以分号结尾，一个分号就表示一个语句结束。多个语句可以写在一行内。
- 分号前面可以没有任何内容，JavaScript引擎将其视为空语句。
- 表达式不需要分号结尾。一旦在表达式后面添加分号，则JavaScript引擎就将表达式视为语句，这样会产生一些没有任何意义的语句。
ß
如：
```javascript
var a = 1 + 3;

```

## 变量
- 变量是对“值”的具名引用。变量就是为“值”起名，然后引用这个名字，就等同于引用这个值。变量的名字就是变量名。
- JavaScript的变量名是区分大小写，A和a是两个不同的变量。
- 变量的声明和赋值是分开的两个步骤。如果只是声明变量而没有赋值，则该变量的值是undefined。undefined是一个特殊的值，表示“无定义”。
- 变量赋值时未写var命令，则这条语句也是有效的。建议总是使用var命令声明变量。如果一个变量没有声明就直接使用，JavaScript会报错，告诉变量未定义。
- 可以在同一条var命令中声明多个变量。
- JavaScript是一种动态类型语言，变量的类型没有限制，变量可以随时更改类型。

## 标识符
- 标识符（identifier）指的是用来识别各种值的合法名称。最常见的标识符就是变量名，以及函数名。
- JavaScript语言的标识符对大小写敏感。
- 中文是合法的标识符，可以用作变量名。

JavaScript 有一些保留字，不能用作标识符：arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield。

### 标识符命名规则
标识符有一套命名规则，不符合规则的就是非法标识符。标识符命名规则如下：
- 第一个字符，可以是任意Unicode字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）
- 第二个字符及后面的字符，除了Unicode字母、美元符号和下划线，还可以用数字0-9

## 注释
## 区块
## 条件语句
### if
### if...else
### switch
### 三元运算符
## 循环语句
### while 循环
### for 循环
### do...while 循环
### break语句和continue语句
### 标签（label）
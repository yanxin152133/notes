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

JavaScript
有一些保留字，不能用作标识符：arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield。

### 标识符命名规则

标识符有一套命名规则，不符合规则的就是非法标识符。标识符命名规则如下：

- 第一个字符，可以是任意Unicode字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）
- 第二个字符及后面的字符，除了Unicode字母、美元符号和下划线，还可以用数字0-9

## 注释

### 单行注释

 ```javascript
// 这是单行注释


```

### 多行注释

```javascript
/*
 这是
 多行
 注释
*/
```

## 区块

- JavaScript使用大括号，将多个相关的语句组合在一起，称为“区块”（block）。
- 对于var命令来说，JavaScript的区块不构成单独的作用域（scope）。

## 条件语句

JavaScript提供if结构和switch结构，完成条件判断，即只有满足预设的条件，才会执行相应的语句。

### if 结构

if结构先判断一个表达式的布尔值，然后根据布尔值的真伪，执行不同的语句。所谓布尔值，指的是 JavaScript
的两个特殊值，true表示“真”，false表示“伪”。

```javascript
if (布尔值)
    语句;

// 或者
if (布尔值) 语句;
```

### if...else 结构

if代码块后面，还可以跟一个else代码块，表示不满足条件时，所要执行的代码。

```javascript
if (m === 3) {
    // 满足条件时，执行的语句
} else {
    // 不满足条件时，执行的语句
}

```

### switch 结构

多个if...else连在一起使用的时候，可以转为使用更方便的switch结构。

```javascript
switch (fruit) {
    case "banana":
        // ...
        break;
    case "apple":
        // ...
        break;
    default:
    // ...
}
```

### 三元运算符

三元运算符（即该运算符需要三个运算子） `?:`，也可以用于逻辑判断。

```javascript
(条件) ? 表达式1 : 表达式2
```

如果“条件”为true，则返回“表达式1”的值，否则返回“表达式2”的值。

## 循环语句

循环语句用于重复执行某个操作。

### while 循环

while语句包括一个循环条件和一段代码块，只要条件为真，就不断循环执行代码块。

```javascript
while (条件)
    语句;

// 或者
while (条件) 语句;
```

### for 循环

for语句是循环命令的另一种形式，可以指定循环的起点、终点和终止条件。

```javascript
for (初始化表达式; 条件; 递增表达式)
    语句

// 或者

for (初始化表达式; 条件; 递增表达式) {
    语句
}
```

for语句后面的括号里面，有三个表达式。

- 初始化表达式（initialize）：确定循环变量的初始值，只在循环开始时执行一次。
- 条件表达式（test）：每轮循环开始时，都要执行这个条件表达式，只有值为真，才继续进行循环。
- 递增表达式（increment）：每轮循环的最后一个操作，通常用来递增循环变量。

### do...while 循环

do...while循环与while循环类似，唯一的区别就是先运行一次循环体，然后判断循环条件。

```javascript
do
    语句
while (条件);

// 或者
do {
    语句
} while (条件);
```

### break语句和continue语句

#### break

break语句和continue语句都具有跳转作用，可以让代码不按既有的顺序执行。

break语句用于跳出代码块或循环。

```javascript
var i = 0;

while (i < 100) {
    console.log('i 当前为：' + i);
    i++;
    if (i === 10) break;
}
```

#### continue

continue语句用于立即终止本轮循环，返回循环结构的头部，开始下一轮循环。

```javascript
var i = 0;

while (i < 100) {
    i++;
    if (i % 2 === 0) continue;
    console.log('i 当前为：' + i);
}
```

### 标签（label）

- JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置。
- 标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。
- 标签通常与break语句和continue语句配合使用，跳出特定的循环。

```javascript
label:
    语句
```
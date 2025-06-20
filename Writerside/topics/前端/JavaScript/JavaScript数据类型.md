# JavaScript数据类型
JavaScript的数据类型共有六种。
- 数值（number）：整数和小数（比如1和3.14）
- 字符串（string）：文本（比如hello world）
- 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）
- undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值。
- null：表示空值，即此处的值为空。
- 对象（object）：各种值组成的集合。

## null和undefined
二者区别：null是一个表示“空”的对象，转为数值时为0；undefined是一个表示“此处无定义”的原始值，转为数值时为NaN。

```javascript
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```

## 布尔值
布尔值代表“真”和“假”两个状态。“真”用关键字true表示，“假”用关键字false表示。布尔值只有这两个值。

下列运算符会返回布尔值：
- 前置逻辑运算符： ! (Not)
- 相等运算符：===，!==，==，!=
- 比较运算符：>，>=，<，<=

转换规则：除了下面六个值被转为false，其他值都视为true。
- undefined
- null
- false
- 0
- NaN
- ""或''（空字符串）

如：

```javascript
if ('') {
  console.log('true');
}
// 没有任何输出

if ([]) {
    console.log('true');
}
// true

if ({}) {
    console.log('true');
}
// true
```


## 数值

## 字符串

## 对象

## 函数

## 数组
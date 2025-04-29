# TypeScript 变量声明
## TypeScript 命名规则
命名规则如下：
- 变量名称可以包含数字和字母
- 除了下划线`_`和美元符号`$`外，不能包含其他特殊字符，包括空格。
- 变量名不能以数字开头

变量使用前必须先声明，可以使用var来声明变量。

```html
var [变量名] : [类型] = 值;

var [变量名] : [类型];

var [变量名] = 值;

var [变量名];
```

## 类型断言（Type Assertion）
类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。语法格式为：

```typescript
<类型>值

值 as 类型
```

## 类型推断
当类型没有给出时，TypeScript 编译器利用类型推断来推断类型。

如果由于缺乏声明而不能推断出类型，那么它的类型被视作默认的动态 any 类型。

如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查：

```typescript
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

## 变量作用域
变量作用域指定了变量定义的位置。程序中变量的可用性由变量作用域决定，其作用域如下：
- 全局作用域：全局变量定义在程序结构的外部，它可以在你代码的任何位置使用
- 类作用域：这个变量也可以称为字段。类变量声明在一个类里头，但在类的方法外面。该变量可以通过类的对象来访问。类变量可以是静态的，静态的变量可以通过类名直接访问，
- 局部作用域：局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用

```typescript
var global_num = 12          // 全局变量
class Numbers { 
   num_val = 13;             // 实例变量
   static sval = 10;         // 静态变量
   
   storeNum():void { 
      var local_num = 14;    // 局部变量
   } 
} 
console.log("全局变量为: "+global_num)  
console.log(Numbers.sval)   // 静态变量
var obj = new Numbers(); 
console.log("实例变量: "+obj.num_val)
```
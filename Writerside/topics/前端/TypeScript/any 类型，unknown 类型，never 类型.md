# any 类型，unknown 类型，never 类型

## any类型

any 类型表示没有任何限制，该类型的变量可以赋予任意类型的值。

```typescript
let x: any;

x = 1; // 正确
x = 'foo'; // 正确
x = true; // 正确
```

any类型主要适用以下两个场合。

- 出于特殊原因，需要关闭某些变量的类型检查，就可以把该变量的类型设为any。
- 为了适配以前老的 JavaScript 项目，让代码快速迁移到 TypeScript，可以把变量类型设为any。有些年代很久的大型 JavaScript
  项目，尤其是别人的代码，很难为每一行适配正确的类型，这时你为那些类型复杂的变量加上any，TypeScript 编译时就不会报错。

## unknown类型

为了解决any类型“污染”其他变量的问题，TypeScript 3.0
引入了unknown类型。它与any含义相同，表示类型不确定，可能是任意类型，但是它的使用有一些限制，不像any那样自由，可以视为严格版的any。

unknown类型跟any类型的不同之处在于，它不能直接使用。主要有以下几个限制。

- unknown类型的变量，不能直接赋值给其他类型的变量（除了any类型和unknown类型）。
- 不能直接调用unknown类型变量的方法和属性。
- unknown类型变量能够进行的运算是有限的，只能进行比较运算（运算符==、===、!=、!==、||、&&、?）、取反运算（运算符!
  ）、typeof运算符和instanceof运算符这几种，其他运算都会报错。

```typescript
let x: unknown;

x = true; // 正确
x = 42; // 正确
x = 'Hello World'; // 正确
```

## never类型

为了保持与集合论的对应关系，以及类型运算的完整性，TypeScript
还引入了“空类型”的概念，即该类型为空，不包含任何值。由于不存在任何属于“空类型”的值，所以该类型被称为never，即不可能有这样的值。

```typescript
let x: never;
```


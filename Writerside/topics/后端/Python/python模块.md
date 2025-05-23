# python模块
模块（Moudule）是一个python文件，以.py结尾，包含了python对象定义和python语句。    
模块能让你能够有逻辑地组织你的python代码段。    
把相关的代码分配到一个模块里能让你的代码更好用，更易懂。    
模块能定义函数、类和变量，模块里也能包含可执行的代码。

## import 语句
使用`import`语句来引入模块；语法如下：
```python
import module1[, module2[,... moduleN]]
```

## from...import 语句
`from`语句能从模块中导入一个指定的部分到当前命名空间中，语法如下：
```python
from modname import name1[, name2[, ... nameN]]
```

## from...import* 语句
把一个模块中的所有内容全都导入到当前的命名空间。语法如下：
```python
from modname import *
```

## 搜索路径
当导入一个模块，python 解析器对模块位置的搜索顺序是：
1. 当前目录
2. 如果不在当前目录，python 则搜索在shell变量`PYTHONPATH`下的每个目录
3. 如果都找不到，python 会查看默认路径。UNIX下，默认路径一般为`/usr/local/lib/python`

## 命名空间和作用域
变量是拥有匹配对象的名字（标识符）。命名空间是一个包含了变量名称们（键）和它们各自相应的对象们（值）的字典。    
一个python 表达式可以访问局部命名空间和全局命名空间里的变量。如果一个局部变量和一个全局变量重名，则局部变量会覆盖全局变量。    
每个函数都有自己的命名空间。类的方法的作用域规则和通常函数一样。     
python 会智能地猜测一个变量是局部地还是全局的，它假设任何在函数内赋值地变量都是局部的。    
因此，如果要给函数内的全局变量赋值，必须使用`global`语句。

## 包
包是一个分层次的文件目录结构，它定义了一个由模块及子包，和子包下的子包等组成的python的应用环境。目录结构如下：
```html
test.py
package_runoob
|-- __init__.py
|-- runoob1.py
|-- runoob2.py
```
# 环境
- Linux/Ubuntu18.04
- docker
- mysql 5.6
       
# 代码仓库
- [GitHub](https://github.com/yanxin152133/PHP)
        
# 视频地址
参考：
- 链接: https://pan.baidu.com/s/1oxqVH8_GH94bhbpmQ4b-8Q 提取码: wsfd 
        
<!--more-->
           
# 数据库概念
>数据库（Database）是按照数据结构来阻止、存储和管理数据的建立再计算机存储设备上的仓库。
       
数据库:存储数据的仓库。
         
## 数据库分类
### 网络数据库
>网络数据库是指把数据库技术引入到计算机网络系统中，借助于网络技术将存储与数据库中的大量信息及时发布出去；而计算机网络借助于成熟的数据库技术对网络中的各种数据进行有效管理，并实现用户与网络中的数据库进行实时动态数据交互。
        
### 层级数据库
>层次结构模型实质上是一种有根结点的定向有序树（在数学中“树”被定义为一个无回的连通图）。
   
### 关系数据库
>关系数据库，是建立在关系模型基础上的数据库，借助于几何代数等数学概念和方法来处理数据库中的数据。
      
数据库的另一种区分方式：基于存储介质。
      
存储介质分为两种：磁盘和内存。
       
关系型数据库：存储在磁盘中。
       
非关系型数据库：存储在内存中。
        
## 关系型数据库
### 基本概念
>关系数据库，是建立在关系模型基础上的数据库，借助于几何代数等数学概念和方法来处理数据库中的数据。现实世界中的各种实体以及实体之间的各种练习均用关系模型来表示。关系模型是由埃德加·科德于1970年首先提出的，并配合“科德十二定律”。现如今虽然对此模型有一些批评意见，但它还是数据存储的传统标准。关系模型由关系数据结构、关系操作集合、关系完整性约束三部分组成。
       
- 关系数据结构：指的是数据以什么方式来存储，是一种二维表的形式存储。
    - 本质：二维表
- 关系操作集合:如何来关联和管理对应的存储数据，SQL指令
- 关系完整性约束：数据内部有对应的关联关系，以及数据与数据之间也有对应的关联关系。
    - 表内约束：对应的具体列只能放对应的数据（不能乱放）
    - 表间约束：自然界各实体都是有着对应的关联关系（外键）

        
### 典型关系型数据库
- 小型关系型数据库Microsoft Access、SQLite
- 中型关系型数据库：Microsoft SQL Server、MySQL
- 大型关系型数据库：Oracle、DB2
       
# SQL介绍
## SQL基本介绍
>结构化查询语言（Structured Query Language）简称SQL，是一种特殊目的的编程语言，是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统；同时也是数据库脚本文件的扩展名。
      
SQL就是专门为关系型数据库而设计出来的。
       
## SQL分类
1. 数据查询语言（DQL：Data Query Language）:
其语句，也称为“数据检索语句”，用以从表中获取数据，确定数据怎样在应用程序给出。保留字SELETE是DQL（也是所有SQL）用的最多的动词i，其它DQL常用的保留字有WHERE，ORDER BY，GROUP BY和HAVING。这些DQL保留字常与其他类型的SQL语句一起使用。
**专门用于查询数据：代表指令为selete/show**
        
2. 数据操作语言（DML：Data Manipulation Language）:
其语句包括动词INSERT，UPDATE和DELETE。它们分别用于添加、修改和删除表中的行。也称为动作查询语言。
**专门用于写数据：代表指令：insert,update和delete**
          
3. 事务处理语言（TPL）：
它的语句能确保被DML语句影响的表的所有行即使得以更新。TPL语句包括BEGIN、TRANSACTIOIN、COMMIT和ROLLBACK。（不是所有的关系型数据库都提供事务安全处理）
**专门用于事务安全处理：transation**
         
4. 数据控制语言（DCL）：
它的语句通过GRANT和REVOKE获取许可，确定单个用户和用户组对数据库对象的访问。某些RDBMS可用GRANT或REVOKE控制对表单个列的访问。
**专门用于权限管理：代表指令为grant和revoke**
        
5. 数据定义语言（DDL）：
其语句包括动词CREATE和DROP。在数据库中创建新表或删除表（CREATE TABLE或DROP TABLE）；为表加入索引等。DDL包括许多与数据库目录中获得数据有关的保留字。它也是动作查询的一部分。
**专门用于结构管理：代表指令create和drop(alter)**
        
## MySQL基本介绍
>MySQL是一个关系型数据库关系系统，由瑞典MySQL AB公司开发，目前属于Oracle旗下产品。MySQL是最流行的关系型数据库管理系统之一，在WEB应用方面，MySQL是最好的RDBMS（Relational Database Management System,关系数据库管理系统）应用软件。
       
### 启动和停止MySQL
>MySQL是一种C/S结构：客户端和服务端。
        
- 命令行方式
通过Windows下打开cmd控制器，然后使用命令进行管理。
    - net start 服务（mysql）：开启服务
    - net stop mysql：关闭服务
- 系统服务方式
前提：在安装的MySQL的时候将MySQL添加到Windows的服务中去。
            
### 登录和退出MySQL
通过客户端与服务器进行连接认证，就可以进行操作。通常服务端与客户端不在同一台电脑上。
        
- 登录
1. 找到相关程序
2. 输入对应的服务器地址
3. 输入服务器中MySQL监听的端口
4. 输入用户名：-u:username
5. 输入密码：-p:password
         
**注意事项**
1. 通常端口都可以默认，MySQL监听的端口通常是3306
2. 密码的输入可以先输入-p，直接换行，然后再以密文方式输入密码
         
- 退出
断开与服务器的连接：通常MySQL提供的服务器数量有限，一旦客户端用完，建议就应该断开连接。     
建议方式：使用SQL提供的指令
    - `exit;`
    - `\q`
    - `quit`
        
## MySQL服务端架构
>MySQL服务端架构有以下几层构成：
>1. 数据库管理系统（最外层）：DBMS，专门管理服务器端的所有内容
>2. 数据库（第二层）：DB，专门用于存储数据的仓库（可以有很多个）
>3. 二维数据表（第三层）：Table，专门用于存储具体实体的数据
>4. 字段（第四层）：Field，具体存储某种类型的数据（实际存储单元）

数据库中常用的几个关键字：
- row：行
- Column：列（field）
         
# 数据库基本操作
>数据库是数据存储的最外层（最大单元）

## 库操作        
### 创建数据库
基本语法：
        
```sql
create database 数据库名字 [库选项];

## 示例代码
create database mydatabase;
```
        
库选项：数据库的相关属性。
- 字符集：charset字符集，代表着当前数据库下的所有表存储的数据默认指定的字符集（如果当前不指定，那么采用DBMS默认的）。
- 校对集：collate校对集
           
基本语法：
            
```sql
create database 数据库名字 charset 字符集名称;

## 示例代码
create database mydatabase2 charset gbk;
```
        
### 显示数据库
>每当用户通过SQL指令创建一个数据库，那么系统就会产生一个对应的存储数据的文件夹（data）
           
- 显示全部
基本语法：
      
```sql
show databases;
```
          
![](https://live.staticflickr.com/65535/48622298426_9149c94e79_b.jpg)
             
- 显示部分
基本语法：
      
```sql
show databases like '匹配模式';

_：匹配当前位置单个字符
%：匹配指定位置多个字符

获取以my开头的全部数据库：'my%';
获取m开头，后面第一个字母不确定，最后为database的数据库：'m_database';
获取以database结尾的数据库：'%database';


## 示例代码
show databases like 'my%';
```
         
- 显示数据库创建语句
基本语法：
        
```sql
show create database 数据库名字

## 示例代码
show create database mydatabase;
```
             
### 选择数据库
>为什么要选择数据库？
因为数据是存储到数据表，表存在数据库下。如果要操作数据，那么必须进入到对应的数据库才行
            
基本语法：
        
```sql
use 数据库名字;

## 示例代码
use mydatabase;
```
        
### 修改数据库
>修改数据库字符集（库选项）：字符集和校对集。在5.5之前是可以通过`rename`命令进行修改数据库名字，但是之后的版本是不可以的。
         
基本语法：
       
```sql
alter database 数据库名字 charset = 字符集;

## 示例代码
alter database mydatabase charset gbk;
```
         
### 删除数据库
基本语法：
       
```sql
drop database 数据库名字

## 示例代码
drop database mydatabase;
show databases;
```
        
**删除虽简单，但是切记要做好安全操作：确保里面数据没有问题。**
       
删除数据库之后，对应的存储数据的文件也会被删除。
       
## 表操作
### 创建数据表
- 普通创建表
基本语法：
        
```sql
create table 表名(字段名 字段类型[字段属性],...) [表选项];

- 表选项：与数据库选项类似。
    - engine：存储引擎，MySQL提供的具体存储数据的方式，默认有一个innodb（5.5以前默认是myisam）
    - charse：字符集，只对当前自己表有效（级别比数据库高）
    - collate：校对集
```
                       
   

```sql   
## 示例代码
# 创建数据表
use mydatabase2;
create table class
(
    name varchar(10)
);

# 创建数据表
create table mydatabase2.class2
(
    name varchar(10)
);

# 使用表选项
create table mydatabase2.student
(
    name varchar(10)
) charset utf8;
```
       
- 复制已有表结构
从已经存在的表复制一份（只复制结构，如果表中有数据不复制）         
基本语法：

```sql
create table 表明 like 表名;   //只要使用数据库.表名，就可以在任何数据库下访问其他数据库的表名

## 示例代码
# 在test数据库下创建一个与student一样的表
use test;
create table student like mydatabase2.student;
show tables;
```
        

### 显示数据表
- 显示所有表
基本语法：
       
```sql
show tables;
```
        
- 匹配显示表
基本语法：
       
```sql
show tables like '匹配模式'
```
              
### 显示表结构
本质含义：显示表中所包含的字段信息（名字，类型，属性等）
       
- describe 表名
- desc 表名
- show columns from 表名
      
```sql
## 示例代码
# 显示表结构
use mydatabase2;
describe class;

desc student;

show columns from student;
```
        
### 显示表创建语句
查看数据表创建时的语句：此语句看到的结果已经不是用户之前自己输入的。
       
基本语法：
       
```sql
show create table 表名;
```
       
**MySQL中有多种语句结束符**
- `;`与`\g`所表示的效果是一样的，都是字段在上排横着，下面跟对应的数据
- `\G`字段在左侧竖着，数据在右侧横着
          
### 设置表属性
表属性指的就是表选项：engine，charset和collate。
      
基本语法：
        
```sql
alter table 表名 表选项 [=] 值;

## 示例代码
alter table mydatabase2.student charset gbk;

show create table mydatabase2.student;
```
       
**注意**：如果数据库已经确定了，里面有很多数据了，不要轻易修改表选项（字符集影响不大）
        
### 修改表结构
- 修改表名
基本语法：
       
```sql
rename table 旧表名 to 新表名;
```
        
- 修改表选项
基本语法：
       
```sql
alter table 表名 表选项 [=] 新值;
```
        
- 新增字段
基本语法：
       
```sql
alter table 表名 add [column] 新字段名 列类型 [列属性] [位置first/after 字段名];
```
                    
**字段位置**：字段想要存放的位置。
1. First：在某某之前（最前面），第一个字段
2. after 字段名：放在某个具体的字段之后
           

- 修改字段名
基本语法：
       
```sql
alter table 表名 change 旧字段名 新字段名 字段类型 [列属性] [新位置];
```
        
- 修改字段类型（属性）
基本语法：
       
```sql
alter table 表名 modify 字段名 新类型 [新属性] [新位置];
```
        
- 删除字段
基本语法：
       
```sql
alter table 表名 drop 字段名;
```
        
```sql
## 示例代码
# 数据库中数据表名字通常有前缀：取数据库的前两个字母加上下划线
rename table mydatabase2.student to my_student;
show tables;

# 给学生表增加age字段
alter table my_student add column age int;
desc my_student;
# 增加字段：放在第一个字段
alter table my_student add id int first;

# 修改字段名
alter table my_student change age nj int;

# 修改字段类型
alter table my_student modify name varchar(20);

# 删除字段
alter table my_student drop nj;
```
          
### 删除表结构
基本语法：
       
```sql
drop table 表名[,表名2]...;   //可以同时删除多个数据表
```
        
```sql
## 示例代码
# 删除表名
drop table class;
show tables;
drop table class2,my_student;
```
         
## 数据操作
### 插入操作
本质含义：将数据以SQL的形式存储到指定的数据表（字段）里面
         
基本语法：
       
```sql
insert into 表名 [(字段列表)] values (对应字段列表);

```
        
```sql
# 插入数据到数据表
create table my_teacher
(
    name varchar(10),
    age  int
);

insert into my_teacher (name, age) values ('Jaca', 30);
insert into my_teacher (age, name) values (40, 'Tom');
insert into my_teacher (name) values ('Li');

insert into my_teacher values ('Zhang',22);
```
        
### 查询操作
- 查询表中全部数据
基本语法：
       
```sql
select * from 表名;   //*表示匹配所有的字段
```
         
- 查询表中部分字段
基本语法：
       
```sql
select 字段列表 from 表名;   //字段列表使用逗号隔开
```
         
- 简单条件查询数据
基本语法：
       
```sql
select 字段列表/* from 表名 where 字段名=值;
```
         
```sql
# 获取所有数据
select * from my_teacher;

# 获取指定字段数据
select name from my_teacher;

# 获取年龄为30岁的人的名字
select name from my_teacher where age=30;
```
         
### 删除操作
基本语法：
        
```sql
delete from 表名 [where 条件];  //如果没有where条件：意味着系统会自动删除该表所有数据

## 示例代码
# 删除年龄为30岁的人
delete from my_teacher where age = 30;
```
           
### 更新操作
更新：将数据进行修改（通常是修改部分字段数据）
           
基本语法：
       
```sql
update 表名 set 字段名=新值 [where 条件];     //如果没有where条件，那么所有的表中对应的那个字段都会被修改成同一值

## 示例代码
# 更新年龄
update my_teacher set age=28 where name='Li';
```
        
## 字符集
### 字符编码概念
>字符（character）是各种文字和符号的总称，包括各国家文字、标点符号、图形符号、数据等。
>字符编码（character code）是计算机针对各种符号，在计算机中的一种二进制存储代号。
        
### 字符集概念
>字符集（character set）是多个字符的集合，字符集种类较多，每个字符集包含的字符个数不同。
       
常见字符集名称：ASCII字符集，GB2312字符集，BIG5字符集，GB18030字符集，Unicode字符集等。计算机要准确的处理各种字符集文字，需要进行字符集编码，以便计算机能够识别和存储各种文字。中文文字数目大，而且还分为简体中文和繁体中文两种不同书写规则的文字，而计算机最初是按英文单字节字符设计的，因此，对中文字符进行编码，是中文信息交流的技术基础。
            
### 设置客户端所有字符集
快捷方式：
        
```sql
set names 字符集;


# 查看系统保存的三种关系处理字符集
show variables like 'character_set%';
```
      
# 列类型
## 整数类型
### tinyint
>迷你整型：系统采用一个字节来保存整型，一个字节=8位，最大能表示的数值的0-255
### smallint
>小整型：系统采用两个字节来保存的整形：能表示0-65535之间
### mediumint
>中整型：系统采用三个字节保存数据
### int
>整型（标准整型）：采用四个字节保存数据
### bigint
>大整型：采用八个字节来保存数据
       
```sql
-- 示例代码
# 创建数据表
create table my_int
(
    int_1 tinyint,
    int_2 smallint,
    int_3 mediumint,
    int_4 int,
    int_5 bigint
);

# 插入数据
insert into my_int values (10,10000,100000,1000000,1000000000);

# 插入错误数据（超出对应的数据范围）
insert into my_int values (255,255,255,255,255);
# 上面的错误原因
insert into my_int values (-128,255,255,255,255);
insert into my_int values (127,255,255,255,255);

```
              
### 无符号表示设定
>无符号，表示存储的数据在当前字段中，没有负数（只有正数，区间为0-255）
        
基本语法：在类型之后加上一个`unsigned`
        
```sql
# 无符号表示设定
alter table my_int add int_6 tinyint unsigned first;
insert into my_int values (255,100,255,255,255,255);
insert into my_int values (-100,100,255,255,255,255);
```
        
## 显示长度
>显示长度：指数据（整型）在数据显示的时候，到底可以显示多长位。如tinyint(3),表示最长可以显示3位，显示长度只是代表了数据是否可以达到指定的长度，但是不会自动满足到指定长度：如果想要数据显示的时候，保持最高位（显示长度），那么还需要给字段增加一个`zerofill`属性才可以。同时显示长度可以自己设定，超出长度（但是不超出范围）不会影响，只会对不够长度的进行补充（显示长度）。
       
```sql
-- 示例代码
# 显示长度
alter table my_int add int_7 tinyint zerofill first;
insert into my_int values (1,1,1,1,1,1,1);

alter table my_int add int_8 tinyint(2) zerofill first;
insert into my_int values (100,1,1,1,1,1,1,1);
insert into my_int values (1,1,1,1,1,1,1,1);
```
       
## 小数类型
>专门用来存储小数的。在MySQL中将小数类型又分为两类：浮点型和定点型。
      
### 浮点型
>浮点型又称之为精度类型，是一种有可能丢失精度的数据类型，数据有可能不那么准确（尤其是在超出范围的时候）。浮点型之所以能够存储较大的数值（不精确），原因就是利用存储数据的位来存储指数。
         
#### float
>float又称之为单精度类型，系统提供4个字节用来存储数据，但是能表示的数据范围比整型大得多，大概是10^38。只能保证大概7个左右的精度（如果数据在7位数以内，那么基本是准确的，但是如果超过7位数，那么就是不准确的）。
        
基本语法
float：表示不指定小数位的浮点数
float(M,D)：表示一共存储M个有效数字，其中小数部分占D位
float(10,2)：整数部分为8位，小数部分为2位
       
```sql
-- 示例代码
# 创建表
create table my_float(
    f1 float,
    f2 float(10,2)
);

# 插入数据
insert into my_float values (123.123,123456789.90);
insert into my_float values (123.1234567,123456789.00);
insert into my_float values (123.1234567,99999999.99);  # 用户不能插入数据直接超过指定的整数部分长度，但是如果是系统自动进位导致的，系统可以承担
insert into my_float values (123.123,10e5);   # 浮点数可以采用科学计数法来存储数据
```

浮点数的应用：通常是用来保存一些数量特别大的，大到可以不用那么精确的数据。
         
#### double
>double又称之为双精度，系统用8个字节来存储数据，表示的范围更大，10^308次方，但是精度也只有15位左右。

### 定点数
>定点数：能够保证数据精确的小数（小数部分可能不精确，超出长度会四舍五入），整数部分一定精确。
          
#### decimal
>decimal定点数：系统自动根据存储的数据来分配存储空间，每大概9个数就会分配四个字节进行存储，同时小数和整数部分是分开的。
        
基本语法：       
decimal(M,D)：M表示在那个长度，最大值不能超过65，D表示小数部分长度，最长不能超过30。
        
```sql
-- 示例代码
# 创建表
create table my_decimal(
    f1 float(10,2),
    f2 decimal(10,2)
);

# 插入数据
insert into my_decimal values (12345678.90,12345678.90);
insert into my_decimal values (99999999.99,99999999.99);
insert into my_decimal values (99999999.99,99999999.999);  # 定点数如果整数部分进位超出长度也会报错
```
        
定点数的应用：如果涉及到钱的时候有可能使用定点数。
       
## 时间日期型
### date
>日期类型：系统使用三个字节来存储数据，对应的格式位YYYY-mm-dd，能表示的范围是从1000-01-01到9999-12-12，初始值位0000-00-00
        
### time
>时间类型：能够表示某个指定的时间，但是系统同样是提供三个字节来存储，对应的格式为HH:ii:ss，但是MySQL中time类型能够表示时间范围要大得多，能表示从-838:59:59-838:59:59，在MySQL中具体的用处是用来描述时间段。
      
### datetime
>日期时间类型：就是将前面的date和time合并起来，表示的时间，使用8个字节存储数据，格式为YYYY-mm-dd HH:ii:ss，能表示的区间1000-01-01 00:00:00到9999-12-12 23:59:59，其可以为0值：0000-00-00 00:00:00。
        
### timestamp
>时间戳类型：MySQL中的时间戳只是表示从格林威治事件开始，但是其格式依然是YYYY-mm-dd HH:ii:ss。
       
### year
>年类型：占用一个字节来保存，能表示1900-2188年，但是year有两种数据插入方式：0-99和四位数的具体年。
          
```sql
-- 示例代码
# 创建表
create table my_date(
    d1 date,
    d2 time,
    d3 datetime,
    d4 timestamp,
    d5 year
);

# 插入数据
insert into my_date values ('1900-01-01','12:12:12','1900-01-01 12:12:12','1999-01-01 12:12:12',69);
# year的特殊性：可以采用两位数的数据插入，也可以四位数的年份插入
insert into my_date values ('1900-01-01','12:12:12','1900-01-01 12:12:12','1999-01-01 12:12:12',2020);
# year进行两位数插入的时候，有一个区间划分，零界点为69和70，当输入69以下，那么系统时间为20+数字，如果是70以上。那么系统时间为19+数字
insert into my_date values ('1900-01-01','12:12:12','1900-01-01 12:12:12','1999-01-01 12:12:12',70);
# timestamp
update my_date set d1='2000-01-01' where d5=2069;
# time类型的特殊性，本质是用来表示时间区间，能表示的范围比较大
insert into my_date values ('1900-01-01','512:12:12','1900-01-01 12:12:12','1999-01-01 12:12:12',70);
# 在进行时间类型录入的时候（time）还可以使用一个简单的日期代替时间，在时间格式之前加一个空格，然后指定一个数字（可以是负数），系统会自动将该数字转换成天数*24小时，再加上后面的时间
insert into my_date values ('1900-01-01','5 12:12:12','1900-01-01 12:12:12','1999-01-01 12:12:12',70);
```
         
```sql
mysql> desc my_date;
+-------+-----------+------+-----+-------------------+-----------------------------+
| Field | Type      | Null | Key | Default           | Extra                       |
+-------+-----------+------+-----+-------------------+-----------------------------+
| d1    | date      | YES  |     | NULL              |                             |
| d2    | time      | YES  |     | NULL              |                             |
| d3    | datetime  | YES  |     | NULL              |                             |
| d4    | timestamp | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| d5    | year(4)   | YES  |     | NULL              |                             |
+-------+-----------+------+-----+-------------------+-----------------------------+
5 rows in set (0.00 sec)

## d4一行  时间戳类型不能为空，有默认值，为当前时间戳对应的时间，当数据被更新的时候，这个字段更新为当前最新的时间。

```
           
## 字符串型
### char
>定长字符：指定长度之后，系统一定会分配指定的空间用于存储数据。
           
基本语法：char(L):L代表字符数（中文与英文字母一样），L长度为0到255
          
### varchar
>变长字符：指定长度之后，系统会根据实际存储的数据来计算长度，分配合适的长度（数据没有超出长度）
          
基本语法：varchar(L):L代表字符数，L的长度理论值为0到65535
         
因为varchar要记录数据长度（系统根据数据长度自动分配空间），所以每个varchar数据产生后，系统都会在数据后面增加1-2个字节的额外开销，是用来保存数据所占用的空间长度，如果数据本身小于127个字节，额外开销一个字节；如果大于127个，就开销两个字节。

char和varchar数据存储对比（utf8，一个字符都会占用3个字节）
          
|存储数据|char(2)|varchar(2)|char所占字节|varchar所占字节|
|--|--|--|--|--|
|A|A|A|2x3=6|1x3+1=4|
|AB|AB|AB|2x3=6|2x3+1=7|      
        
char和varchar的区别       
1. char一定会使用指定的空间，varchar是根据数据来定空间
2. char的数据查询效率比varchar高，varchar是需要通过后面的记录数来计算
        
如果确定数据一定是占指定长度，那么使用char类型        
如果不确定数据到底有多少，那么使用varchar类型       
如果数据长度超过255个字符，不论是否固定长度，都会使用text，不再使用char和varchar。
            
### text
>文本类型：本质上MySQL提供了两种文本类型
text：存储普通的字符文本
blob：存储二进制文本（图片，文件），一般都不会使用blob来存储文件本身，通常是使用一个链接来指向对应的文件本身。
          
text系统中提供的四种text
- tinytext:系统使用一个字节来保存，实际能够存储的数据为2^8+1
- **text**:使用两个字节保存，实际存储：2^16+2
- mediumtext:使用三个字节保存，实际存储：2^24+3
- longtext:使用四个字节保存，实际存储为2^32+4
       
**注意**：
1. 在选择对应的存储文本的时候，不用刻意去选择text类型，系统会自动根据存储的数据长度来选择合适的文本类型。
2. 在选择字符存储的时候，如果数据超过255个字符，那么一定选择text存储
          
### enum
>枚举类型：在数据插入之前，先设定几个项，这几个项就是可能最终出现的数据结果。
       
如果确定某个字段的数据只有那么几个值：如性别，男，女，保密，系统就可以在设定字段的时候规定当前字段只能存放固定的几个值，使用枚举。
           
基本语法：enum(数据值1，数据值2,...)
系统提供了1到2个字节来存储枚举数据：通过计算enum列举的具体值来选择实际的存储空间，如果数据值列表在255个以内，那么一个字节就够；如果超过255但是小于65535，那么系统采用两个字节保存。
                
枚举的意义：
1. 规范数据本身，限定只能插入规定的数据项
2. 节省存储空间
           
```sql
-- 示例代码
# 创建表
create table my_enum(
    gender enum('男','女','保密')
);

desc my_enum;

# 插入数据
insert into my_enum values ('男');
insert into my_enum values ('女');
insert into my_enum values ('男');

# enum字段存储的结果是数值
# 将字段按照数值输出
select gender + 0 from my_enum;

insert into my_enum values (3);
```
        
### set
>集合：是一种将多个数据选项可以同时保存的数据类型，本质是将指定的项按照对应的二进制位来进行控制：1表示该选项被选中，0表示该选项没有被选中
        
基本语法：set('值1','值2','值3',...)     
       
系统为set提供了多个字节进行保存，但是系统会自动计算来选择具体的存储单元。
       
- 1个字节：set只能由8个选项
- 2个字节：set只能有16个选项
- 3个字节：set只能有24个选项
- 8个字节：set可以表示64个选项
          
set和enum一样，最终存储到数据字段中的依然是数字而不是真实的字符串。
          
```sql
# 创建表
create table my_set(
    hobby set('篮球','足球','羽毛球','乒乓球','网球','橄榄球','冰球','跳高')
);

# 插入数据
insert into my_set values ('篮球,乒乓球,足球');
insert into my_set values ('橄榄球,跳高,篮球,乒乓球,足球');   # 排序顺序为创建表时的顺序

# 查看数据:以数值方式查看
select hobby + 0 from my_set;

insert into my_set values (255);
```
        
set集合的意义：
1. 规范数据
2. 节省存储空间
       
- enum:单选框
- set:复选框
       
## MySQL记录长度
在MySQL中，有一项规定，MySQL的记录长度（record==行row）总长度不能超过65535个字节。        
varchar能够存储的理论值为65535个字符，字符在不同的字符集下不可能占用多个字节。
        
```sql
# 创建表(证明varchar在MySQL中能够达到的理论值   utf8和gbk)
create table my_varchar(
    name varchar(65535)
)charset utf8;

# 计算在utf8和gbk下对应的varchar能够存储的长度
# utf8 65535/3=21845 如果采用varchar存储，需要两个额外的字节来保存长度
# gbk 65535/2=32767 | 1 如果采用varchar存储，需要额外的2个字节

create table my_utf(
    name varchar(21844)
)charset utf8;

create table my_gbk(
    name varchar(32766)
)charset gbk;
```
          
# 列属性
>列属性又称之为字段属性，在MySQL中一共有6个属性：**null，默认值，列描述，主键，唯一键，自动增长**。
         
## null属性
>NULL属性：代表字段为空
           
```sql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| name  | varchar(10) | YES  |     | NULL    |       |
| age   | int(11)     | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```
         
如果对应的值为**YES**表示该字段可以为NULL。
         
**注意**
1. 在设计表的时候，尽量不要让数据为空。
2. MySQL的记录长度为65535个字节，如果一个表中有字段允许为NULL，那么系统就会设计保留一个字节来存储NULL，最终有效存储长度为65534个字节。
               
## 默认值
>default：默认值，当字段被设计的时候，如果允许默认条件下，用户不进行数据的插入，那么就可以使用事先准备好的数据来填充，通常填充的是NULL。
        
```sql
# 创建表
create table my_default(
    name varchar(10) NOT NULL,   -- 不能为空
    age int default 18    -- 表示，如果当前字段在进行数据插入的时候没有提供，那么就使用18
);

# 测试插入数据
insert into my_default(name) values('Tom');

# default关键字的另外一层使用：显示的告知字段使用默认值，在进行数据插入的时候，对字段值直接使用default
insert into my_default values ('Jack',default);
```
          
## 列描述
>列描述：comment，是专门用于开发人员进行维护的一个注释说明。
      
基本语法：comment '字段描述';
        
```sql
# 创建表，增加字段描述
create table my_comment(
    name varchar(10) not null comment '当前是用户名，不能为空。',
    pass varchar(50) not null comment '密码不能为空'
);

# 查看效果(查看表创建语句)
show create table my_comment;
```
       
## 主键
>顾名思义，主要的键，primarykey，在一张表中，有且只有一个字段，里面的值具有唯一性。
       
### 创建主键
#### 随表主键
系统提供了两种增加主键的方式
1. 方案1：直接在需要当作主键的字段之后，增加primarykey属性来确定主键
2. 方案2：在所有字段之后增加primarykey选项：primarykey(字段信息)
       
#### 表后增加
基本语法：`alter table 表名 add primary key(字段);`
         
### 查看主键
方案1：查看表结构。`desc 表名`    
方案2：查看表的创建语句。`show create table 表名`
       
### 删除主键
基本语法：`alter table 表名 drop primary key`
         
```sql
# 随表主键
# 方案1
# 创建表，在字段后增加主键属性
create table my_pri1(
    username varchar(10) primary key
);

# 方案2
create table my_pri2(
    username varchar(10),
    primary key(username)
);

# 表后增加
create table my_pri3(
    username varchar(10)
);

# 增加主键
alter table my_pri3 add primary key(username);

# 删除主键
alter table my_pri3 drop primary key;
```
        
### 复合主键
案例：有一张学生选修课表，一个学生可以选修多个选修课，一个选修课也可以由多个学生来选，但是一个学生在一个选修课中只有一个成绩。
       
```sql
# 创建表
create table my_score(
    student_no char(10),
    course_no char(10),
    score tinyint not null,
    primary key (student_no,course_no)
);
```
          
### 主键约束
主键一旦增加，那么对对应的字段有数据要求。       
1. 当前字段对应的数据不能为空
2. 当前字段对应的数据不能有任何重复
       
```sql
-- 以上面的my_score表为例
# 创建表
create table my_score(
    student_no char(10),
    course_no char(10),
    score tinyint not null,
    primary key (student_no,course_no)
);

# 主键约束
insert into my_score values ('000000001','course001','100');
insert into my_score values ('000000002','course001','90');
insert into my_score values ('000000001','course002','95');
insert into my_score values ('000000002','course001','95');
```
        
## 主键分类
>主键分类采用的是主键所对应的字段的业务意义分类。
>业务主键：主键所在的字段具有业务意义（学生ID，课程ID）
>逻辑主键：自然增长的整型（应用广泛）
       
## 自动增长
>自动增长：auto_increment，当给定某个字段该属性之后，该列的数据在没有提供确定数据的时候，系统会根据之前已经存在的数据进行自动增加后，填充数据。
       
通常自动增长用于逻辑主键。
           
### 原理
自动增长的原理：
1. 在系统中有维护一组数据，用来保存当前使用了自动增长属性的字段，记住当前对应的数据值，再给定一个指定的步长。
2. 当用户进行数据插入的时候，如果没有给定值，系统在原始值上再加上步长变成新的数据。
3. 自动增长的触发，给定属性的字段没有提供值。
4. 自动只适用于数值。
       
### 使用自动增长
基本语法：在字段之后增加一个属性`auto_increment`
        
```sql
# 创建一个带自动增长的表
create table my_auto(
    id int primary key auto_increment,
    name varchar(10) not null comment '用户名',
    pass varchar(50) not null comment '密码'
);

# 插入数据，触发自动增长，不能给定具体值
insert into my_auto values (null,'Tom','123456');
```
           
### 修改自动增长
1. 查看自增长：自增长一旦触发使用之后，会自动的在表选项中增加一个选项（一张表最多只能拥有一个自增长）。
        
```sql
-- 实例
+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table                                                                                                                                                                                                                                                                                                           |
+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| my_auto | CREATE TABLE `my_auto` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(10) COLLATE utf8_unicode_ci NOT NULL COMMENT '用户名',
  `pass` varchar(50) COLLATE utf8_unicode_ci NOT NULL COMMENT '密码',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci      |
+---------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```
        
2. 表选项可以通过修改表结构来实现。
        
```sql
alter table 表名 auto_increment = 值;
```
        
### 删除自动增长
删除自增长，就是在字段属性之后不再保留auto_increment，当用户修改自增长所在字段时，如果没有看到auto_increment属性，系统送会自动清除该自增长。
        
```sql
-- 实例代码
alter table my_auto modify id int;
# 切记不要再次增加primary key
```
          
### 初始设置
>在系统中有一组变量维护自增长的初始值和步长
        
```sql
show variables like 'auto_increment%';
```
        
```sql
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| auto_increment_increment | 1     |
| auto_increment_offset    | 1     |
+--------------------------+-------+
2 rows in set (0.00 sec)


-- auto_increment_increment 步长
-- auto_increment_offset 初始值
```
         
### 细节问题
1. 一张表只有一个自增长，自增长会上升到表选项中
2. 如果数据插入中没有触发自增长（给定了数据），那么自增长不会表现
3. 自增长在修改的时候，值可以较大，但是不能比当前已有的自增长的字段值小
        
## 唯一键
>唯一键：unique key，用来保证对应的字段中的数据唯一的。
>主键也可以用来保证字段数据唯一性，但是一张表只有一个主键
>1. 唯一键在一张表中可以有多个
>2. 唯一键允许字段数据为NULL，NULL可以有多个（NULL不参与比较）
         
### 创建唯一键
创建唯一键与创建主键非常类似。        
1. 直接在表字段之后增加唯一键标识符：unique[key]
2. 在所有的字段之后使用unique key(字段列表)
3. 在创建完表之后也可以增加唯一键。`alter table 表名 add unique key(字段列表);`
        
```sql
# 创建表，唯一键
create table my_unique1(
    id int primary key auto_increment,
    usernmae varchar(10) unique
);

create table my_unique2(
    id int primary key auto_increment,
    usernmae varchar(10),
    unique key (usernmae)
);

create table my_unique3(
   id int primary key auto_increment,
   usernmae varchar(10)
);
alter table my_unique3 add unique key (usernmae);
```
       
### 查看唯一键
>唯一键是属性，可以通过查看表结构来实现。
        
```sql
mysql> desc my_unique1;
+----------+-------------+------+-----+---------+----------------+
| Field    | Type        | Null | Key | Default | Extra          |
+----------+-------------+------+-----+---------+----------------+
| id       | int(11)     | NO   | PRI | NULL    | auto_increment |
| usernmae | varchar(10) | YES  | UNI | NULL    |                |
+----------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```
      
唯一键的效果：在不为空的情况下不允许重复
          
```sql
-- 实例
# 唯一键效果
insert into my_unique1 values (null,default);
insert into my_unique1 values (null,default);
insert into my_unique1 values (null,default);

insert into my_unique1 values (null,'army');
insert into my_unique1 values (null,'army');  # 非空不允许重复
```
         
在查看表创建语句的时候，会看到与主键不同的一点，多出一个“名字”
       
```sql
+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table      | Create Table                                                                                                                                                                                                                                                                |
+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| my_unique1 | CREATE TABLE `my_unique1` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `usernmae` varchar(10) COLLATE utf8_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `usernmae` (`usernmae`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci |
+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

-- UNIQUE KEY `usernmae` (`usernmae`)  中的username是系统为唯一键自动创建一个名字（默认是字段名）
```
        
### 删除唯一键
一个表中允许存在多个唯一键，删除的基本语法与主键不一样。
        
删除唯一键的基本语法：
           
```sql
alter table 表名 drop index 唯一键名字（上面UNIQUE KEY `usernmae` (`usernmae`)  的username）
```
                  
index关键字：索引，唯一键是索引的一种（提升查询效率）
        
**修改唯一键：先删除后增加**
        
### 复合唯一键
>唯一键与主键一样可以使用多个字段来共同保证唯一性。一般主键都是单一字段（逻辑主键），而其他需要唯一性的内容都是由唯一键来处理。
         
# 表关系
>表关系：表与表之间（实体）有什么样的关系，每种关系应该如何设计表结构。
        
## 一对一
>一对一：一张表中的一条记录与另外一张表中最多有一条明确的关系，通常，此设计方案保证两张表中使用同样的主键即可。

**示例**
              
学生表
        
|学生ID（主键）|姓名|年龄|性别|籍贯|婚否|住址|
|--|--|--|--|--|--|--|
|||||||||
        
表的使用过程中，常用的信息会经常去查询，而不常用的信息会偶尔才会用到
         
解决方案：将两张表拆分，常见放一张表，不常见的放一张表。
         
常见表
       
|学生ID（主键）|姓名|年龄|性别|
|--|--|--|--|
||||||
             
不常用表
          
|学生ID（主键）|籍贯|婚否|住址|
|--|--|--|--|
||||||
        
## 一对多
>一对多，通常也叫做多对一的关系。通常一对多的关系设计的方案，在“多”关系的表中去维护一个字段，这个字段是“一”关系的主键。
         
**实例**
       
母亲表

|母亲ID（主键）|姓名|年龄|身高|
|--|--|--|--|
|M1||||
|M2|||||
         
孩子表
        
|孩子ID（主键）|姓名|年龄|身高|母亲ID|
|--|--|--|--|--|
|K1||||M1|
|K2||||M2|
          
## 多对多
>多对多，一张表中的一条记录在另外一张表中可以匹配到多条记录，反过来也一样。
>多对多的关系如果按照多对一的关系维护，就会出现一个字段中有多个其他表的主键，在访问的时候就会带来不便。
>既然通过两张表自己增加字段解决不了问题，那么就通过第三张表来解决 
        
**实例**
         
师生关系
1. 一个老师交过多个班级的学生
2. 一个学生听过多个老师讲的课
          
解决方案：
       
![](https://live.staticflickr.com/65535/48677396686_07c2cd09d6_b.jpg)
        
多对多的解决方案：增加一个中间表，让中间表与对应的其他表形成两个多对一的关系，多对一的解决方案是在“多”表中增加“一”表对应的主键字段。
        
# 高级数据操作
## 新增数据
### 多数据插入
>只要写一次insert指令，但是可以直接插入多条记录。
            
基本语法：
      
```sql
insert into 表名 [(字段列表)] values(值列表),(值列表)...;
```
        
```sql
-- 示例代码
# 创建表(多数据插入)
create table my_insert(
    name varchar(10)
);

# 多数据插入
insert into my_insert values ('张三'),('李四'),('王五');

```
        
### 主键冲突
>主键冲突：在有的表中，使用的是业务主键（字段有业务含义），但是往往在数据插入的时候，又不确定数据表中是否已经存在对应的主键。
         
主键冲突的解决方案：         
1. 主键冲突更新：类似插入数据语法，如果插入的过程中主键冲突，那么采用更新方法。 
         
```sql
insert into 表名 [(字段列表)] values (值列表) on duplicate key update 字段 = 新值;
```
       
2. 主键冲突替换：当主键冲突之后，干掉原来的数据，重新插入进去。
        
```sql
replace into 表名 values [(字段列表)](值列表);
```
          
```sql
# 创建表（主键冲突）
create table my_student(
    stu_id varchar(10) primary key comment '主键，学生ID',
    stu_name varchar(10) not null comment '学生姓名'
);
# 插入数据
insert into my_student
values ('stu0001', '张三'),
       ('stu0002', '张四'),
       ('stu0003', '张五'),
       ('stu0004', '张六');

insert into my_student values ('stu0004','小明');   # 结果无法插入

# 冲突更新
insert into my_student values ('stu0004','小明') on duplicate key update stu_name ='小明';

replace into my_student values ('stu0001','小张');
```
         
### 蠕虫复制
>蠕虫复制：一分为二，成倍的增加。从已有的数据中获取数据，并且将获取到的数据插入到数据表中。
      
基本语法：
        
```sql
insert into 表名 [(字段列表)] select */字段列表 from 表;
```
        
```sql
# 创建表（蠕虫复制）
create table my_simple(
    name char(1) not null
);

# 插入数据
insert into my_simple values ('a'),('b'),('c'),('d');

# 蠕虫复制
insert into my_simple (name) select name from my_simple;
```
         
**注意**
1. 蠕虫复制的确通常是重复数据，没有太大也无意义，可以在短期内快速增加表的数据量，从而可以测试表的压力，还可以通过大量数据来测试表的效率（索引）
2. 蠕虫复制虽好，但是要注意主键冲突
        
## 更新数据
1. 在更新数据的时候，特别注意，通常一定是跟随条件更新。
        
```sql
update 表名 set 字段名 = 新值 where 判断条件;
```
         
2. 如果没有条件，是全表更新数据。但是可以使用limit来显示更新的数量
       
```sql
update 表名 set 字段名 = 新值 [where 判断条件] limit 数量;

-- 示例代码
# 更新数据
update my_simple set name='e' where name='a' limit 2;
```
         
## 删除数据
1. 删除数据的时候尽量不要全部删除，应该使用where进行判定。
2. 删除数据的时候可以使用limit来限制要删除的具体数量
       
**delete删除数据的时候无法重置auto_increment**
       
MySQL有一个能够重置表选项中的自增长的语法。
        
```sql
truncate 表名;
```
         
```sql
-- 示例代码
select * from my_auto;
# 删除整个表的数据
delete from my_auto;

# 插入数据
insert into my_auto values (null,'ceshi','123456');  # auto_increment是无法进行重置的

# 重置表选项中的自增长
truncate my_auto;
# 插入数据
insert into my_auto values (null,'ceshi','123456');  # auto_increment是无法进行重置的
```
          
## 查询数据
>完整的查询指令：`select select选项 字段列表 from 数据源 where 条件 group by 分组 having 条件 order by 排序 limit 限制;`

- select选项：系统该如何对待查询得到的数据
    - all：默认的，表示保存所有的记录
    - distinct：去重，去除重复的记录，只保留一条（所有的字段都相同）
         
```sql
# select选项
select all * from my_simple;  -- select *

select distinct * from my_simple;
```
         
- 字段列表：有的时候需要从多张表获取数据，在获取数据的时候，可能存在不同表中有同名的字段，需要将同名的字段命名成不同名的（别名）。基本语法如下：
       
```sql
字段名 [as] 别名
```
       
```sql
# 字段别名
select distinct name as name1,name name2 from my_simple;
```
             
### from数据源
>from是为前面的查询提供数据，数据源只要是一个符合二维表结构的数据即可。
         
#### 单表数据
`from 表名`
       
#### 多表数据
从多张表中获取数据，基本语法：`from 表1，表2...`    

```sql
select * from my_int,my_set;
```
         
上面的结果为：
         
```sql
+-------+-------+-------+-------+-------+--------+---------+------------+------------------------------------------------------------------+
| int_8 | int_7 | int_6 | int_1 | int_2 | int_3  | int_4   | int_5      | hobby                                                            |
+-------+-------+-------+-------+-------+--------+---------+------------+------------------------------------------------------------------+
|  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 | 篮球,足球,乒乓球                                                 |
|  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
|  NULL |  NULL |  NULL |  -128 |   255 |    255 |     255 |        255 | 篮球,足球,乒乓球                                                 |
|  NULL |  NULL |  NULL |  -128 |   255 |    255 |     255 |        255 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|  NULL |  NULL |  NULL |  -128 |   255 |    255 |     255 |        255 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
|  NULL |  NULL |  NULL |   127 |   255 |    255 |     255 |        255 | 篮球,足球,乒乓球                                                 |
|  NULL |  NULL |  NULL |   127 |   255 |    255 |     255 |        255 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|  NULL |  NULL |  NULL |   127 |   255 |    255 |     255 |        255 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
|  NULL |  NULL |   255 |   100 |   255 |    255 |     255 |        255 | 篮球,足球,乒乓球                                                 |
|  NULL |  NULL |   255 |   100 |   255 |    255 |     255 |        255 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|  NULL |  NULL |   255 |   100 |   255 |    255 |     255 |        255 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
|  NULL |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,乒乓球                                                 |
|  NULL |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|  NULL |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
|   100 |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,乒乓球                                                 |
|   100 |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|   100 |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
|    01 |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,乒乓球                                                 |
|    01 |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,乒乓球,橄榄球,跳高                                     |
|    01 |   001 |     1 |     1 |     1 |      1 |       1 |          1 | 篮球,足球,羽毛球,乒乓球,网球,橄榄球,冰球,跳高                    |
+-------+-------+-------+-------+-------+--------+---------+------------+------------------------------------------------------------------+
21 rows in set (0.00 sec)
```
          
结果：两张表的记录数相乘，字段数拼接。       
本质：从第一张表去除一条记录。去拼凑第二张表中的所有记录，保留所有记录。      
在数学上有一个专业的说法：笛卡尔积，这个结果除了给数据库造成压力，没有其他意义。应该避免出现笛卡尔积。
         
#### 动态数据
from后面跟的数据不是一个实体表，而是一个从表中查询出来得到的二维结果表（子查询）。
         
基本语法：`from (select 字段列表 from 表) as 别名;`
        
```sql

# 动态数据
select * from (select int_1,int_8 from my_int) as int_my;
```
         
### where字句
>where子句：用来从数据表获取数据的时候，然后进行条件筛选。
>数据获取原理：针对表去对应的磁盘出获取所有的记录（一条条），where的作用就是在拿到一条结果就开始进行判断，判断是否符合条件，如果符合就保存下来，如过上不符合直接舍弃（不放到内存中）
>where是通过运算符进行结果比较来判断数据。        
    
### group by 子句
>group by表示分组的含义，根据指定的字段，将数据进行分组，分组的目标是为了统计。
      
#### 分组统计
基本语法：`group by 字段名`

```sql
# 分组统计
alter table my_student add class_id int;
# 插入数据处理
# 进行分组
select * from my_student group by class_id;  -- 根据班级id进行分组
```
         
**group by 是为了分组后进行数据统计，如果只是想看数据显示，那么group by没有什么含义，group by将数据按照指定的字段分组之后，只会保留每组的第一条记录。**
           
利用一些统计函数：       
- `count()`：统计每组中的数量，如果统计的目标是字段，那么不统计为空NULL字段，如果为*，那么代表统计记录。
- `avg()`：求平均值
- `sum()`：求和
- `max()`：求最大值
- `min()`：求最小值      
- `group_concat()`：为了将分组中指定的字段进行合并（字符串拼接）
                  
```sql
# 使用聚合函数
alter table my_student add stu_age tinyint unsigned;
alter table my_student add stu_height tinyint unsigned;
# 自行处理插入数据
# 按照班级统计每班人数，最大年龄，最矮身高，平均年龄
select class_id,count(*),max(stu_age),min(stu_height),avg(stu_age) from my_student group by class_id;
select class_id,group_concat(stu_name),count(*),max(stu_age),min(stu_height),avg(stu_age) from my_student group by class_id;
```
         
结果为：
        
```sql
+----------+------------------------+----------+--------------+-----------------+--------------+
| class_id | group_concat(stu_name) | count(*) | max(stu_age) | min(stu_height) | avg(stu_age) |
+----------+------------------------+----------+--------------+-----------------+--------------+
|        1 | 小张,张四              |        2 |           28 |             165 |      23.0000 |
|        2 | 张五,小明              |        2 |           25 |             187 |      23.5000 |
+----------+------------------------+----------+--------------+-----------------+--------------+
2 rows in set (0.00 sec)
```
         
#### 多分组
>将数据按照某个字段进行分组之后，对已经分组的数据进行再次分组。
        
基本语法：`group by 字段1,字段2;  //先按照字段1进行排序，之后将结果在按照字段2进行排序，以此类推`
          
```sql
# 多分组
alter table my_student add gender enum('男','女','保密');
# 插入数据自行处理
# 进行多分组
select class_id,gender,count(*),group_concat(stu_name) from my_student group by class_id, gender;
```
        
上面结果为：
        
```sql
+----------+--------+----------+------------------------+
| class_id | gender | count(*) | group_concat(stu_name) |
+----------+--------+----------+------------------------+
|        1 | 男     |        1 | 小张                   |
|        1 | 女     |        2 | 张四,小猪              |
|        2 | 男     |        2 | 张五,小狗              |
|        2 | 女     |        1 | 小明                   |
+----------+--------+----------+------------------------+
4 rows in set (0.00 sec)
```
        
#### 分组排序
>MySQL中，分钟默认有排序的功能，按照分组字段进行排序，默认是升序。
      
基本语法：`group by 字段 [asc|desc], 字段 [asc|desc]`
              

#### 回溯统计
>当分组进行多分组之后，往上统计的过程中，需要进行层层上报，将这种层层上报统计的过程称之为回溯统计，每一次分组向上统计的过程都会产生一次新的统计数据，而且当前数据对应的分组字段为NULL。
        
基本语法：`group by 字段 [asc|desc] with rollup;`
        
```sql
select class_id,count(*) from my_student group by class_id with rollup;
```
         
结果为：
       
```sql
+----------+----------+
| class_id | count(*) |
+----------+----------+
|        1 |        3 |
|        2 |        3 |
|     NULL |        6 |
+----------+----------+
3 rows in set (0.00 sec)
```
        
```sql
select class_id,gender,count(*) from my_student group by class_id,gender with rollup;
```
        
结果为：
       
```sql
+----------+--------+----------+
| class_id | gender | count(*) |
+----------+--------+----------+
|        1 | 男     |        1 |
|        1 | 女     |        2 |
|        1 | NULL   |        3 |
|        2 | 男     |        2 |
|        2 | 女     |        1 |
|        2 | NULL   |        3 |
|     NULL | NULL   |        6 |
+----------+--------+----------+
7 rows in set (0.00 sec)
```
        
### having子句
>having的本质和where一样，是用来进行数据条件筛选。
>1. having是在group by子句之后，可以针对分组数据进行统计筛选，但是where不行（**having在group by分组之后，可以使用聚合函数或者字段别名，where是从表中取出数据，别名是在数据进入到内存才有的**）
          
```sql
# having子句
# 插入数据自行处理
# 查询班级人数大于等于4个以上的班级
select class_id,count(*) as number from my_student group by class_id having number >= 4;
```
        
结果为：
        
```sql
+----------+--------+
| class_id | number |
+----------+--------+
|        1 |      4 |
+----------+--------+
1 row in set (0.06 sec)
```
         
**强调：having是在group by之后，group by 是在where之后，where的时候表示将数据从磁盘拿到内存，where之后的所有操作都是内存操作。**
           

### order by 子句
>order by排序：根据校对规则对数据进行排序。
        
基本语法：`order by字段 [asc|desc];    //asc升序,默认的`
          
```sql
# order by 子句
# 班级学生按照身高排序
select * from my_student order by stu_height asc ;
```
         
结果为：
        
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
+---------+----------+----------+---------+------------+--------+
7 rows in set (0.00 sec)
```
         
order by 也可以向group by 一样进行多字段排序，先按照第一个字段进行排序，然后再按照第二个字段进行排序。

基本语法：`order by 字段1 规则,字段2 规则;`

多字段排序：

```sql
# 按照班级、身高排序
select * from my_student order by class_id desc ,stu_height;
```

结果为：

```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
+---------+----------+----------+---------+------------+--------+
7 rows in set (0.00 sec)
```
               
### limit 子句
>limit限制子句：主要是用来限制记录数量获取。
       
#### 记录数据限制
>纯粹的限制获取的数量：从第一条到指定的数量
             
基本语法：`limit 数量;`

```sql
# limit 子句
select * from my_student limit 2;
```
        
结果为：
        
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
+---------+----------+----------+---------+------------+--------+
2 rows in set (0.00 sec)
```
       
limit通常在查询的时候如果限定为一条记录的时候，使用的比较多，有时候获取多条记录并不能解决业务问题，但是会增加服务器的压力。
           
#### 分页
>利用limit来限制获取指定区间的数据。
            
基本语法：``limit offset,length;  //offset 偏移量,从哪里开始，length，就是具体 的获取多少条记录`
         
如：MySQL中记录的数量从0开始，`limit 0,2;  //表示获取前两条数据`
           
```sql
mysql> select * from my_student limit 0,2;
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
+---------+----------+----------+---------+------------+--------+
2 rows in set (0.00 sec)

mysql> select * from my_student limit 2,2;
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
+---------+----------+----------+---------+------------+--------+
2 rows in set (0.00 sec)
```
           
**注意：limit后面的length表示最多获取对应数量，但是如果数量不够，系统不会强求**
            
### 聚合函数
参考10.4.3节。     
       
- `count()`：统计每组中的数量，如果统计的目标是字段，那么不统计为空NULL字段，如果为*，那么代表统计记录。
- `avg()`：求平均值
- `sum()`：求和
- `max()`：求最大值
- `min()`：求最小值
           
## 查询中的运算符
### 算数运算符
`+      -       *        /        %`
           
基本算数运算：通常不在条件中使用，而是用于结果运算（select 字段中）
           
```sql
# 创建表
create table ysf1(
    int_1 int,
    int_2 int,
    int_3 int,
    int_4 int
);

insert into ysf1 values (100,-100,0,default);

# 算术运算
select int_1+int_2,int_1-int_2,int_1*int_2,int_1/int_2,int_2/int_3,int_2%2,int_4%5 from ysf1;
```
               
结果为：

```sql
+-------------+-------------+-------------+-------------+-------------+---------+---------+
| int_1+int_2 | int_1-int_2 | int_1*int_2 | int_1/int_2 | int_2/int_3 | int_2%2 | int_4%5 |
+-------------+-------------+-------------+-------------+-------------+---------+---------+
|           0 |         200 |      -10000 |     -1.0000 |        NULL |       0 |    NULL |
+-------------+-------------+-------------+-------------+-------------+---------+---------+
1 row in set (0.00 sec)

```  
### 比较运算符
`>     >=      <     <=      =       <>`
         
通常是用来在条件中进行限定结果。
          
- `=`：在MySQL中没有对应`==`比较符号，就是使用`=`来进行相等判断
- `<=>`：相等比较
         
```sql
# 比较运算符
select * from my_student where stu_age>=20;
```
         
结果为：
         
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
5 rows in set (0.00 sec)
```
           
特殊应用：就是在字段中进行比较运算。

```sql
# MySQL中没有规定select必须有数据表
select '1' <=> 1,0.02 <=> 0,0.02 <> 0;
```
            
结果为：

```sql
+-----------+------------+-----------+
| '1' <=> 1 | 0.02 <=> 0 | 0.02 <> 0 |
+-----------+------------+-----------+
|         1 |          0 |         1 |
+-----------+------------+-----------+
1 row in set (0.00 sec)
```
            
在条件判断的时候，还有对应的比较运算符，计算区间。`between 条件1 and 条件2`。

```sql
# 查找年龄区间
select * from my_student where stu_age between 20 and 30;
```

结果为：    

```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
5 rows in set (0.00 sec)
```
            
between中条件1必须小于条件2，反过来不可以。
          
### 逻辑运算符
`and         or        not`
                     
- `and`：逻辑与
            
```sql
# 逻辑运算符
# 与
select * from my_student where stu_age >= 20 and stu_age <= 30;
```
            
结果为：
         
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
5 rows in set (0.00 sec)
```
            
- `or`：逻辑或
           
```sql
# 或
# 查找身高高于170的或者年龄大于20岁的学生
select * from my_student where stu_height >= 170 or stu_age >= 20;
```
       
结果为：
            
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
7 rows in set (0.00 sec)
```
             
- `not`：逻辑非
                   
### in运算符
>in：在什么里面，是用来替代`=`，当结果不是一个值，而是一个结果集的时候。
                   
基本语法：`in (结果1，结果2，结果3,...)     //只要是当前条件在结果集中出现过，那么就成立`
            
```sql
# in运算符
# 知道学生信息
select * from my_student where stu_id in ('stu0001','stu0004','stu0007');
```
           
结果为：
           
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
3 rows in set (0.08 sec)
```
              
### is运算符
>is是专门用来判断字段是否为null的运算符。
         
基本语法：`is null/is not null`

```sql
# is运算符
# 查询不为空数据
select * from my_int where int_6 is null;
```
          
结果为：
            
```sql
+-------+-------+-------+-------+-------+--------+---------+------------+
| int_8 | int_7 | int_6 | int_1 | int_2 | int_3  | int_4   | int_5      |
+-------+-------+-------+-------+-------+--------+---------+------------+
|  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
|  NULL |  NULL |  NULL |  -128 |   255 |    255 |     255 |        255 |
|  NULL |  NULL |  NULL |   127 |   255 |    255 |     255 |        255 |
+-------+-------+-------+-------+-------+--------+---------+------------+
3 rows in set (0.00 sec)
```
             
### like运算符
>like运算符：是用来进行模糊匹配（匹配字符串）
          
基本语法：`like '匹配模式'`
          
匹配模式中，用两种占位符：
- `_`：匹配对应的单个字符
- `%`：匹配多个字符
              
```sql
# like运算符
# 获取所有姓小的同学
select * from my_student where stu_name like '小%';
select * from my_student where stu_name like '小_';
```
          
结果为：
          
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
5 rows in set (0.00 sec)
```
              
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
5 rows in set (0.00 sec)
```
               
## 联合查询
### 基本概念
>联合查询是可合并多个相似的选择查询的结果集，等同于将一个表追加到另一个表，从而实现将两个表的查询组合到一起，使用为此为UNION或UNION ALL。
         
联合查询：将多个查询的结果合并到一起（纵向合并），字段数不变，多个查询的记录数合并。
             
### 应用场景
1. 将同一张表中不同的结果（需要对应多条的查询语句来实现），合并到一起展示数据。
2. 最常见，在数据量大的情况下，会对表进行分表操作，需要对每张表进行部分数据统计，使用联合查询来将数据存放到一起显示。
       
### 基本语法
基本语法：`select 语句 union [union选项] select 语句`
        
- union选项：与select选项基本一样
- distinct：去重，去掉完全重复的数据（默认的）
- all：保存所有的数据
           
```sql
select * from my_student
union
select * from my_student;
```
          
结果为：
            
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
7 rows in set (0.00 sec)
```
          
```sql
select * from my_student
union all
select * from my_student;
```
       
结果为：
           
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
14 rows in set (0.00 sec)
```
            
**注意细节：union理论上只要保证字段数一样，不要每次拿到的数据对应的字段类型一致.永远只保留第一个select语句对应的字段名字。**
            
```sql
select stu_id,stu_name,stu_height from my_student
union all
select stu_height,stu_name,stu_id from my_student;
```
           
结果为：
            
```sql
+---------+----------+------------+
| stu_id  | stu_name | stu_height |
+---------+----------+------------+
| stu0001 | 小张     | 185        |
| stu0002 | 张四     | 165        |
| stu0003 | 张五     | 187        |
| stu0004 | 小明     | 189        |
| stu0005 | 小猪     | 173        |
| stu0006 | 小狗     | 170        |
| stu0007 | 小江     | 178        |
| 185     | 小张     | stu0001    |
| 165     | 张四     | stu0002    |
| 187     | 张五     | stu0003    |
| 189     | 小明     | stu0004    |
| 173     | 小猪     | stu0005    |
| 170     | 小狗     | stu0006    |
| 178     | 小江     | stu0007    |
+---------+----------+------------+
14 rows in set (0.00 sec)
```
            
### order by的使用
1. 在联合查询中，如果要使用order by，那么对应的select语句必须使用括号括起来
             
```sql
# 获取男生身高升序，女生身高降序
(select * from my_student where gender='男' order by stu_height asc) union (select * from my_student where gender='女' order by stu_height desc) ;
```
          
结果为：
          
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
+---------+----------+----------+---------+------------+--------+
7 rows in set (0.00 sec)
```
       
2. order by在联合查询中若要生效，必须配合使用limit。而limit后面必须跟对应的限制数量（通常可以使用一个较大的之，大于对应表的记录数）
           
```sql
# 获取男生身高升序，女生身高降序
(select * from my_student where gender='男' order by stu_height asc limit 10) 
union 
(select * from my_student where gender='女' order by stu_height desc limit 10) ;
```
             
结果为：
         
```sql
+---------+----------+----------+---------+------------+--------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender |
+---------+----------+----------+---------+------------+--------+
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |
| stu0001 | 小张     |        1 |      18 |        185 | 男     |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |
| stu0007 | 小江     |        1 |      25 |        178 | 女     |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |
+---------+----------+----------+---------+------------+--------+
7 rows in set (0.00 sec)
```

# 连接查询   
## 交叉连接
>交叉连接：将两张表的数据与另外一张表彼此交叉
        
### 原理
1. 从第一张表依次取出每一条记录
2. 取出每一条记录之后，与另外一张表的全部记录挨个匹配
3. 没有任何匹配条件，所有的结果都会进行保留
4. 记录数=第一张表记录数*第二张表记录数，字段数=第一张表字段数+第二张表字段数（笛卡尔积）
        
### 语法
基本语法：`表1 cross join 表2`

```sql
select * from my_student cross join my_int;
```
         
结果为：
         
```
+---------+----------+----------+---------+------------+--------+-------+-------+-------+-------+-------+--------+---------+------------+
| stu_id  | stu_name | class_id | stu_age | stu_height | gender | int_8 | int_7 | int_6 | int_1 | int_2 | int_3  | int_4   | int_5      |
+---------+----------+----------+---------+------------+--------+-------+-------+-------+-------+-------+--------+---------+------------+
| stu0001 | 小张     |        1 |      18 |        185 | 男     |  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
| stu0002 | 张四     |        1 |      28 |        165 | 女     |  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
| stu0003 | 张五     |        2 |      22 |        187 | 男     |  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
| stu0004 | 小明     |        2 |      25 |        189 | 女     |  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
| stu0005 | 小猪     |        1 |      30 |        173 | 女     |  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
| stu0006 | 小狗     |        2 |      18 |        170 | 男     |  NULL |  NULL |  NULL |    10 | 10000 | 100000 | 1000000 | 1000000000 |
.
.
.
.
.
.
```
                 
### 应用
交叉连接产生的结果是笛卡尔积，没有实际应用。
              
## 连接查询
>连接查询：将多张表连到一起进行查询（会导致记录数和字段数列发生改变）
         
### 连接查询的意义
在关系型数据库设计过程中，实体（表）与实体之间是存在很多联系的。在关系型数据库表的设计过程中，遵循这关系来设计：一对一，一对多和多对多，通常在实际操作的过程中，需要利用这层关系来保证数据的完整性。
        
### 连接查询分类
连接查询一共有以下几类：
- 交叉连接
- 内连接
- 外连接：左外连接（左连接）和右外连接（右连接）
- 自然连接
          
## 内连接
>内连接：inner join，从一张表中取出所有的记录去另外一张表中匹配，利用匹配条件进行匹配，成功了则保留，失败则放弃。
        
### 原理
1. 从第一张表中取出一条记录，然后去另外一张表中进行匹配
2. 利用匹配条件进行匹配
2.1. 匹配到，保留，然后继续向下匹配
2.2. 匹配失败，向下匹配，如果全表匹配失败，结束
         
### 语法
基本语法：`表1 [inner] join 表2 on 匹配条件`
           
1. 如果内连接没有条件（可以允许），那么其实就是交叉连接（避免发生）
2. 使用匹配条件进行匹配
3. 因为表的设计通常容易产生同名字段，尤其是ID，所以为了避免重名出现错误，通常使用`表名.字段名`，来确保唯一性
4. 通常如果条件中使用到独赢的表名，而表名通常比较长。所以可以通过表别名来简化
5. 内连接匹配的时候，必须保证匹配到才会保存
6. 内连接因为不强制必须使用匹配条件（on），因此可以在数据匹配完成之后，使用where条件来限制，效果与on一样（建议使用on）
            
```sql
# 内连接
create table my_class(
     id int primary key auto_increment,
     name varchar(10) not null unique
);

# 插入数据
insert into my_class values(null,'1班'),(null,'2班'),(null,'3班');

# 内连接
select * from my_student inner join my_class;
select * from my_student inner join my_class on class_id=id;

select * from my_student inner join my_class on my_student.class_id=my_class.id;

select * from my_student as s inner join my_class as c on s.class_id=c.id;

select * from my_class as c inner join my_student as s on s.class_id=c.id;

select * from my_class as c inner join my_student as s where s.class_id=c.id;
```
              
### 应用
内连接通常是对数据有精确要求的地方使用，必须保证两种表中都能进行数据匹配
         
## 外连接
### 原理
### 语法
### 应用
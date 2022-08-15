# MySQL数据库

## 数据库

* 数据库【按照数据结构来组织来存储和管理数据的仓库】。是一个长期存储在计算机内的有组织的可共享的，统一管理的大量数据的集合。
* 数据对于公司来说是最宝贵的财富，程序员的工作就是对数据进行管理，包括，运算，流转，存储，展示等，数据库最重要的功能【存储数据】，长期保存数据。

## Mysql

* Mysql是一个【关系型数据库管理系统】，瑞典的公司研发的，被Oracle收购。
* MySQL使用了一种语言【SQL语言】。
* MySQL分为社区版和商业版，体积小，速度快，成本低。

## 基本操作

> Mysql保存数据的模式：
>
> 1. 创建一个数据库。
>
> 2. 在数据库下保存多张表。
>
> 3. 在每张表中保存多条数据。

登录mysql

**mysql -h 127.0.0.1 -p3306 -uroot -p**

mysql是一个**数据库管理系统**，可以管理多个数据库。

创建一个数据库：

``` shell
create database 数据库名;
create schema 数据库名;
```

查看所有的数据库：

```shell
show databases;
```

使用数据库：

```shell
use 数据库名;
```

## 表

用来存储数据的对象，是有结构的数据的集合。

* 行：一行即为一条数据，数据库一共有躲闪数据，实际上就是有几行数据。
* 列：一列即为一个字段，数据库一共有多少个字段，实际上就是有几列数据。

# SQL语言

SQL就是一种特殊目的的编程语言，是一种数据库查询和程序设计语言，用于存储数据，以及查询更新和管理关系型数据库系统。

## SQL语句的分类

* DCL：（Data control Language）：数据控制语言，用来定义访问权限和安全级别。
* DDL： (Data Definition Lahguage)︰数据定义语言，用来定义数据库对象︰库，表，字段（列）。功能：创建、删除、修改库和表结构。
* DML：（Data Manipulation Language）：数据操作语言，用来定义数据的，增删改。
* DQL：（Data Query Language）：数据库查询语言，用来查询记录。
* TCL： (Transition Control Language) :事务控制语言，用来管理事务。

### DCL数据控制语言

#### 创建一个用户

创建个用户，该用户只能在指定的ip地址上登录mysql :

```sql
create user 用户名@IP地址 identified by '密码';
```

创建一个用户，该用户可以在任意IP地址上登录mysql

```sql
create user 'moon'@'%' identified by 'root';
```

修改密码：

```sql
-- 5.7版本
set password for moon@'%' =password('新密码')
-- 8.0版本
set password for moon@'%' =('新密码')
```

#### 给用户授权：

给指定用户在指定数据库上赋予指定权限，权限有很多，列举集合常用的；

* 创建数据库：可以创建数据库
* select：查询数据
* delete：删除数据
* update：修改数据
* insert：插入数据

```sql
-- 语法：grant '权限1,权限2,....权限n' on 数据库名.* to 用户名@IP地址 

grant select,insert update,delete,create on `sun`.`user` to `moon`@`%`;
```

> 注意：这里用的是着重符，不是单引号。

#### 撤销授权

```sql
--语法:revoke权限1，....权限n on数据库.* from用户名@IP地址
revoke a1l on `jsoft`.* from ^moon`@`%~;
```

#### 查看授权

查看指定用户名的权限

```sql
--语法: show gr ants for用户名@IP地址
show grants for 'moon'@ '%';
```

#### 删除用户

```sql
--语法:drop user 用户名@IP地址
drop user 'moon'@'%';
```

### DDL(数据定义语言）

DDl主要是用在定义或改变表的结构

#### 创建表

```sql
create table 表名(
	字段名1(列名) 类型(长度) 约束条件,
    字段名2(列名) 类型(长度) 约束条件,
    字段名3(列名) 类型(长度) 约束条件,
    .........
);
```

>在关系型数据库中，我们需要设定表名和列名，同时设定。

#### 数据类型

整形

| MySQL数据类型 | 含义                    |
| ------------- | ----------------------- |
| tinyint       | 1字节，范围（-128~127） |
| smallint      | 2字节，范围（）         |
| mediumint     | 3字节，范围（）         |
| int           | 4字节，范围（）         |
| bigint        | 8字节，非常大           |

在整型中，我们默认使用的是【有符号】的，我们可以使用【unsigned】关键字，定义成无符号类型，tinyint unsigned的取值范围`0~255`.

如果长度需要配合`zerofill`

```sql
int(4) unsigned zerofill
```

> 说明：上述的int长度位4，如果设置了zerofill，如果数据为1，最终存到表格中的数据格式为0001，0010。

| MySql数据类型 | 含义                                                      |
| ------------- | --------------------------------------------------------- |
| float(m,d)    | 4字节，单精度浮点值，m代表总长度，d小数位。               |
| double(m,d)   | 8字节，双精度浮点型，m总长度，d小数位。                   |
| decimal(m,d)  | decimal是存储字符串的浮点数，对应我们java中的BigDecimal。 |

比如定义一个float(5.3):

* 插入123.45678,最后查询的结构就是99.9999；
* 插入12.345678，最后查询得到的结构就是12.346；

> 所以在使用浮点型，要以插入到数据库中的实际数据为准。

##### 字符串类型

| MySQL数据类型 | 含义                        |
| ------------- | --------------------------- |
| char(n)       | 固定长度，最多255个字符。   |
| varchar(n)    | 可变长度，最多65535个字符。 |
| tinytext      | 可变长度，最大255个字节。   |
| text          | 可变长度，最大65535个字节。 |
| mediumtext    | 可变长度，最大16M           |
| longtext      | 可变长度，最大4G            |

(1)char和varchar的区别

* char类型是【定长】的类型，当定义char(10)，输入`123`，他们占用的空间依然是10个字符。当输入的字符如果超出了指定的范围，char会截取超出的字符，而且当存储char ，Mysql会自动删除输入字符串末尾的空格。
* char适合存储很短的，一般固定长度的字符串。例如，char非常适合存储密码MD5值，因为它是一个定长额值，对于短的列，char比varchar在存储空间上效率更高。
* varchar(n)类型用来存储可变长度，长度最大为n个字符的可变长度的字符串数据。比如varchar(10)，然后存储"abc"，实际就是存储了3个字符。
* char类型每次修改的数据长度相同，效率更高，varchar，每次修改的数据长度如果不同，效率更低。

(2)varchar和text的区别

* text不能设置默认值，varchar可以设置默认值。
* text类型，由于单表的最大行宽的限制，支持溢出存储，只会存放768字节在数据页中，剩余的数据存储在溢出段中。
* 一般我们都是用varchar。

##### 日期类型

| MySQL数据类型 | 含义                                       |
| ------------- | ------------------------------------------ |
| date          | 3字节，日期，格式：2022-08-15              |
| time          | 3字节，日期，格式：10:54:30                |
| datatime      | 8字节，日期格式，格式：2022-08-15 10:55:40 |
| timestamp     | 4字节，时间戳，毫秒数。                    |
| year          | 1字节，年份。                              |

#### 建表约束

因为一张表要有多个列，数据库中的表不止有一张，建表约束说的就是我们应该如何规范表中的数据以及表之间的关系。

##### MySQL约束类型

| 约束名称    | 描述                                     |
| ----------- | ---------------------------------------- |
| NOT NULL    | 非空约束                                 |
| UNIQUE      | 唯一约束，取值不允许重复                 |
| PRIMARY KEY | 主键约束(主关键字)，自带非空，唯一、索引 |
| DEFAULT     | 默认值                                   |
| FOREIGH KEY | 外键约束，表和表之间的约束               |

（1）NOT NULL约束 非空约束

```sql
create table student(
	stu_id int,
	stu_name varchar(50) NOT NULL,
	gender char(1) DEFAULT 'n',
	brithday datetime,
	primary key(stu_id)
);
```

(2)UNIQUE约束 唯一约束

```sql
create table book(
	id int primary key auto_increment,
	name varchar(50) not null,
	bar_code varchar(30) not null,
	aut_name int not null,
	unique(bar_code)
);
```

（3）主键约束

用多个列来共同当主键︰

```sql
create table author (
aut_id int，
aut_name varchar (50) not nu11,
gender char(1) default '男',
country varchar (50),
birthday datetime,
PRIMARY KEY(aut_id ,aut_name)
);
```

(4)外键约束

配合生键去使用。有了这个约束，我们在向表中插入数据时，来源于另外一张表的主键

外键会产生的效果:

1. 删除表的时候，如果不删除引用外键的表，被引用的表不能直接删除。
2. 外键的值必须来源于引用的表的主键字符。

```sql
create table `author`(
	`aut_id` int,
	`aut_name` varchar(50) not null,
	`gender` char(1) default '男',
	`country` varchar(50),
	`birthday` datetime,
	PRIMARY KEY(aut_id)
);
```

```sql
create table `book` (
	`id` int PRIMARY KEY auto_increment,
	`name` varchar(50) not null,
	`bar_code` VARCHAR(30) not null UNIQUE,
	`aut_id` int not null,
	FOREIGN KEY(aut_id) REFERENCES author(aut_id)
);
```

> 在创建表的时候，建议字段名使用着重符。

#### 对表的修改操作

查询当前库中所有的表

```sql
show tables;
```

查看表的结构

```sql
desc 表名;
```

修改表5个操作，但是前缀都是一样的 alter table 表名.....;

* 添加一个列：

  ```sql
  alter table sun add (hobby varchar(20),address varchar(50));
  ```

* 修改列数据类型

  ```sql
  alter table author modify address varchar(100);
  ```

* 修改列名称和数据类型

  ```sql
  alter  table 表名 change 列名 新列名 新数据类型;
  alter  table 表名 modify  列名 新数据类型;
  ```

* 删除列

  ```sql
  alter table 表明 drop 列名; 
  ```

* 修改表名

  ```sql
  alter table alter table表名rename to新的表名;
  ```

* 删除表

  ```sql
  drop table表名;
  drop table if exists表名﹔
  ```

### DML（数据库操作语言）

该语言来对表记录进行操作（增删改），不包含查询。

#### 插入数据

```sql
INSERT INTO authors(aut_id，aut_name，gender，country，birthday，hobby )VALUES (4,'罗曼罗兰'，'女'，'漂亮国'，'1945-8-15','写字');
```

如果插入的是全字段，

```sql
INSERT INTO ^authors`VALUES (5,'韩寒'，'男','中国'， '1984-8-15'，'赛车');
```

说明∶

	1. 在数据库中所有的字符串类型，必须使用引号。
 	2. 如果部分字段插入，必须列名和值要匹配。如果全字段插入，则列名可以省略。

批量插入

```sql
INSERT INTO "authors^ VALUES
(7,"李诞","男",'中国','1985-8-15','脱口秀'),
(8,"史铁生","男",'中国','1967-8-15','绘画');|
```

#### 修改数据

修改某列的全部的值：

```sql
update "authors' set aut_name =‘郭小四', country='中国';
```

修改特定行的数据：

```sql
update 'authors' set aut_name = '金庸', country='中国' where aut_id = 1;
```

> where是一个关键字，我们可以使用where关键字实现丰富的筛选，它很像我们的if语句，可以使用各种复杂的条件运算∶
>
> * =
> * \!=
> * \>
>   * where ant_id > 1
> * <
> * \>=
> * \<=
> * < >
> * between...and
>   * where aut_id between 1 and 4
>   * where aut_id > 1 and  ant_name='xxx'
> * in(...)
>   * where aut_id in (1,3,5)
> * is null
>   * where id is null
> * not
>   * where name is not null
> * or
> * and

#### 删除数据

全部删除

```sql
delete from student;
```

根据条件删除

```sql
delete from authors where aut_id=8;
```

> 说明:通过delete这种删除方式删除的数据,主键如果是自定递增的，会断档。

截断：清空表

```sql
TRUNCATE student;
```

> 说明
>
> * truncate实际上应该属于DDl语言，操作立即生效，不能撤回,
>
> * truncate和delete都是删除数据，drop删除整个表。
>
> * truncate速度快，效率高，可以理解为直接删除整个表，再重新建立。
> * truncate和delete都不会是表结构及其列、约束、索引的发生改变。
>
> 


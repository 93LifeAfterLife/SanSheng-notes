## ✒ 前言
作者 : 叁昇 ( 93LifeAfterLife ) 	[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)
公众号:
<div align="center"> <img src="https://i1.fuimg.com/719027/ca0c1d25208ae899.jpg" width="350px"> </div><br>纠错请联系工作邮箱 : tangdingjnust@163.com



## ✒ MySQL 基础

### ↪ MySQL 命名规范

1. 数据库表名、字段名用小写命名, 用下划线分隔, 如 username、id_number、home_addr

2. 数据库字段名禁止使用数据库内置字段 ( 关键字 ) :

   > 请参考 https://www.cnblogs.com/wuyifu/p/5949764.html MySQL 关键字及保留字

   

### ↪ 什么是 SQL 语言?

Structured Query Language , 即结构化查询语言. 是操作所有关系型数据库的通用语言, 相当于 "普通话", 而每一种数据库厂商都有自己的一套规范, 可以看做是 "方言".

SQL 语言的分类有多种, 最重要的有如下: 

1. DDL -- 数据定义语言, `create`、`alter`、`drop` ...
2. DML -- 数据操作语言, `insert`、`update`、`delete` ...
3. DQL -- 数据查询语言, `select` 



### ↪ 数据类型以及字段约束

1. 数值类型

我们知道, Java 中有六种数值类型: byte、short、int、long、float、double; MySQL 中也有对应的: tinyint、smallint、int、bigint、float、double, 区别是存储的大小范围不同, 依次占 1、2、4、8、4、8 字节.

2. 字符串类型
   - char 
   - varchar 
   - text  ( text 最大65535字节,  bigtext 最大可存4GB )

❗ 尤其注意 `char` 和 `varchar` 的区别 :

①. `char(n)` 定长字符串, 最大255字节; 当插入的值长度小于指定的长度 n 时, 剩余的空间将被空格填充, 势必会造成一定的空间浪费, 但是存储的效率比较高.

```
-- 创建用户表, 指定用户身份证编号为 char 类型, 长度固定为18位
create table user(
	id_number char(18),
	...
)
```

②. `varchar(n)` 变长字符串, 最大65535字节 ( 实际应用中, 当长度超出255, 建议使用 `text` 类型 ); 当插入的值小于指定长度 n 时, 剩余的空间可以留给其他数据使用, 避免了空间浪费, 但是, 存储效率比 char 略低.

```
-- 创建用户表, 指定用户名为 varchar 类型, 长度不超过20位
create table user(
	username varchar(20),
	...
)
```

3. 日期类型

| 类型      | 格式                | 范围                                      | 存储   |
| --------- | ------------------- | ----------------------------------------- | ------ |
| YEAR      | YYYY                | 1901 ~ 2144                               | 1 byte |
| TIME      | HH:MM:SS            | -838:59:59 ~  838:59:59                   | 3 byte |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-31                   | 3 byte |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59 | 8 byte |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:01 ~ 2038-01-19 03:14:07 | 4 byte |

❗ 注意 `datetime` 和 `timestamp` 的区别 :

相同点是, 它们俩都是相同格式, 即 "年月日 时分秒", 不同点是 : 

①. datetime 底层存储的就是 "年月日 时分秒", 占 8 byte ; 但 timestamp 存储的是从 1970-01-01 00:00:00 ( 格林威治时间 ) 开始到当前时间的时间毫秒值, 底层是 int , 占 4 byte, 但是显示还是会转换为 "年月日 时分秒";

②. datetime 默认值是 null , 常运用在记录固定时间, 比如 birthday 等; timestamp ( 时间戳 ) 默认值是当前时间, 在插入与修改时, 若不填值, 则默认自动更新为系统当前时间, 常运用在记录时间更改方面, 比如 createtime / updatetime ;

③. 在表中可见, 它们俩的时间范围亦是不一样的, **特别注意** timestamp 的范围是十分小的!!!



4. 字段约束

create table 时, 除了必须规定字段 ( 列 ) 的数据类型, 有时也可以选择给列添加约束, 常见的约束有 : 主键约束、唯一约束、非空约束、外键约束.

①. 主键 ( `primary key` )

主键, 可以唯一的表示一条表记录, 要求是值必须唯一且不能为空. 常用来约束编号, 如

```
-- 创建用户表, 指定 id 为主键, 作为用户的唯一标识
create table user{
	id int primary key auto increment,
	...
}
```

其中 `auto increment` ( 自增 ) 的作用是, 当未给 id 赋值时, 会自动插入上次所记录值 +1 的值, 保证数据的增长和唯一. 

> ❗ 拓展 : MySQL 之 `auto_increment` 
>
> https://blog.csdn.net/qq_40378034/article/details/90736544
>
> MySQL 自增主键详解



②. 唯一 ( `unique` )

唯一, 可以保证所约束列是唯一的, 不能重复, 常用来约束用户昵称, 如

```
-- 创建用户表, 指定用户昵称不能重复
create table user(
	id int primary key auto increment,
	nickname varchar(20) unique,
	...
)
```

❗ **注意 :** 唯一约束, 允许为 null !!!



③. 非空 ( `not null` )

非空, 保证所约束列不能是空值, 但是允许重复, 常用来约束密码, 如

```
-- 创建用户表, 指定用户的密码不能为空
create table user(
	id int primary key auto increment,
	nickname varchar(20) unique,
	password varchar(20) not null,
	...
)
```

非空配合唯一, 常用来约束用户账号, 如

```
-- 创建用户表, 指定用户的账号名不能重复且不能为空
create table user(
	id int primary key auto increment,
	username varchar(20) unique not null,
	nickname varchar(20) unique,
	password varchar(20) not null,
	...
)
```



④. 外键 ( `foreign key` )

外键, 映射两张表之间列与列的对应关系的唯一标识, 确保数据库数据的*完整性和一致性*. 常用于相互有所属关系的两表之间, 比如员工表与部门表.



> ❗为什么说可以确保数据的*完整性和一致性*呢?
>
> 当存在外键约束时, 





## ✒ MySQL sql语句

### ↪ 建库、建表



### ↪ 更新操作

### ↪ 查询操作

### ↪ 表关系





















## ✒ 结语

感谢你的查阅, 欢迎 star 我的 github 开源仓库! 或者关注我的个人公众号! 

[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)

<div align="center"> <img src="https://i1.fuimg.com/719027/ca0c1d25208ae899.jpg" width=""> </div><br>



## ✒ 更新补充 & FAQ
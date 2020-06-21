## ✒ 前言

作者 : 叁昇 ( 93LifeAfterLife ) 	[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)
公众号:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/27/t2XJp6.png" width="150px"> </div><br>纠错请联系工作邮箱 : tangdingjnust@163.com



## ✒ MySQL 基础

### ↪ MySQL 命名规范

1. 数据库表名、字段名用小写命名, 用下划线分隔, 如 username、id_number、home_addr

2. 数据库字段名禁止使用数据库内置字段 ( 关键字 ) :

   > 请参考 https://www.cnblogs.com/wuyifu/p/5949764.html MySQL 关键字及保留字

3. `MySQL` 对大小写敏感吗? 可通过参数 `lower_case_file_system` 查询!

   - 在 `windows` 环境下是不敏感的, 查询时大小写均可

   <div align="center"> <img src="https://i.loli.net/2020/06/06/3Sj7EcLAVsq5h2F.png"/> </div><br>

   - 在 `linux` 环境下是敏感的, 大小写是需要区分的, 注意实际工作中可能会存在的坑!!! 

   <div align="center"> <img src="https://i.loli.net/2020/06/06/21uIbUyR8oArsLv.png"/></div><br>



### ↪ 什么是 SQL 语言?

Structured Query Language , 即结构化查询语言. 是操作所有关系型数据库的通用语言, 相当于 "普通话", 而每一种数据库厂商都有自己的一套规范, 可以看做是 "方言".

SQL 语言的分类有多种, 最重要的有如下: 

1. DDL -- 数据定义语言, `create`、`alter`、`drop` ...
2. DML -- 数据操作语言, `insert`、`update`、`delete` ...
3. DQL -- 数据查询语言, `select` 



### ↪ 数据类型以及字段约束

1. 数值类型

我们知道, Java 中有六种数值类型: byte、short、int、long、float、double; MySQL 中也有对应的: tinyint、smallint、int、bigint、float、double, 区别是存储的大小范围不同, 依次占  tinyint(**1**)、smallint(**2**)、int(**4**)、bigint(**8**)、float(**4**)、double(**8**) 字节.

2. 字符串类型
   - char 
   - varchar 
   - text  ( text 最大65535字节,  bigtext 最大可存4GB )

❗ 尤其注意 `char` 和 `varchar` 的区别 :

①. `char(n)` 定长字符串, 最大255字节; 当插入的值长度小于指定的长度 n 时, 剩余的空间将被空格填充, 势必会造成一定的空间浪费, 但是存储的效率比较高.

```mysql
-- 创建用户表, 指定用户身份证编号为 char 类型, 长度固定为18位
create table user(
	id_number char(18),
	...
)
```

②. `varchar(n)` 变长字符串, 最大65535字节 ( 实际应用中, 当长度超出255, 建议使用 `text` 类型 ); 当插入的值小于指定长度 n 时, 剩余的空间可以留给其他数据使用, 避免了空间浪费, 但是, 存储效率比 char 略低.

```mysql
-- 创建用户表, 指定用户名为 varchar 类型, 长度不超过20位
create table user(
	username varchar(20),
	...
)
```

3. 日期类型

| 类型      | 格式                    | 范围                                          | 存储   |
| --------- | ----------------------- | --------------------------------------------- | ------ |
| YEAR      | YYYY                    | 1901 ~ 2144                                   | 1 byte |
| TIME      | HH: MM: SS              | -838: 59: 59 ~  838: 59: 59                   | 3 byte |
| DATE      | YYYY-MM-DD              | 1000-01-01 ~ 9999-12-31                       | 3 byte |
| DATETIME  | YYYY- MM- DD HH: MM: SS | 1000-01-01 00: 00: 00 ~ 9999-12-31 23: 59: 59 | 8 byte |
| TIMESTAMP | YYYY- MM- DD HH: MM: SS | 1970-01-01 00: 00: 01 ~ 2038-01-19 03: 14: 07 | 4 byte |

❗ 注意 `datetime` 和 `timestamp` 的区别 :

相同点是, 它们俩都是相同格式, 即 "年月日 时分秒", 不同点是 : 

①. datetime 底层存储的就是 "年月日 时分秒", 占 8 byte ; 但 timestamp 存储的是从 1970-01-01 00: 00: 00 ( 格林威治时间 ) 开始到当前时间的时间毫秒值, 底层是 int , 占 4 byte, 但是显示还是会转换为 "年月日 时分秒";

②. datetime 默认值是 null , 常运用在记录固定时间, 比如 birthday 等; timestamp ( 时间戳 ) 默认值是当前时间, 在插入与修改时, 若不填值, 则默认自动更新为系统当前时间, 常运用在记录时间更改方面, 比如 createtime / updatetime ;

③. 在表中可见, 它们俩的时间范围亦是不一样的, **特别注意** timestamp 的范围是十分小的!!!



4. 字段约束

create table 时, 除了必须规定字段 ( 列 ) 的数据类型, 有时也可以选择给列添加约束, 常见的约束有 : 主键约束、唯一约束、非空约束、外键约束.

①. 主键 ( `primary key` )

主键, 可以唯一的表示一条表记录, 要求是值必须唯一且不能为空. 常用来约束编号, 如

```mysql
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

```mysql
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

```mysql
-- 创建用户表, 指定用户的密码不能为空
create table user(
	id int primary key auto increment,
	nickname varchar(20) unique,
	password varchar(20) not null,
	...
)
```

非空配合唯一, 常用来约束用户账号, 如

```mysql
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

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJ0tju.png" width="400px"> </div><br>

> ❗为什么说可以确保数据的*完整性和一致性*呢?
>
> 不妨利用数据库来进行实践 : ( 复制以下代码, 创建 db01 库, 新建员工与部门表, 并插入测试用数据 ), 注意 MySQL 中的注释使用 `--空格` 或者 `#` 开头, 注释后的语句不会执行. 
>
> ```mysql
> -- -----------------------------------------
> -- 测试脚本: 创建 db01 库, 新建员工部门表并插入数据
> -- -----------------------------------------
> -- 删除重复库, 确保 db01 库并不是你重要的数据库
> drop database if exists db01;
> -- 创建 db01 库
> create database db01 charset utf8mb4;
> -- 进入 db01 库
> use db01;
> -- 创建部门表, 约束 id, name 字段
> create table dept(
> 	id int primary key auto_increment,
> 	name varchar(20) unique not null
> );
> -- 创建员工表, 约束 id, name, dept_id 字段
> create table emp(
> 	id int primary key auto_increment,
> 	name varchar(20) unique not null,
> 	dept_id int,
> 	foreign key(dept_id) references dept(id)	-- 设置外键
> );
> 
> -- 往两表中插入测试用数据
> insert into dept values(null, '研发部');
> insert into dept values(null, '测试部');
> insert into dept values(null, '设计部');
> insert into dept values(null, '策划部');
> insert into dept values(null, '运营部');
> insert into dept values(null, '编辑部');
> insert into dept values(null, '市场部');
> insert into dept values(null, '客服部');
> 
> insert into emp values(null, '壹壹', 1);
> insert into emp values(null, '贰贰', 2);
> insert into emp values(null, '叁叁', 2);
> insert into emp values(null, '肆肆', 3);
> insert into emp values(null, '伍伍', 4);
> insert into emp values(null, '陆陆', 4);
> -- ------------- 执行完毕 -------------------
> ```
>
> 你可以在我的github上查看源文件 :  <a href="https://github.com/93LifeAfterLife/SanSheng-notes/tree/master/docs/notes/scripts" target="_blank">点击查看db01脚本</a>

执行完后, 查看数据库和表, 显示为以下结构:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJpM1u.png" width="400px"> </div><br>

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJpA2a.png" width="400px"> </div><br>

- 验证外键作用 :

  - 尝试删除部门表中的存在员工的部门, 例如 1- 研发部

  ```
  delete from dept where id = 1;
  ```

  <div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJp7ei.png" width="400px"> </div><br>

  报错, 无法删除一个父类列.

  这样是不是显而易见, 为什么外键可以确保数据的*完整性和一致性*! 如果想删除存在外键关系的两表中的列, 可必须保证表2中没有引用表1中的列, 否则无法删除!

  在 Navicat 中, 可以直观地看库中所有表的模型, 并查看之中的外键关系: 

  <div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJ0flv.png" width="400px"> </div><br>

  右键需要查看的库, 点击 "逆向数据库到模型..." 即可.

  

## ✒ MySQL sql语句

### ↪ 建库、建表

1. 查看数据库服务器中的所有数据库

```mysql
show databases;
```

2. 进入指定的数据库, 如进入 db01 库

```mysql
use db01;
```

3. 查看当前库中的所有表

```mysql
show tables;
```

4. 创建数据库

```mysql
-- 如果存在则删除重复的库
drop database if exists db01;
-- 重新创建 db01 库, 并设置编码为 utf8mb4, 切记要指定编码.
create database db01 charset utf8mb4;
-- 创建所需的表, 例如学生表
create table stu(
	id int,
    name varchar(20),
    sex char(1),
    birthday date
);				-- 注意格式
-- 查看建表结构
desc stu;
-- 查看建表语句
show create table stu;
```

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJSHoa.png" width="500px"> </div><br>

### ↪ 更新操作

1. `insert` —— 插入表记录

```mysql
insert into table_name(col1, col2, ...) values (value1, value2, ...);
```

为所有列复制时, 表名后的括号内容可以省略, 但是插入值的对应顺序应该和声明( 建表 )时的顺序一致; 若没有省略括号内容, 则需要做到前后一一对应; 当插入字符串( 包括日期 )时, 需要使用单引号.

```mysql
insert into stu values(1, '壹壹', '男', '1993-1-1');
insert into stu(id, name, sex) values(2, '贰贰', '女');
```

2. `alter` —— 修改表结构

```
-- 在 stu 表中新增成绩列
alter table stu add column score float; 
```

3. `update` —— 修改表记录

```mysql
-- 目前两位学生的分数为默认值 null, 给'壹壹'更新分数
update stu set score = 91.5 where name = '壹壹';

-- 若没有 where 条件子句, 则默认修改所有学生, 比如全体加5分
update stu set score = score + 5;

-- 特别注意, 由于表中存在 score 默认为 null, null+5 还是为 null, 则需通过 ifnull 函数将 null 的值置为 0; 这里的 ifnull(score, 0) 可以看作是一个新表, 表名就是 ifnull(score, 0)!
update stu set score = ifnull(score, 0) + 5;
```

4. delete` —— 删除表记录

```mysql
-- 删除 stu 表中 id=2 的学生数据
delete from stu where id = 2;

-- 删除 stu 表中的所有数据, 但是表结构还在, 表重置为空集
delete from stu;
```



### ↪ 查询操作

> 首先准备数据, 执行脚本( 直接复制到MySQL终端命令行中 ): 
>
> 你可以在我的github上查看源文件 :  <a href="https://github.com/93LifeAfterLife/SanSheng-notes/tree/master/docs/notes/scripts" target="_blank">点击查看db02脚本</a>

新建 db02, 建立 emp 表, 插入初始数据, 查询结果如下:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/29/tJXOb6.png" width="400px"> </div><br>

1. 简单查询

```mysql
-- 查询 emp 表中的所有员工, 显示员工姓名、薪资、奖金
select name, sal, bonus from emp;
```

2. 剔除查询

```mysql
-- 查询 emp 表中的所有部门, 剔除重复的结果
select distinct dept from emp;

-- 查询 emp 表中的所有奖金数值, 剔除重复的结果; 重命名表 ifnull(bonus, 0) 为"奖金数值"
select distinct ifnull(bonus, 0) as 奖金数值 from emp;		-- as 可以缺省
```

3. 条件查询

对表记录进行筛选, 使用 `where` 子句进行条件查询, 下列表中的运算符皆可以在 `where` 条件中使用:

|        运算符        | 含义                                                         |
| :------------------: | :----------------------------------------------------------- |
| =、<>、>、>=、<、<=  | 等于、不等于、大于...                                        |
|   between x and y    | x、y之间, 包含x、y                                           |
|         like         | 模糊查询, 配合 `%` ( 通配所有字符 )和 `_` ( 任意一个字符 ) 使用 |
|          in          | 集合查询                                                     |
|         not          | 否定条件                                                     |
|         and          | 同时满足                                                     |
|          or          | 只要满足其中之一即可                                         |
| is null、is not null | 判断是否为 null 值, 切记不能使用 "= null"                    |

```mysql
-- 查询 emp 表中薪资大于 3000 的所有员工, 显示姓名和薪资
select name, sal from emp where sal>3000;

-- 查询 emp 表中总薪资大于 3500 的所有员工, 显示姓名和总薪资, 注意前后表名保持一致!
select name, (sal+ifnull(bonus, 0)) as 总薪资 from emp where (sal+ifnull(bonus, 0))>3000;
```

- **注意** : `where` 子句中不能使用列别名, 但是可以使用表别名或者使用 ( 表的别名+属性名即列名 ), 举例如下:

```mysql
-- 使用表别名查询 emp 表中的所有员工, 显示姓名和部门
select e.name, e.dept from emp as e;				-- 同样的, as 可以缺省

-- 使用属性查询 emp 表中薪资大于 3000 的所有员工, 显示姓名、职业和薪资
select name, job, sal from emp where emp.sal>3000;

-- 使用表别名+属性查询 emp 表中薪资大于 3000 的所有员工, 显示姓名、职业和薪资
select name, job, sal from emp e where e.sal>3000;

-- 查询 emp 表中姓名以"壹"开头的员工, 显示该员工的详细信息
select * from emp where name like '壹%';

-- 查询 emp 表中薪资为 1400、1600、1800 的员工, 显示员工姓名和薪资
select name, sal from emp where sal in(1400, 1600, 1800);

-- 查询 emp 表中薪资小于 2000 和 薪资大于 4000 的员工, 显示员工姓名和薪资
select name, sal from emp where sal<2000 or sal>4000;
```

- **拓展** :

  -  `where` 与 `having` 的区别

  先对 `having` 的用法做个示例:

  ```mysql
  -- 根据职位进行分组, 统计每个职位的最低薪资
  select job, min(sal) from emp group by job;
  
  -- 筛选出薪资大于 1500 的职位
  select job, min(sal) from emp group by job having min(sal)>1500; 
  ```


以上在进行分组后, 就无法使用 `where` , 只能用 `having` , 并放在分组之后.

还有一个区别就是, `where` 子句**不能使用列别名和聚合函数**, ( 如 count, sum, max, min, avg ... ), 而 `having` 子句中可以使用列别名和聚合函数.

究其原因, 都是因为 MySQL 语句的执行顺序导致的.

- SQL语句的书写顺序
  
    | SQL书写顺序 ( 由上到下 ) | 含义                                   |
    | ------------------------ | -------------------------------------- |
    | select 列名              | 查询哪些列                             |
    | from 表名                | 查询哪张表                             |
    | where 子句               | 通过条件筛选过滤, 剔除不符合条件的记录 |
    | group by 列名            | 按列分组                               |
    | having 子句              | 通过条件对分组后的数据进行筛选过滤     |
    | order by 列              | 按列排序                               |
  | limit x, y               | 指定返回 y 页第 x 条记录               |
  

  
- SQL语句的执行顺序
  
  | SQL执行顺序( 由上到下 ) |
  | ----------------------- |
  | from 表/表别名          |
  | where 子句              |
  | select 列名/列别名      |
  | group by 列名           |
  | having 子句             |
  | order by 列             |
| limit x, y              |
  
  由以上两个表可以看出, `where` 虽然写在 `select` 后, 但是 `where` 是在 `select` 之前执行的, 所以 `where` 里不能使用列别名, 因为列别名要在 `select` 里定义后才有效; 但是 `where` 可以使用表别名, 因为 `where` 在  `from` 后执行!

4. 排序查询

order by 排序的列 asc ( 升序 ) / desc ( 降序 )

```mysql
-- 对 emp 表中所有员工的薪资进行升序排序, 显示姓名和薪资
select name, sal from emp order by sal;			-- 默认 asc 排序, 句尾的 asc 可缺省
```

5. 分组查询

```mysql
-- 对 emp 表按照部门进行分组, 并统计每个部门的人数, 显示部门和部门人数
select dept 部门, count(*) 部门人数 from emp group by dept;

-- 查看每个部门的最高薪资
select dept 部门,max(sal) 最高薪资 from emp group by dept;
```

6. 聚合函数查询

常用的聚合函数有, max, min, count, sum, avg

- **特别注意** : 可以使用 `count(*)` 统计总行数; 多个聚合函数可以一起查询; where 子句中不能使用聚合函数; 在未按列进行分组时, 不能将聚合函数同字段一起查询. 

```mysql
-- 根据部门进行分组, 统计每个部门员工人数和平均薪资
select dept 部门, count(*) 员工人数, avg(sal) 平均薪资 from emp group by dept;

-- 查询 emp 表中薪资最高的员工姓名
select name, sal from emp where sal =(
	select max(sal) from emp
);				-- 使用子查询, 并不能直接用 select name, max(sal) from emp; 切记!!!
```

7. 其他函数

| 数值函数      | 含义     |
| ------------- | -------- |
| ceil( 数值 )  | 向上取整 |
| floor( 数值 ) | 向下取整 |
| round( 数值 ) | 四舍五入 |
| rand( 数值 )  | 随机数   |

| 含义                            | 日期函数                                           |
| ------------------------------- | -------------------------------------------------- |
| 当前日期 ( 年月日 )             | curdate()                                          |
| 当前时间 ( 时分秒 )             | curtime()                                          |
| 当前日期+时间 ( 年月日 时分秒 ) | now()                                              |
| 增加/减少日期                   | date add(), date sub()                             |
| 获取各个单位下的值              | year(), month(), day(), hour(), minute(), second() |

```mysql
-- 将员工"贰贰"的薪资上涨 5.718%, 向上取整
update emp set sal = ceil(sal*1.05718) where name='贰贰';		-- 原 2500 更新后为 2693

-- 查询 emp 表中所有员工的年龄, 并按年龄降序排序, 显示姓名和年龄
select name 姓名, year(curdate())-year(birthday) 年龄 from emp order by 年龄 desc;

-- 查询 emp 表中所有在93到95年间出生的员工, 显示姓名与出生日期
select name, birthday from emp where year(birthday) between 1993 and 1995;
```

8. 连接 ( 关联 ) 查询

> 同样的, 首先准备数据, 执行脚本( 直接复制到MySQL终端命令行中 ): 
>
> 你可以在我的github上查看源文件 :  <a href="https://github.com/93LifeAfterLife/SanSheng-notes/tree/master/docs/notes/scripts" target="_blank">点击查看db03脚本</a>

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/30/tVOqUa.png" width="400px"> </div><br>

```mysql
-- 查询所有部门以及其下的员工
select * 
from dept d, emp e
where d.id=e.dept_id;			-- 分列来书写有助于梳理逻辑, 表名过长时, 可以使用别名的方式

-- 或者使用内连接查询的方式
select * 
from dept d inner join emp e on d.id=e.dept_id;		-- inner join ... on ... 详见后续
```

查询结果为:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVDeov.png" width="400px"> </div><br>

```mysql
-- 优化结果集表头名
select d.id 部门编号, d.name 部门, e.id 员工编号, e.name 员工 
from dept d, emp e
where d.id=e.dept_id;		-- 保证表头名没有重复的, 避免造成歧义
```

查询结果为:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVDn0D.png" width="400px"> </div><br>

8. 外连接查询

   ①. 内连接查询 ( 显示两表中具有映射的所有记录, 过滤 null 值 )

   ```mysql
   -- 查询所有部门以及员工, 不显示没有部门的员工
   select * 
   from dept d inner join emp e on d.id=e.dept_id;
   ```

   ②. 左外连接查询 ( 显示左侧表中的所有记录, 若右侧表中没有对应记录, 则显示为 null )

   ```mysql
   -- 查询所有部门以及员工, 若部门下没有员工, 显示 null
   select *
   from dept d left join emp e on d.id=e.dept_id;
   ```

   ③. 右外连接查询 ( 与左连接相反 )

   ```mysql
   -- 查询所有部门以及员工, 若员工没有所属部门, 显示 null
   select * 
   from dept d right join emp e on d.id=e.dept_id;
   ```

   以上两种查询结果为:

   <div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVDuci.png" width="400px"> </div><br>

9. 子查询

子查询, 顾名思义, 即是将一个查询得到的结果作为另一个查询的条件; 也可以看作是一个查询得到了一张新的整理好的表, 再在此表的基础上进行查询.

> 准备数据, 执行脚本( 直接复制到MySQL终端命令行中 ): 
>
> 你可以在我的github上查看源文件 :  <a href="https://github.com/93LifeAfterLife/SanSheng-notes/tree/master/docs/notes/scripts" target="_blank">点击查看db04脚本</a>

执行完后进行查询, 得到的表记录如下: 

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVDSQF.png" width="400px"> </div><br>

```mysql
-- 列出薪资比"王壹壹"高的所有员工, 显示姓名和薪资
select name, sal
from emp 
where sal>(
select sal from emp where name="王壹壹"		-- 单引号和双引号均可
);

-- 列出与'刘肆肆'从事相同职位的所有员工, 显示姓名、职位以及部门
-- 这次的查询结果涉及到两张表, 可以分三步梳理查询逻辑:
-- 1. 先关联查询两张表, 得到员工以及员工对应的部门的结果集/表
select e.name, e.job, d.name from emp e, dept d where e.dept_id=d.id;
-- 2. 再查询'刘肆肆'的职位
select job from emp where name='刘肆肆';
-- 3. 使用子查询
select e.name 员工姓名, e.job 职位, d.name 部门名
from emp e, dept d
where e.dept_id=d.id and job=(
select job from emp where name='刘肆肆'
);
```

查询结果: 

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVEVXX.png" width="400px"> </div><br>

```mysql
-- 列出薪资高于'运营部'最高薪资的所有员工的姓名、薪资以及部门名
-- 分析:
-- 1. 查询'运营部'的部门编号
select id from dept where name='运营部';
-- 2. 根据部门编号查询'运营部'的最高薪资
select max(sal) from emp where dept_id=(
select id from dept where name='运营部'
);
-- 3. 关联查询, 获得结果
select e.name 员工姓名, e.sal 薪资, d.name 部门名
from emp e, dept d
where e.dept_id=d.id and sal>(
	select max(sal) from emp where dept_id=(
		select id from dept where name='运营部'
	)
);
```

查询结果为:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVEc2T.png" width="400px"> </div><br>

10. 查询练习

供参考的表记录:

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/06/01/tVDSQF.png" width="400px"> </div><br>

```mysql
-- (外连接) 列出所有部门和其下的员工, 如果部门下没有员工, 显示null
select * 
from emp e right join dept d on e.dept_id=d.id; 
```

查询结果: 

<div align="center"> <img src="https://i.loli.net/2020/06/03/bZzSfEFWyDnBQrI.png"/> </div><br>

```mysql
-- (关联查询) 列出在'测试部'任职的所有员工的详细信息
select * 
from emp e, dept d
where e.dept_id=d.id and d.name='测试部';
```

查询结果:

<div align="center"> <img src="https://i.loli.net/2020/06/03/iAyIwFzEhO1fD7V.png"/>
 </div><br>

```mysql
-- (自连接) 列出所有员工及其直接上级, 显示员工的姓名、上级的编号与姓名
select e1.name, e1.topid, e2.name
from emp e1, emp e2
where e1.topid=e2.id;
```

查询结果:

<div align="center"> <img src="https://i.loli.net/2020/06/03/xNMjlpBfQhSY7nW.png"/>
 </div><br>

```mysql
-- (分组、聚合函数) 列出最低薪资大于1500的各种职位, 显示职位和该职位的最低薪资
select job, min(sal) 
from emp 
group by job
having min(sal)>1500;
```

查询结果:

<div align="center"> <img src="https://i.loli.net/2020/06/03/JdFugpO7BSvfbDs.png"/>
 </div><br>

```mysql
-- (分组、组合函数) 列出在每个部门就职的员工数量、平均工资, 显示部门编号、员工数量以及平均薪资
select dept_id, count(*), avg(sal)
from emp
group by dept_id;
```

查询结果:

<div align="center"> <img src="https://i.loli.net/2020/06/03/rLOVXZ8CsNblQRn.png"/>
 </div><br>

```mysql
-- (分组、关联、聚合函数) 查询至少有一个员工的部门, 显示部门编号、名称、位置以及人数
select d.id, d.name, d.loc, count(*) 
from emp e, dept d
where e.dept_id=d.id
group by d.id;
```

查询结果:

<div align="center"> <img src="https://i.loli.net/2020/06/09/ePnc8lGtTIjpbao.png"/> </div><br>

```mysql
-- (自连接) 列出受雇日期早于直接上级的所有员工的编号、姓名以及部门名
select e1.id, e1.name, d.name
from emp e1, emp e2, dept d
where e1.dept_id=d.id and e1.topid=e2.id and e1.hdate<e2.hdate;
```

查询结果: 

<div align="center"> <img src="https://i.loli.net/2020/06/09/nZzEXNsjWJifGgm.png"/>
 </div><br>



### ↪ 修改表操作

```mysql
-- 创建测试用库 db06
create database db06;
use db06;
create table emp(
	id int,
    name varchar(20),
    sex char(1),
    bitth date
);
```

1. 新增列 `add`

```mysql
alter table emp add dept_id int, add addr varchar(50) not null;
```

2. 修改列 `modify`

```mysql
alter table emp modify id primary key;
alter table emp modify id auto_increment; 	# 若没有设置主键, 无法设置自增
```

3. 删除列 `drop`

```mysql
alter table emp drop addr;
```

4. 删除主键

   ①. 若存在自增则先删除自增, 使用 `modify` 修改

   ```mysql
   alter table emp modify id int;
   ```

   ②. 再删除主键约束

   ```mysql
   alter table emp drop primary key;
   ```

5. 添加外键

```mysql
-- 在 db06 中创建新的员工表和部门表
drop table emp;

create table dept(
	id int primary key auto_increment,
	name varchar(20),
	loc varchar(50)
);

create table emp(
	id int primary key auto_increment,
    name varchar(20),
    dept_id int,
    foreign key(dept_id) references dept(id)
);
```

现在有员工和部门表, 那么让哪张表来维护 ( 存放 ) 外键呢?

答案是, 员工表, 一个部门应对多个员工, 则需*将映射关系保存在多的一方中*!

```mysql
show create table emp;	# 可以查看到外键名, 如 'emp_ibfk_1'
```

倘若一开始忘记了设置外键

```mysql
alter table emp drop foreign key emp_ibfk_1;	# 删除外键模拟未设置外键
```

则再次添加外键: 

```mysql
alter table emp add constraint fk_dept_id foreign key(dept_id) references dept(id);		# 外键约束的名称可以自己取便于理解的名称
```



### ↪ 表关系

之前我已经概括了什么是外键, 外键的作用和用法; 外键体现了表之间的关系, 而表关系又分为 "一对一"、"一对多( 多对一 )" 和 "多对多".

针对日常生活中复杂的公司、单位、部门、人员的关系, 又该如何来设计数据库和表呢? 首先第一步就是确定**表关系**! 先看一个出自《数据库系统概论( 第4版 )》[作者: 王珊] 里的工厂物资管理 `E-R 图`:

> E-R 图提供了表示实体-属性-联系的方法, 是数据库概念模型设计的方法:
>
> 矩形框表示实体型, 椭圆形( 圆角矩形 ) 表示属性, 菱形表示联系

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/30/tV8wAF.md.png" width="400px"> </div><br>

对于复杂的多对多关系, 可以将表拆分为两张一对多的关系, 因为无法在两张表中添加列来保存多对多的关系, 所以可以添加一张第三方的表, 来专门保存两张表的主键, 从而实现了保存两张表的关系. 如下图所示 : 

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/30/tVO1fT.png" width="400px"> </div><br>



### ↪ 备份与恢复

- 备份 ( 转储 ) 指令 `mysqldump` 

不用进入 mysql 的 client , 直接在命令行中使用 `mysqldump + <` 命令

```mysql
mysqldump -u<username> -p db05 > 绝对路径\db05.sql
```

如: 

<img src="https://i.loli.net/2020/06/09/L2dcfjYnie8HEuP.png"/>

将在目标文件夹下备份一个 `db05.sql` 文件.

你可以用文本编辑器打开这个 sql 文件, 会发现, 备份只是备份了数据表, 并没有建库语句!!! 所以恢复数据库时, 还需要手动按照文件名来创建相同名的库, 这也是备份时使用原来库名的理由.

- 恢复

同样不需要进入 client, 直接使用 `mysql + >` 指令

```mysql
mysql -u<username> -p db05 < 绝对路径\db05.sql
```

为了测试, 我们先进入 client 中将 db05库, 彻底删除:

```mysql
drop database db05;
show databases;	# 确保已删除

-- 记住一定要创建一个库!
create database db05 charset utf8;
\q	# 退出数据库
```

<img src="https://i.loli.net/2020/06/09/eAX1OyU5cHm3n64.png"/>

再去查看数据库里的表, 发现已然恢复, 和所转储备份的一模一样.



### ↪ 事务操作

> 查看 [数据库系统原理](mysql_数据库系统原理.md) 中的事务第四节




## ✒ 结语

感谢你的查阅, 欢迎 star 我的 github 开源仓库! 或者关注我的个人公众号! 

[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/27/t2XJp6.png" width="150px"> </div><br>



## ✒ 更新补充 & FAQ

### ↪ 补充 : SQL练习题

借鉴于网上流传甚广的50道 `SQL` 训练题, 在参考的同时尽可能作出最简洁、最富有逻辑的解答.

- 关系模型
  - **注意:** *关系的每一个分量必须是一个不可分的数据项, ( 理论上不允许表中还有表 )*

1. 学生( 学生编号, 姓名, 生日, 性别) 
2. 课程( 课程编号, 名称, 授课老师编号)
3. 教师( 教师编号, 姓名)
4. 成绩( 学生编号, 课程编号, 分数)

> 准备数据, 执行脚本( 直接复制到MySQL终端命令行中 ): 
>
> 你可以在我的github上查看源文件 :  <a href="https://github.com/93LifeAfterLife/SanSheng-notes/tree/master/docs/notes/scripts" target="_blank">点击查看db05脚本</a>

- 表数据

<img src="https://i.loli.net/2020/06/08/w5MdTPJbLxWBOVA.png"/>

( 注意成绩表中的 s_id 与 c_id 为联合主键, 创建一个单独的表用来保存多个主键的方式, 可以保证数据的唯一性和完整性, 在 Java 中也可以将联合主键的字段单独放在一个类中, 将联合主键的字段都注解为 `@Id` , 并在类上添加 `@IdClass( className.class )` 注解, 表明这是一个联合主键类 )

- 问题与解答: 

> 参考文章: 
>
> https://cloud.tencent.com/developer/article/1540526
>
> https://www.jianshu.com/p/476b52ee4f1b
>
> 或者查看我的笔记 :
>
> - [ ] -- TODO



### ↪ 补充 : CentOS下安装MariaDB

- 为何使用 `MariaDB` 替代 `MySQL`?

> MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可 。开发这个分支的原因之一是：甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险， 因此社区采用分支的方式来避开这个风险。 MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。 在存储引擎方面，10.0.9版起使用XtraDB（名称代号为Aria）来代替MySQL的InnoDB。
>
> From : wikipedia

1. 安装 `MariaDB` 依赖库, 首先配置仓库入口文件

**注意 :** 切记!!! 不要在生产环境下, 对所有包或者 linux内核系统的版本进行更新, 特别是在 CentOS 下**不要**轻易使用 `yum update` 操作!!! 否则导致应用的依赖版本变更, 将造成大错!!! 因为我在很多介绍如何安装 `MariaDB` 或者 `MySQL` 的文章中都看到, 有不少作者将这个指令加入其中, 虽然单机测试中, 并没啥影响, 最多是花点时间重新更新下包、软件和内核, 但是我还是要提醒初学者, **切记不要在生产环境当中, 随意去升级已有的依赖或者系统!!!**

首先我们进入 `MariaDB` 官网的下载中心, 查询自己的服务器系统、位数以及想要安装的数据库版本.

> `MariaDB` 官方安装文档 : https://downloads.mariadb.org/mariadb/repositories

如, 我的服务器系统为, CentOS v7.0 64 位 ( 选择 x86_64 ), 接着选择 `MariaDB` 的版本, 我这里选择稳定版 `10.4[Stable]`. 见下图 :

<div align="center"> <img src="https://i.loli.net/2020/06/08/9XU4pm3MfkYtv5z.png"/> </div><br>

接着进入终端命令行, 若使用 `YUM` 的方式安装, 则需按照官方文档所述, 新建一个 `MariaDB YUM` 仓库入口的配置文件

```
[root@localhost ~]# vi /etc/yum.repos.d/MariaDB.repo
```

文件名可以自定义, 也可以按照官方的要求使用类似的, 便于后续修改.

在文件中粘贴以下配置 ( 如果你选择了不同的内核位数与数据库版本, 应复制你所看到的官网文档内容 ) : 

```
# MariaDB 10.4 CentOS repository list - created 2020-06-06 13:16 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

`esc + :wq` 保存退出即可, 我们就配置了仓库入口文件.

2. 查询是否已然安装了 `MariaDB` 或者有冲突的 `MySQL` 依赖

```
[root@localhost ~]# rpm -qa | grep mariadb
[root@localhost ~]# rpm -qa | grep mysql
```

没有返回消息则表示没有残留的依赖包. 如果存在, 则应先删除

```
[root@localhost ~]# rpm -e mysql-*
错误：未安装软件包 mysql-* 
[root@localhost ~]# rpm -e mariadb-*
错误：未安装软件包 mariadb-* 
```

也可以选择使用 yum 的方式针对性地删除, 例如: 

```
[root@localhost ~]# yum remove mysql
```

会处理所有 `MySQL` 的依赖关系.

3.  使用 `yum` 安装 `MariaDB` 的服务器与客户端

```
[root@localhost ~]# yum -y install MariaDB-server MariaDB-client
```

<div align="center"> <img src="https://i.loli.net/2020/06/08/Rr2bPf9BDqvdkWc.png"/> </div><br>

图中, 我已然安装过, 所以会提示无须任何处理, 第一次安装完毕会有事务概要提示, 全部 ok 后即为安装成功, 不成功的原因最可能是前面第 2 步骤未全部清理旧依赖! 建议重新清理一遍.

4. 启动 `MariaDB` 服务, 并进入 `MariaDB` 进行测试, 观察下默认设置与 `Windows` 的不同之处

```
[root@localhost ~]# systemctl start mariadb			# 启动 mariadb 服务
[root@localhost ~]# mariadb -uroot -p				# 使用 root 权限进入 mariadb
```

<div align="center"> <img src="https://i.loli.net/2020/06/08/lAkTC85WgDIN294.png"/> </div><br>

查看之前我所记录的一个坑 : 大小写问题!

```mysql
MariaDB [(none)]> show variables like '%case%';
```

<div align="center"> <img src="https://i.loli.net/2020/06/08/uEwL8VpShFzNBki.png"/> </div><br>

5. MySQL 或者 MariaDB ( 可以看做本质是一样的 ) 在 Linux 下的重要指令

- 服务命令:

```
systemctl start mariadb				# 启动服务
systemctl stop mariadb.service		# 关闭服务
systemctl restart mariadb 			# 重启服务
systemctl enable mariadb			# 允许开机启动
```

- 初始化配置:

```
mysql_secure_installation
```

 进入配置引导页, 先输入默认的 root 密码, 直接回车即可

```
Enter current password for root (enter for none):  # 初次运行直接回车
Set root password? [Y/n] # 是否设置root用户密码，输入y并回车或直接回车
New password: # 设置root用户的密码
Re-enter new password: # 再输入一次你设置的密码
Remove anonymous users? [Y/n] # 是否删除匿名用户，回车
Disallow root login remotely? [Y/n] # 是否禁止root远程登录,回车,
Remove test database and access to it? [Y/n] # 是否删除test数据库，回车
Reload privilege tables now? [Y/n] # 是否重新加载权限表，回车
```

初始化 `MariaDB` 完成, 可以进行登陆测试!

- 配置字符集:

  默认的字符集规则如下:

  <img src="https://i.loli.net/2020/06/08/I9FJWfTdvnxEuyt.png"/>

  - GBK、UTF-8、utf8mb4
    - GBK 专门用于解决中文编码, *不论中英文都是双字节*; 若用于中文程序开发, 客户也为中文, 则使用 GBK;
    - UTF-8 用来解决国际上字符的多字节编码, 对*英文为单字节*, 对*中文为三字节*; 中外都有的用户, 则使用 UTF8节省英文字符空间;
    - utf8mb4 ( most bytes 4 ) 用于*兼容四字节的 unicode*, 是 UTF-8 的超集; 保持4字节数据, 避免个别字符的显示乱码问题, 比如 emoji 采用的是4字节, 若需存储则使用 utf8mb4 .
  - 注意在 `MySQL` 的 UTF-8 中支持的最大字符长度为3字节 ( 0xffff ) , 很多不常用的汉字或者新增的 unicode 字符如果是4字符的, 则在 mysql 中是不支持的,  建议换做 utf8mb4, 而且使用 `varchar` 替代 `char` 避免空间的浪费.
  - 配置方法一 ( 通过配置文件 ) :  

```
[root@localhost my.cnf.d]# cd ~
[root@localhost ~]# cd /etc/my.cnf.d
[root@localhost my.cnf.d]# ls
enable_encryption.preset  mysql-clients.cnf  server.cnf
```

接着修改 `server.cnf` 文件. 这种方式较为繁琐, 但是一劳永逸, 不需每次启动服务时重新设置.

在 [mysqld] 标签下添加

```
init_connect='SET collation_connection = utf8_unicode_ci' 	# 连接使用的编码
init_connect='SET NAMES utf8' 
character-set-server=utf8	# 默认的内部操作字符集
collation-server=utf8_unicode_ci 	
skip-character-set-client-handshake
```

> 参考: https://yq.aliyun.com/articles/283374  MySQL字符集和校对规则（Collation）作者: [小麦苗](https://yq.aliyun.com/users/nudg3bxzllofu)

在 `client.cnf` 的 [client] 标签下添加

```
default-character-set=utf8
```

在 `mysql-clients.cnf` 的 [mysql] 标签下添加

```
default-character-set=utf8
```

重启 mysql 服务. 则全部初始化为 utf-8 字符集.

现在的字符集规则如下 : 

<img src="https://i.loli.net/2020/06/08/bQN3P1J8BMkzA9L.png"/>

校验字符集如下: 

<img src="https://i.loli.net/2020/06/08/q2JfugEapMo9xSL.png"/>



- 添加用户并设置权限

```mysql
create user username@localhost identified by 'password';	# 创建新用户
grant all on *.* to username@localhost identified by 'password'; # 授权
grant all on *.* to username@'%' identified by 'password'; # 外网登陆权限
grant all privileges on *.* to username@'hostname' identified by 'password' with grant option;	# 授予权限控制的权限
```

<img src="https://i.loli.net/2020/06/08/bF7kMKrcuPxXvZ5.png"/>

你可以在 mysql 库下查询用户信息 :

```mysql
use mysql;
select host,user,password from user;
```

更精细的赋予权限, 可以将指令中的 `all privileges` 改为 `select,insert,update,delete,create,drop,index,alter,grant,references,reload,shutdown,process,file` 其中的单独权限即可.



修改密码: ( 使用 root 相同权限的账户登录 )

```mysql
mysql> alter user "root"@"localhost" identified by "新密码";  --方法1
mysql> update user set authentication_string=password("新密码") where user="root";  -- 方法2
mysql> flush privileges;
mysql> quit
```



- 查询库和表占用的空间

```mysql
select table_schema,round(sum(DATA_LENGTH/1024/1024),2) as datasize from tables 
group by table_schema;	# 查询所有库大小, 取两位有效小数, 按库名排序
 
select table_name, concat(round(sum(data_length/1024/1024),2),'MB') as datasize 
from tables 
where table_schema='mysql' 
group by table_name;	# 查询 mysql 库下所有表的大小, 取两位有效小数, 以MB为单位, 按表名排序
```

这一阶段的补充就告一段落了! 感谢!

> 参考文献: https://www.cnblogs.com/river2005/p/6813618.html 作者: clearriver



### ↪ 补充 : 选择MySQL还是PostgreSQL?

实际工作中, `MySQL` 的应用场景还是更多的, 但是不乏很多大型公司数据库有转向 `psSQL` 的倾向. 建议有时间还是需要预先了解, 只有实际使用过才可以在遇到时稳住脚跟.

个人实感, 它们的关系就像你买手机选择 `xiaomi` 还是 `huawei` 一样, 差别没有那么大. 

你可以看看它们俩官网的介绍, 就可知一二: 

<img src="https://i.loli.net/2020/06/08/CmTEk3o7uNVshiX.png"/>

MySQL - 世界上最受欢迎 ( 使用最广 ) 的开源数据库. ( 并入Oracle 后存在闭源风险, 但短时间内使用没有任何问题 ).

<img src="https://i.loli.net/2020/06/08/CGBLpORD9aucW6m.png"/>

PostgreSQL - 世界上最先进 ( 最智能, 设计更科学 ) 的开源关系型数据库. 

如果你想具体了解, 可以参考他人的博客文章; 我这里也发现一篇好的讨论可以推荐一下: 

> https://www.zhihu.com/question/20010554 知乎问答: PostgreSQL 与 MySQL 相比，优势何在？
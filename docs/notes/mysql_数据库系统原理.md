# 💾 数据库系统原理

## ✒ 前言
作者 : 叁昇 ( 93LifeAfterLife ) 	[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)
公众号:
<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/27/t2XJp6.png" width="150px"> </div><br>纠错请联系工作邮箱 : tangdingjnust@163.com



## ✒ 数据库事务

### 一. 什么是事务

数据库事务, Database Transaction, 是指一系列作为单个逻辑工作执行的操作, 要么全部执行, 要么全部不执行. 换言之, 就是将一系列作为一个单一工作的需要处理的 SQL 绑定在一条绳子上, 中间有一环出现问题断掉了, 那么整条绳子, ( 事务断开 ) 就都不可再用了.

最好理解的例子就是, 转账事务: 

Jack 需要给 Rose 转账 500.00 元, 这就涉及到了至少两条 sql 语句 ( 不考虑其他需要做记录的表 ) : 

```mysql
-- Jack 的账户余额减去 500
update account set balance=balance-500 where name='Jack';	# ①
-- Rose 的账户余额加上 500
update account set balance=balance+500 where name='Rose';	# ②
```

 我们来设想这样的情形: 

① 执行完毕, 成功, Jack 的 balance 已然被减去 500. 但是 ② 还未来得及执行, 或者执行不成功, 程序就抛出异常并终止了, 这时候没有任何保障的两条语句, 第一条执行完了, 余额扣了, 但是 Rose 的账户却没有收入 500, 这是相当毁灭性的错误了.

所以我们必须给这两条执行一个业务逻辑的 sql 语句添加一层保障. 这个保障要确保业务逻辑的原子性、一致性、隔离性、持久性.



### 二. ACID 四大事务特性

1. 原子性 ( **Atomicity** , 美 /ˌætəˈmɪsəti/ ) : 事务中所有操作都是原子单位, 是不可再分割的. 换言之, 就是一条逻辑下的所有操作全部执行成功或者全部不成功, 回到最初模样;
2. 一致性 ( **Consistency** , 美 /kənˈsɪstənsi/ ) : 事务执行后, 数据库状态要符合业务规则. 比如转账, 无论成功与否, 金额总数应该保持不变;
3. 隔离性 ( **Isolation **,  美 /ˌaɪsəˈleɪʃn/ ) : 在并发操作中, 不同事物之间应该隔离开, 每条业务线程中的事物不会互相干扰. 就是说, 从一个事务中去查看另一个事务中的数据状态, 要么看到的是另一个事务开始之前的初态, 要么看到的是它执行完成后的终态, 永远不能查看到执行中数据的中间状态.
4. 持久性 ( **Durability** , 美 /ˌdʊrəˈbɪləti/ ) : 当事务完成时, 其中涉及到的数据操作都必须持久化到数据库中, 即便提交完事务后, 数据库崩溃, 也要保证在数据库重启后能恢复到事务完成的状态.



### 三. MySQL 如何保证事务的 ACID ?

InnoDB 下有如下四条保证:

1. `redo log` 重做日志 : 保证事务的持久性 ( **D** )
2. `undo log` 回滚日志 : 保证事务的原子性 ( **A** )
3. `redo log + undo log` : 保证事务的一致性 ( **C** )
4. 共享和排他锁 : 保证事务的隔离性 ( **I** )

>  参考博文: https://www.cnblogs.com/jianzh5/p/11643151.html



### 四. MySQL 中的事务操作

- 开启事务 : `start transaction`
- 结束事务 : `commit (提交) / rollback (回滚)`

演示案例: 

```mysql
-- 创建 javadb 数据库, 你可以在我的 Jdbc 文章中找到同样的测试用代码
drop database if exists javadb;
create database javadb charset utf8;
use javadb;
create table account(
	id int primary key auto_increment,
    name varchar(20),
    balance double
);

-- 插入数据
insert into account values(null, 'Jack', 500.00);
insert into account values(null, 'Rose', 1500.50);
insert into account values(null, 'Julie', 520.10);
insert into account values(null, 'Hanck', 2500.55);
insert into account values(null, 'Jobes', 3500.28);
-- --------- execute completed --------------------
```

1. 回滚事务

```mysql
-- 查询账户表中 Jack 和 Rose 的账户信息
use javadb;
select * from account where name="Jack" or name="Rose";
```

<img src="https://i.loli.net/2020/06/09/LDiedf6sY1n3xEh.png"/>

```mysql
-- 开启事务, 进行转账业务
start transaction;
-- 将 Rose 的余额减少 100
update account set balance=balance-100 where name="Rose";
-- 查询账户表中 Jack 和 Rose 的账户信息
select * from account where name="Jack" or name="Rose";
```

<img src="https://i.loli.net/2020/06/09/h9ZPYxuy8gEQpHL.png"/>

```mysql
-- 将 Jack 余额增加 100
update account set balance=balance+100 where name="Jack";
-- 查询账户表中 Jack 和 Rose 的账户信息
select * from account where name="Jack" or name="Rose";
```

<img src="https://i.loli.net/2020/06/09/OnSIm4eE5NHAYs8.png"/>

```mysql
-- 撤销转账业务, 回滚事务
rollback;
-- 查询账户表中 Jack 和 Rose 的账户信息
select * from account where name="Jack" or name="Rose";
```

<img src="https://i.loli.net/2020/06/09/1SHf2LwFJRbOxA4.png"/>

2. 提交事务

```mysql
-- 开启事务, 进行转账业务
start transaction;
-- 将 Rose 的余额减少 100
update account set balance=balance-100 where name="Rose";
-- 将 Jack 余额增加 100
update account set balance=balance+100 where name="Jack";
-- 模拟确认转账无误后, 提交事务
commit;
-- 查询账户表中 Jack 和 Rose 的账户信息
select * from account where name="Jack" or name="Rose";
```

<img src="https://i.loli.net/2020/06/09/Gfpe9k3ZgdRiTut.png"/>

3. 中断事务

```mysql
-- 开启事务, 进行转账业务 ( 此时经过上述操作后 Jack 600, Rose 1400.50 )
start transaction;
-- 将 Rose 的余额减少 100
update account set balance=balance-100 where name="Rose";
-- 将 Jack 余额增加 100
update account set balance=balance+100 where name="Jack";
-- 模拟程序异常, 事务被中断 (直接退出了 Mysql client)
quit;
-- 再次启动 client
mysql -uroot -p
use javadb;
-- 查询账户表中 Jack 和 Rose 的账户信息
select * from account where name="Jack" or name="Rose";
```

Jack 600, Rose 1400.50 , 和开启事务前数据一致.

以上都是演示的单个事务, 然而现实中的例子都是多个事务对同一块数据进行操作, 称之为事务并发. 

- 事务并发下, 存在以下三类问题, 假设存在两个事务 A 和 B
  - 脏读 ( dirty read ) : A 读到 B 的未提交的数据
  - 不可重复读 ( unrepeatable read ) : 对统一记录的两次读取结果不一致, 例如, A 中, 前后两次查询 Jack 的余额, 在两次查询间隔里, B 中修改了 Jack 的余额. 导致 A 中两次的查询不一致. 称为不可重复读 ( *针对修改操作* )
  - 幻读 ( phantom read ) : 情形同不可重复读, 但是针对的是*插入或者删除操作*, 即读到了并不存在的记录.
- 以下我们来模拟这三类问题

4. 脏读

   为了模拟事务并发, 我们需要多开 mysql client, 即使用两个窗口, 不妨称它们分别为窗口 A 与 B, 相当于事务 A 与 B 

   ```mysql
   -- 首先查看下当前的数据状态
   use javadb;
   select * from account;
   ```

   Jack 600, Rose 1400.5

   ```mysql
   -- 在 A 中, 开启事务, 执行 Rose 转账500给 Jack
   -- 首先设置数据库, 允许脏读、不可重复读和幻读
   set tx_isolation = 'read-uncommitted';
   -- 开启事务
   start transaction;
   update account set balance=balance-500 where name='Rose';
   update account set balance=balance+500 where name='Jack';
   
   -- 在 B 中, 开启事务, 查询 Jack 的余额
   set tx_isolation = 'read-uncommitted';
   start transaction;
   select * from account where name='Jack';
   #结果是 Jack 1100, 未提交的数据已然被其他事务读到了, 这就是脏读
   
   -- 回到 A 中, 将事务回滚
   rollback;
   
   -- 再到 B 中, 进行查询
   select * from account where name='Jack';
   #结果是 Jack 600, 说明上面的 1100 是脏读数据
   ```

<img src="https://i.loli.net/2020/06/09/useM2pFbISOBNok.png"/>



5. 不可重复读

```mysql
-- 接着上面的两个窗口, 在 A 中, 开启事务, 查询 Rose 的余额
start transaction;
select * from account where name='Rose';
# Rose 余额为 1400.5

-- 在 B 中, 开启事务, 将 Rose 的余额减去200, 然后查询 Rose 的余额, 最后提交事务
start transaction;
update account set balance=balance-200 where name='Rose';
select * from account where name='Rose';
# Rose 余额为 1200.5
commit;

-- 切换到 A 中, 再次查询 Rose 的余额
select * from account where name='Rose';
# Rose 余额为 1200.5
```

<img src="https://i.loli.net/2020/06/09/3o5pYTbkDSc9f72.png"/>

这样你会发现在 A 事务中, 两次相同的查询得到了不同的记录, 原因就是另一个事务 B 对账户余额进行了修改, 这种情况就是 "不可重复读".



6. 幻读

```mysql
-- 还是在原来的两个窗口下, 先把之前的事务全部提交完毕
-- 然后在 A 中, 开启事务, 查询账户表中是否存在 id=8 的用户
start transaction;
select * from account where id=8;
# Empty set 不存在 id=8 的记录

-- 在 B 中, 开启事务, 往账户表中插入一条 id=8 的记录, 并提交事务
start transaction;
insert into account values(8, 'Enterprise', 345.5);
commit;

-- 切换回 A 中, 由于该事务中并没有 id=8 的记录, 则可以插入 id=8 的新纪录
insert into account values(8, 'AzurLane', 1149.9);
# 插入失败
-- 查询 id=8 的用户信息
select * from account where id=8;
```

<img src="https://i.loli.net/2020/06/09/hOn5IMbvQwS32xJ.png"/>

这样先没有的记录, 又出现了, 仿佛出现了幻觉, 所以被称作为 "幻读".

这样看来, "不可重复读" 和 "幻读" 是差不多的, 前者是一个事务中读到了另一个事务中修改了的记录, 造成前后记录不一, 后者是一个事务中读到了另一个事务中新增或者删除的记录, 造成记录突然出现或者消失的错觉. 而 "脏读" 则是一个事务中读到了另一个事务中尚未提交的记录.



### 五. 事务隔离级别

MySQL InnoDB 默认的隔离级别为 `REPEATABLE-READ` , 即可重复读, 可以防止脏读和不可重复读, 但是无法处理幻读问题;

```mysql
select @@tx_isolation;	# 查看隔离级别
```

<img src="https://i.loli.net/2020/06/11/CjkHusbSqfVZE5l.png"/>

Oracle 数据库默认的则是 `READ-COMMITTED` , 即读已提交的记录, 防止脏读, 但是无法处理不可重复读与幻读;

还有 `READ-UNCOMMITTED` ( 读未提交的记录, 三种问题都无法处理 ) 和 `SERIALIZABLE` ( 串行化, 三种问题都可以处理 ).

四种事务的隔离级别的并发能力也是不一样的, 处理的问题越多, 则性能越差, 性能由高到低排序如下 :

`READ-UNCOMMITTED` > `READ-COMMITTED` > `REPEATABLE-READ` > `SERIALIZABLE`

 在开发中, 往往不需要特地的去修改事务隔离级别.



## ✒ 结语

感谢你的查阅, 欢迎 star 我的 github 开源仓库! 或者关注我的个人公众号! 

[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)

<div align="center"> <img src="https://t1.picb.cc/uploads/2020/05/27/t2XJp6.png" width="150px"> </div><br>



## ✒ 更新补充 & FAQ


# 💿 Redis

## ✒ 前言

作者 : 叁昇 ( 93LifeAfterLife ) 	[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)
公众号:

<div align="center"> <img src="https://i1.fuimg.com/719027/ca0c1d25208ae899.jpg" width="350px"> </div><br>纠错请联系工作邮箱 : tangdingjnust@163.com



## ✒ Redis 简介

<div align="center"> <img src="https://i1.fuimg.com/719027/cd38f0ad91d1f766.png" width="" style="background:#99CCCC"> </div><br>

官方网址 : https://redis.io/

中文网址 : http://www.redis.cn/

GitHub : https://github.com/microsoftarchive/redis/releases

Redis 全称 "**Re**mote **Di**ctionary **S**erver", 是一个 日志型、`Key-Value`  存储系统, 仅仅只有 5MB 大小, 但是功能强大, 支持网络、基于内存、可持久化, 完全免费开源 ( BSD协议, 意味着企业在选型技术时, 对该协议的第三方代码可以完全控制 ). 发展至今, Redis 已经具备了多种语言的 API, 常常被用来做**数据库**、**高速缓存**、**消息队列代理**

Redis 也被称作 "数据结构服务器", 因为它存储的 `Value` 包括五种数据类型: `String`, `Hash`, `List`, `Set`, `Zset`. 这些也是开发中最基本的数据结构. 

### 🔥 优势

- 丰富的数据类型.
- 所有的操作都是原子性的, 保证执行要么成功、要么失败回滚至完全不执行.
- 性能极高, ( Redis 读的速度是 110000 次/s,写的速度是 81000 次/s ) 运行在内存里, 持久化到硬盘.
- 内置 Lua 脚本、LRU算法回收没有必要保存的数据.
- 通过 Sentinel 提供高可用, 通过 Cluster 提供自动分区.



## ✒ Redis 安装与配置

### 🪟 Windows环境

<div align="center"> <img src="https://i1.fuimg.com/719027/d32e0997d6c6406a.png" width="350px"> </div><br>

- 使用 `.msi` 安装的形式则可以通过勾选自动配置 Redis 的环境变量.
- 如果使用 `.zip` 解压文件形式, 则可以手动配置系统环境变量, 过程如下:

1. 右键 "我的电脑" - "属性" - "高级系统设置" - "环境变量"

2. 在 "系统环境变量" 下找到 `Path` 点击 "编辑" , 

   <div align="center"> <img src="https://i1.fuimg.com/719027/ffccc47d60854eee.png" width="350px"> </div><br>

   添加 Redis 的解压路径, 即所有的与 Redis相关的 `.exe` 文件所在的目录.

   <div align="center"> <img src="https://i1.fuimg.com/719027/ad7e7e5293d9c8fa.png" width="350px"> </div><br>

3. `win+R` 唤醒命令行, 输入如下命令, 即可启动 Redis. 详细命令在下方的 Redis 服务中会再提到. Redis 默认占端口 6379. 

```
redis-server <redis的conf文件>					// 不加配置则使用默认配置		
```

<div align="center"> <img src="https://i1.fuimg.com/719027/1172d6d90fc70ead.png" width="350px"> </div><br>

windows 下需重新再开启一个 cmd 窗口, 同时保持开启 server 的窗口不关闭, 然后进入 Redis 客户端:

```
redis-cli
```

更多 Redis 命令参考 : http://doc.redisfans.com/

### 🪟 Linux环境

演示 CentOS 下 Redis 的安装与配置 :

1. 首先下载 redis 的打包文件 : http://download.redis.io/releases/redis-5.0.5.tar.gz  ( 截止2020年5月16日 最新稳定版为 v5.0.5 ).

2. 使用远程管理工具, 如 `Xshell` 连接 CentOS 服务器, 将压缩包拷贝到 `/usr/src/` 目录下. 并进行解压编译与安装. 
   
   <div align="center"> <img src="https://i2.tiimg.com/719027/8c368467356c7983.png" width="350px"> </div><br>
   
   - 如何连接, 可以参考我的另一篇笔记 [Linux]( linux.md ) ( 待完善 ) 

```
[root@localhost ~]# cd /usr/local/src
[root@localhost src]# mkdir tars
[root@localhost src]# cd tars
[root@localhost tars]#  ...       				// 将文件 redis-5.0.5.tar.gz 直接拖入到 Xshell 完成上传
[root@localhost tars]# tar -xvf redis-5.0.5.tar.gz
[root@localhost tars]# mv redis-5.0.5 /usr/local/src/redis		// 更改和移动目录名, 这只是我的个人习惯
[root@localhost tars]# cd /usr/local/src/redis
[root@localhost redis]# make						// 进行编译
[root@localhost redis]# make install				// 安装 redis
[root@localhost redis]# clear						// 清屏
```

3. 修改 Redis 配置文件 `redis.conf`, **请记住**, 这是 redis 最重要的一个文件, 将来很多的用法需要在此文件或者此文件的副本中去进行

```
[root@localhost redis]# vim redis.conf
```

第一次运行 redis 只需要修改三处: 

- 去除 IP 绑定

<div align="center"> <img src="https://i2.tiimg.com/719027/d3bca2eb739a1928.png" width="350px"> </div><br>

- 关闭保护模式

<div align="center"> <img src="https://i2.tiimg.com/719027/152df9410a04c7ce.png" width="350px"> </div><br>

- 开启后台启动

<div align="center"> <img src="https://i2.tiimg.com/719027/24bc05efc3762a00.png" width="350px"> </div><br>

4. 启动服务 : 

```
[root@localhost redis]# redis-server redis.conf
```

5. 停止:

```
[root@localhost redis]# redis-cli shutdown
```

或者检索服务, 直接杀死进程

```
[root@localhost redis]# ps -ef | grep redis
[root@localhost redis]# kill -9 <PID>
```

4. 进入与退出客户端 : 

```
[root@localhost redis]# redis-cli				// 进入
127.0.0.1:6379> exit							// 退出
```



## ✒ Redis 简单数据操作

### ↪ String 类型

|    命令    |               说明               |                             演示                             |
| :--------: | :------------------------------: | :----------------------------------------------------------: |
|    set     |         添加 key - value         |                        set key value                         |
|    get     |        按 key 获取 value         |                           get key                            |
|   strlen   |         获取 key 的长度          |                          strlen key                          |
|  🔔 exists  |        判断 key 是否存在         |                          exists key                          |
|    del     |       按 key 删除一条记录        |                           del key                            |
|   🔔 keys   |        查询符合条件的 key        |      keys *   查询全部 key<br> keys k?y   <br> keys ke*      |
|    mset    |          赋值多个 k - v          |                    mset k1 v1 k2 v2 k3 v3                    |
|    mget    |         获取多个 k 的值          |                        mget k1 k2 k3                         |
|  🔔 append  |  对某个 key 的原有值后进行追加   |                       append key value                       |
|    type    |         查看 key 的类型          |                           type key                           |
|   select   |        切换 redis 数据库         | select <0-15> <br> 🔔 redis 中默认有 16 个数据库, <br>进入客户端默认选择 0 号数据库. <br> 集群模式下只有一个 0 号数据库! |
|  flushdb   |       清空当前所在的数据库       |                           flushdb                            |
| 🔔 flushall |         清空所有的数据库         |                           flushall                           |
|    incr    |             将值加 1             |                           incr key                           |
|    decr    |             将值减 1             |                           decr key                           |
|   incrby   |          按指定数值加值          |                           incrby 5                           |
|   decrby   |          按指定数值减值          |                           decrby 5                           |
|  🔔 expire  |  指定 key 的存储时间, 单位 / 秒  |                        expire key 10                         |
|  pexpire   | 指定 key 的存储时间, 单位 / 毫秒 |                       pexpire key 1000                       |
|    ttl     |     查看 key 的剩余存活时间      | ttl key<br> 自然数表示存活时间<br> -1 表示永久存在<br> -2 表示不存在 |
|  persist   |       撤销 key 的失效时间        |                         persist key                          |

 

### ↪ Hash 类型

- 以散列类型保存对象和属性值, 如对象 `User { id: 2, username: A, password: 123 }`

|   命令   |                          说明                           |                      演示                       |
| :------: | :-----------------------------------------------------: | :---------------------------------------------: |
|   hset   |                 添加对象的一条属性与值                  |              hset key field value               |
|   hget   |                  获取对象的指定属性值                   |                 hget key field                  |
| hexists  |                 判断对象的属性是否存在                  |                hexists key field                |
|   hdel   |                     删除对象的属性                      |     hdel key field <br> hdel key f1 f2 ...      |
| hgetall  |               获取对象的所有属性与属性值                |                   hgetall key                   |
|  hkeys   |                   获取对象的所有属性                    |                    hkeys key                    |
|   hlen   |                获取对象的所有属性的数量                 |                    hlen key                     |
|  hmset   |                 为对象的多个属性设定值                  |          hmset k f1 v1 f2 v2 f3 v3 ...          |
|  hmget   |                 获取对象的指定属性的值                  |                hmget k f1 f2 ...                |
| 🔔 hsetnx | 添加对象的一条属性与值, <br> 🔔 前提条件是该属性不存在时 | hsetnx key field value<br> 若存在 field, 返回 0 |
| hstrlen  |                获取对象中指定属性的长度                 |                hstrlen key field                |
|  hvals   |                  获取对象的所有属性值                   |                    hvals key                    |



### ↪ List 类型

- Redis 中的 List 集合是[双向循环链表](数据结构.md), 可以从两个方向插入数据, 但是其中的数据无法被用作缓存, 因为它们都将被消费, 所以一般用来做队列 / 栈.

  - 队列: 存入数据的方向和获取数据的方向相反
  - 栈: 存入数据的方向和获取数据的方向相同

  左方向: `left`  , 右方向: `right`, 注意指令方向的不同, 也可以看作头尾.

|  命令  |                             说明                             |                             演示                             |
| :----: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| lpush  |                      从队列左边压入元素                      |                      lpush k1 v1 v2 ...                      |
| rpush  |                      从队列右边压入元素                      |                      rpush k1 v1 v2 ...                      |
|  lpop  |                      从队列左边弹出元素                      |                           lpop key                           |
|  rpop  |                      从队列右边弹出元素                      |                           lpop key                           |
| lpushx |         从队列左边压入元素,<br> 前提条件是该队列存在         |                       lpushx key value                       |
| rpushx |                        同上, 方向相反                        |                       rpushx key value                       |
| lrange |                  从队列中获取指定返回的元素                  | lrange key start stop<br>🔔 lrange key 0 -1 获取全部队列的元素 |
|  lrem  | 从存在 key 的列表里移除前 count 次<br> 出现的值为 value 的元素:<br> count > 0 从头往尾移除<br>  count < 0 从尾往头移除<br> count = 0 移除所有 | lrem key count value<br> lrem name -3 tom 从存在 name 的<br> 列表里移除最后两个压入的 hello<br> 🔔 如果列表中不存在 key 则返回 0 |
|  lset  |                 设置指定位置的 list 元素的值                 |                     lset key index value                     |

关于 `lrem` 具体效果可见下例 :

<div align="center"> <img src="https://i1.fuimg.com/719027/eefc132096be299a.png" width="350px"> </div><br>

## ✒ Redis 事务

Redis 中单一操作都是原子性的, 支持事务, 一项任务可以由事务包裹起来处理, 一旦一个命令失败, 则可以回到事务起点, 从而实现事务回滚.

1. 开启事务, 并记录一个事务的起点, 使用 `multi` 指令标记 :

```
127.0.0.1:6379> multi
OK
```

2. 假如来执行一连串的出入栈操作 :

```
127.0.0.1:6379> lpush name q w e r t y u i o p
QUEUED
127.0.0.1:6379> lpop name
QUEUED
127.0.0.1:6379> lset name 5 a
QUEUED
127.0.0.1:6379> rpop name
QUEUED
127.0.0.1:6379> rpush name z x c v b
QUEUED
127.0.0.1:6379> lpop name
QUEUED
```

3. 以上的命令还未执行, 如果要执行, 使用 `exec` 执行指令 :

```
127.0.0.1:6379> exec
1) (integer) 14
2) "p"
3) OK
4) "jerry"
5) (integer) 17
6) "o"
```

4. 如果要舍弃当前事务呢? 我们清空 redis db, 再次执行 1, 2 两个步骤, 然后使用 discard 指令来丢弃 multi 之后的全部命令, 实现事务回滚 :

```
127.0.0.1:6379> flushall
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> lpush name q w e r t y u i o p
QUEUED
127.0.0.1:6379> lpop name
QUEUED
127.0.0.1:6379> lset name 5 a
QUEUED
127.0.0.1:6379> rpop name
QUEUED
127.0.0.1:6379> rpush name z x c v b
QUEUED
127.0.0.1:6379> lpop name
QUEUED
127.0.0.1:6379> discard
OK
127.0.0.1:6379> lrange name 0 -1
(empty list or set)							// 事务回滚到最初的空数据库状态
```



## ✒ Redis 应用案例

在 `Java` 中, 使用 `Jedis` 工具包来连接与处理 `Redis`  













## ✒ 结语

感谢你的查阅, 欢迎 star 我的 github 开源仓库! 或者关注我的个人公众号! 

[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)

<div align="center"> <img src="https://i1.fuimg.com/719027/ca0c1d25208ae899.jpg" width=""> </div><br>



## ✒ 更新补充 & FAQ


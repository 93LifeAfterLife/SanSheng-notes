# 💿 Redis

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

## ✒ Redis 命令

### ↪ String 类型

|  命令  |          说明           |                        演示                        |
| :----: | :---------------------: | :------------------------------------------------: |
|  set   |    添加 key - value     |                   set key value                    |
|  get   |    按 key 获取 value    |                      get key                       |
| strlen |     获取 key 的长度     |                     strlen key                     |
| exists |    判断 key 是否存在    |                     exists key                     |
|  del   |   按 key 删除一条记录   |                      del key                       |
|  keys  |   查询符合条件的 key    | keys *   查询全部 key<br> keys k?y   <br> keys ke* |
|  mset  |     赋值多个 k - v      |               mset k1 v1 k2 v2 k3 v3               |
|  mget  |     获取多个 k 的值     |                   mget k1 k2 k3                    |
| append | 对某个 key 的值进行追加 |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |
|        |                         |                                                    |

 

### ↪ Hash 类型



### ↪ List 类型




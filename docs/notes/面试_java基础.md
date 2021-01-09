## ✒ 前言

作者 : 叁昇 ( 93LifeAfterLife ) 	[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)
公众号:

<div align="center"> <img src="https://i.loli.net/2020/06/03/XZ4FkMv3O8x2cjH.png" width="250px"> </div><br>纠错请联系工作邮箱 : tangdingjnust@163.com



## ✒ 面试问题总结

### ↪ 001. 无参构造方法的作用？为什么需要无参构造？只写有参构造可行吗？

`Java` 程序在执行子类的构造方法之前，会默认使用 `super()` 来调用父类特定的构造方法；换句话说，我们在实例对象的时候，需要不断地向上（父类）回溯，直到 `Object()`。要想实现这一过程，则需要一条道路，这条道路默认就是 `super()`。子类的构造器无论重载多少个，**第一行必须**是 `super()`！这样便保证了道路的顺畅。不写时默认使用无参的构造方法，写参数则调用父类相应的有参构造。一句话总结就是：如果父类中没有无参构造，那么子类必须用 `super(args...)` **显式**地调用父类的构造方法。

另外 `this()` 是本类对象引用，`super()` 是父类对象引用。它们都必须在构造方法的第一行，但不能同时使用，无参数时可以省略不写。



### ↪ 002. 为什么要用DataSource？DataSource的原理？如何使用？

首先要知道什么是数据源，`DataSource`。`JDK` 的 `rt.jar` 包中有一个 `API` 包：`javax`，即 `java extension` ，属于标准库 `java` 的拓展。在 `JDBC标准` 下提供了 `javax.sql.DataSource` 接口，负责建立与数据库的连接，

<img src="https://i.loli.net/2021/01/09/qbHLlPAF9xgIeDu.png"/>



### ↪ 003. 如何理解 Java 的面向对象？详细解析？

封装 / 继承 / 多态（自动装箱与拆箱）/ 组合 /（重写 override & 重载 overload）/ 单继承？多重继承？ / ...



### ↪ 004.



### ↪ 005.



### ↪ 006.



### ↪ 007.



### ↪ 008.



### ↪ 009.



### ↪ 010.



### ↪ 011.



### ↪ 012.



### ↪ 013.



### ↪ 014.



### ↪ 015.



### ↪ 016.



### ↪ 017.



### ↪ 018.



### ↪ 019.



### ↪ 020.


# JDBC

## JDBC 概述

Java Database Connectivity , 即 Java 连接数据库, 是一门通过 Java 语言来操作数据库的技术. 通常使用了三层架构 ( USL -> BLL -> DAL ) 的思想.

<img src="https://i.loli.net/2020/06/09/ziLDtFRVc3mZgrO.jpg"/>



## JDBC 访问数据库

1. 设计需求: 

   ①. 利用 Java 程序查询 javadb 库中的 account 表中的所有账户余额记录, 并打印到控制台. 

   ②. 进行增删查改操作. 目的是熟悉对结果集的处理方式.

2. 开发步骤:

   ①. 准备数据

   ( 单机测试, 可以使用虚拟机, 我这里直接使用本地 , 在数据库中执行下列脚本 )

   ```mysql
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

   ②. 在 IDE 中创建 Java 工程

   ( 我这里演示使用 Idea )

   新建 Java 项目并引入 Jdbc 依赖 

   <img src="https://i.loli.net/2020/06/09/AlUib25tgdRhp9Y.png"/>

   >  Jdbc 依赖: 

   3. 编码实现:

   ```java
   package org.ss.jdbc;
   
   import java.sql.*;
   
   public class JdbcDemo {
       public static void main(String[] args) throws ClassNotFoundException, SQLException {
           // 1. 加载数据库驱动
           Class.forName("com.mysql.jdbc.Driver");
           // 2. 连接数据库
           Connection con = DriverManager.getConnection(
                   "jdbc:mysql://localhost:3306/javadb?useUnicode=true&characterEncoding=utf-8", "sansheng", "mydb1993");
           // 3. 建立传输器
           Statement sta = con.createStatement();
           // 4. 利用传输器发送 SQL 语句到目标数据库中执行, 完成后返回结果集
           ResultSet res = sta.executeQuery("select * from account");
           // 5. 处理返回的结果集
           while(res.next()){
               int id = res.getInt("id");
               String name = res.getString("name");
               double balance = res.getDouble("balance");
               System.out.println(id + "   |  " + name + " |   "+ balance);
           }
           // 6.关闭连接并释放资源 ( 粗糙的方式 )
           res.close();
           sta.close();
           con.close();
       }
   }
   ```

   4. 运行结果: 

   <img src="https://i.loli.net/2020/06/09/Rgb8as4qSPGtZU3.png"/>

   5. 增删查改: 

      例如: 插入一条新纪录, Tom : 10000.00

      ( 这里使用了 JdbcUtil , 在下面一节中可以查看详细 )

      ```java
      package org.ss.jdbc;
      
      import java.sql.*;
      
      public class JdbcDemo02 {
          public static void main(String[] args) throws Exception {
              // 1. 利用封装的工具直接连接数据库
              Connection con = JdbcUtil.getCon();
              // 2. 获取传输器对象
              Statement sta = con.createStatement();
              String sql = "insert into account values(null , 'Tom', 10000.00)";
              int row = sta.executeUpdate(sql);
              System.out.println("Query OK, "+ row + " row affected" );
      
              ResultSet res = sta.executeQuery("select * from account");
              while(res.next()){
                  int id = res.getInt("id");
                  String name = res.getString("name");
                  double balance = res.getDouble("balance");
                  System.out.println(id + "   |  " + name + " |   "+ balance);
              }
              JdbcUtil.close(con, sta, res);
          }
      }
      ```

   6. 使用 PreparedStatement 对象, 防止 SQL 注入攻击

   使用 preparedStatement 替代 createStatement .

   如:

   ```java
   String sql = "select * from user where username=? and password =?";
   ```

   其中的 ? 之外的 sql 语句, 会被预编译, ? 传入的参数无法改变原来的结构.



## JDBC  要点

### 连接数据库的 URL

```
jdbc:mysql://localhost:3306/javadb
```

localhost:3306 单机测试, 可以不写

`jdbc:mysql` 是协议名称.

后面的两个参数则是数据库的账密.



### 资源的优雅释放

秉承越晚创建的对象越晚先关闭, 则应先关闭 ResultSet 对象, 再关闭 Statement 对象, 最后关闭 Connection 对象. 这样的代码块比较臃肿, 则可以新建一个 Util 类, 封装 Jdbc 的资源释放代码, 随取随用.

```java
package org.ss.jdbc;

import java.sql.*;

/**
 * Jdbc 工具类
 */
public class JdbcUtil {
    /**
     * 连接数据库并返回连接对象
     * @return Connection 对象
     */
    public static Connection getCon() throws Exception{
        // 1. 加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        // 2. 连接数据库
        Connection con = DriverManager.getConnection("jdbc:mysql:///javadb", "sansheng", "mydb1993");
        return con;
    }

    /**
     * 释放资源
     * @param con 连接对象
     * @param sta 传输器对象
     * @param res 结果集对象
     */
    public static void close(Connection con, Statement sta, ResultSet res){
        if (res != null){
            try {
                res.close();
            } catch (SQLException e){
                e.printStackTrace();
            } finally {
                res = null;
            }
        }
        if (sta != null){
            try {
                sta.close();
            } catch (SQLException e){
                e.printStackTrace();
            } finally {
                sta = null;
            }
        }
        if (con != null){
            try {
                con.close();
            } catch (SQLException e){
                e.printStackTrace();
            } finally {
                con = null;
            }
        }
    }
}
```



### 数据库连接池

池, 用来共享对象, 减少对象重复创建与销毁的次数, 实现复用, 提高程序执行的效率.

数据库连接池, 即 DataSource , 也称为数据源.

使用连接池的好处在于, 当用户需要连接时, 直接在池中取出一个初始化就绪的连接, 当用户用完这次连接时, 也不需要关闭这次连接, 直接将连接还回池中, 下一个用户需要连接时, 也是同样的操作. 减少连接开关次数, 则提高了程序执行的效率.

- C3P0 连接池

> 你可以访问这里找到 c3p0 包:

配置 c3p0 连接池连接数据库的基本信息有如下三种方式 ( 推荐使用 xml 配置, yml 或者 properties 配置文件 ) :

1. java 代码

```java
// 创建连接池
ComboPooledDataSource pool = new ComboPooledDataSource(); 

// 配置连接
pool.setDriverClass("com.mysql.jdbc.Driver");
pool.setJdbcUrl("jdbc:mysql:///javadb");
pool.setUser("sansheng");
pool.setPassword("mydb1993");
```

2. xml 文件

在类目录, 或者源码目录, 比如 src 目录下, 新建一个 `c3p0-config.xml` 文件, 保存连接池配置, 这种方式的好处, 在于可以动态修改配置, 不用去源代码里修改, 增加开发成本.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	<default-config>
		<property name="driverClass">
			com.mysql.jdbc.Driver
		</property>
		<property name="jdbcUrl">
			jdbc:mysql:///javadb
		</property>
		<property name="user">
			sansheng
		</property>
		<property name="password">
			mydb1993
		</property>
	</default-config>
</c3p0-config>
```

3. properties 文件

同 xml 文件的位置, 可以添加一个 `c3p0.properties` 文件

```properties
c3p0.driverClass=com.mysql.jdbc.Driver
c3p0.jdbcUrl=jdbc:mysql:///javadb
c3p0.user=sansheng
c3p0.password=mydb1993
```

- 案例 : 

( TODO )
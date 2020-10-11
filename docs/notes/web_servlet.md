# Servlet

## Servlet 概述

`Servlet`, 全称为 `Server Applet`, 即在服务器上运行的 `Java` 应用程序. 本质上就是一个 `Java` 程序, 但是由于没有 `main` 函数导致无法独立运行, 所以必须使用 `Servlet` 容器 ( 例如: `Tomcat` 服务器 ), 供其调用才可以执行.

`Servlet` 的作用就是处理 `web` 请求, 服务器将接收到的请求交给 `Servlet` 进行处理, 整个的简略过程如下: 

1. `Client` 发送请求至 `Service`;
2. `Service` 将请求信息交给 `Servlet` 处理;
3. 返回结果交给 `Service`;
4. `Service` 将结果响应给 `Client` ( 如, 浏览器 ).



## Servlet 入门

### 开发步骤

1. 自定义一个类, 实现 `Servlet` 接口 ( 或继承 `Servlet` 接口的子类, 比如 `GenericServlet` 或 `HttpServlet` ) , 并添加未实现的方法;
2. 将编译后的类放置到 `Servlet` 容器 ( `web` 应用 ) 中, 并在 `web` 应用的 `web.xml` 配置文件中配置 `Servlet` 对外访问的虚拟路径, 然后将 `web` 应用部署到虚拟主机中.

### 开发案例

**注:** 此案例使用 `Eclipse IDE`

1. 创建 `web` 工程: `New --> Dynamic Web Project`

<img src="https://i.loli.net/2020/07/01/SZecEh2B7qabgzp.png"/>

2. 配置基本的项目信息 ( 项目名, 路径, 运行环境以及版本 )

<img src="https://i.loli.net/2020/07/01/wAnKMGFmR79pdzI.png"/>

**注:** 版本默认是 3.0, 将不会自动创建 `web.xml` 文件, 且无法生成 `Servlet` 所需的配置信息! 建议使用 2.5 方便开发.

默认的 `web.xml` 文件信息如下:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>TestServlet</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

项目结构: 

<img src="https://i.loli.net/2020/07/01/73Ry4QSH9csDvaP.png"/>

4. 在项目的 `src` 目录下, 创建 `Servlet` 应用服务: `New --> Servlet`

<img src="https://i.loli.net/2020/07/01/QsJdOfRVW8Fn1hi.png"/>

5. 配置 `Servlet` 信息 ( 包路径, 类名, 所继承的父类 )

<img src="https://i.loli.net/2020/07/01/OY2Jsk6ranVdiFE.png"/>

创建好的自定义 `Servlet` 类如下 ( 默认继承了 `HttpServlet` 类 ) : 

```java
package org.ss;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public HelloServlet() {
        super();
    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```

继承会覆盖 `doGet` 和 `doPost` 方法, 它们的作用分别为:

​	`doGet` : 当客户端发送请求的方式是 `GET` 提交时, `Servlet` 将调用 `doGet` 方法来处理请求;

​	`doPost` : 当客户端发送请求的方式是 `GET` 提交时, `Servlet` 将调用 `doPost` 方法来处理请求.

6. 运行 `Servlet` 程序: `Run As --> Run on Server`

<img src="https://i.loli.net/2020/07/01/vfm4wJuH3ecDtkF.png"/>

发布  `web` 应用成功后 `console` 打印信息:

<img src="https://i.loli.net/2020/07/01/h6m9KiD1pNA4nFI.png"/>



#### 修改 Servlet 模板

**注:** 在实际开发中, 这样快捷生成的类中, 含有大量冗余信息, 也有我们所不期望的代码, 次次创建次次修改, 十分麻烦; 此时可以新建模板, 使用快捷提示模板来快速改变 `service` 相关代码, 使开发更敏捷.

模板代码如下: 

```java
package ${enclosing_package};

import java.io.IOException;
import javax.servlet.*;

/**
 * 
 */
public class ${primary_type_name} extends HttpServlet {
	private static final long serialVersionUID = 1L;
   
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 处理 POST 请求参数乱码
		request.setCharacterEncoding("utf-8");
		// 处理响应正文乱码
		response.setContentType("test/html;charset=utf-8");
		// TODO	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```



## Servlet 继承关系

<img src="https://i.loli.net/2020/07/01/a6Cpz7qU3erjkWF.png"/>

>  更多源码可以下载源码包进行查看
>
> [servlet_src.zip](docs/notes/resources)



## Servlet 拓展

### Servlet 调用过程

❓ 思考一下, 当浏览器提交请求后, 可以看到返回的结果, 这之间, `Servlet` 是如何被调用的, 又是如何执行的呢?

<img src="https://i.loli.net/2020/07/07/VJLEcAduZejMib8.gif"/>



### Servlet 生命周期

`Servlet` 在第一次被访问时创建 `Servlet` 实例, 创建之后服务器会立即调用 `init()` 方法进行初始化操作, 之后 `Servlet` 实例会一直驻留在服务器的内存之中, 为后续的请求服务.

只要有请求来访问这个 `Servlet` , 服务器就会调用 `service()` 方法来处理请求, 直到服务器关闭, 或者 `Web` 应用被移除出容器, 随着 `Web` 应用的销毁, `Servlet` 实例也会跟着销毁, 在销毁之前服务器会调用 `destroy` 方法进行善后的处理.



## Request & response

### 请求响应简介

服务器收到来自浏览器的请求后, 会调用 `Servlet` 的 `service()` 方法处理请求, 在调用 `service()` 之前, 会创建 `request` 对象 ( 用于封装 `http` 信息 ) 和 `response` 对象 ( 用于封装最后将要发送给浏览器的响应信息 ).

在执行 `service()` 处理请求的过程中, 如果要获取任何请求相关的信息, 可以通过 `request` 调用方法来获取; 如果有任何数据要发送给浏览器, 可以通过 `response` 对象进行发送.



## 会话技术

### Cookie & Session 会话技术

1. Cookie 的工作原理?
Cookie 是通过 Set-Cookie 响应头和 Cookie 请求头将会话中产生的数据保存在客户端, 是客户端的技术. 客户端向服务端发送请求, 服务器获取需要保存的数据, 并将需要保存的数据通过 Set-Cookie 响应头发送给浏览器, 浏览器会以 Cookie 的形式保存在浏览器的内部. 当客户端再次发送请求访问服务器, 服务器可以通过 Cookie 请求头获取上次发送给浏览器的 Cookie 信息, 通过这种方式可以保存会话中产生的数据. 
Cookie 旨在让每个客户端各自持有各自的数据, 只有需要时才带给服务器, 避免产生混乱.

2. 如何删除 Cookie?
由于在Cookie的API中没有提供直接删除Cookie的方法, 因此我们需要在服务端设置策略来间接提醒客户端删除Cookie.
首先明确一点, 浏览器是根据Cookie的名称、path、domain ( 后两者不设置时是默认相同的 ) 来区分Cookie的.
此时, 可以向客户端再发一个同名的Cookie并将其 Maxage ( 存活时间 ) 设置为 0, 浏览器识别到会误认为是用一个Cookie, 后接受的Cookie会覆盖之前发送的Cookie, 其属性是立即删除, 从而达到删除Cookie的效果.

3. Session 的工作原理?
Session 是将会话中产生的数据保存在服务端, 是服务端的技术.
浏览器第一次发送请求需要保存数据时, 服务端获取到需要保存的数据, 去服务器内部检查一下有没有为当前浏览器服务的 session, 如果有就直接拿过来用, 若没有, 就创建一个新的 session 并保存, 然后以 id 的方式响应给客户端. 当浏览器再去访问服务器时, 服务器可以从 session 中获取到该数据, 通过这种方式也可以来保存会话中产生的数据.

 4. Session 域对象的销毁?
a) 超时: 若 session 30min 没有被使用, 则会超时销毁;
b) 自杀: 当调用 session.invalidate() 方法时, 将立即销毁 session;
c) 意外身亡: 当服务器非正常关闭时, session 会立即销毁. ps: 服务器正常关闭时, session 将以文件的形式保存在服务器的 work 目录下, 该过程被称为 session 的钝化 ( 序列化 ). 服务器再次启动后, session 会恢复, 这个过程为 session 的活化 ( 反序列化 ).
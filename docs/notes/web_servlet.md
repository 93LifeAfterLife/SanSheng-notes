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

①. 





## Servlet 继承关系

<img src="https://i.loli.net/2020/07/01/a6Cpz7qU3erjkWF.png"/>



## Servlet 调用过程以及生命周期






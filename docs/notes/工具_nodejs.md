# :paperclip:Node.js

### :pencil2:node.js 简介

<img src="imgs_backup\nodejs.svg" style="zoom:25%; background: rgb(51,51,51)" />

- Node.js 就是运行在服务端的 JavaScript , 是一种环境; 基于 Google 的 V8 引擎, 执行效率高、性能稳定.



### :pencil2:node.js 安装与配置

windows 本地下载安装 Node.js : http://nodejs.cn/download/

其他平台 ( Linux、Ubuntu、CentOS、Mac OS ) 配置 Node 环境可参考: https://www.runoob.com/nodejs/nodejs-install-setup.html

在 cmd 中查看已安装的 Node.js 版本( 截止 *2020年5月14日18:58:54*, 最新版本为: **v14.2.0** ) :

```java
node -v
```



### :pencil2:创建第一个应用

- 使用 node.js 时, 我们不仅仅是实现一个应用, 同时还实现了整个 HTTP 服务器.

Node.js 应用由一下三个部分组成 :

1. **加载模块：**( node自带的http模块 ) 使用 **require** 指令来载入 Node.js 模块.
2. **创建服务器：**( http 模块提供的函数: createServer().listen(port...) ) 服务器可以监听客户端的请求.
3. **接收请求与响应请求**

示例如下:

```javascript
var firstApp = require('http');

firstApp.createServer(function (request, response) {
    // 发送 HTTP 头部: 包含状态码和内容类型
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 响应数据 "Hello Node.js"
    response.end('Hello Node.js\n');
}).listen(8888);

// node运行后终端, 如cmd, 将打印的信息
console.log('Server running at http://127.0.0.1:8888/');
```

保存为本地的 js 文件, 如 hello.js

使用 **node**命令执行该文件 :

```
node hello.js
```

终端会响应信息: Server running at http://127.0.0.1:8888/

访问以上的本地端口号, 将会看到一个写着 "Hello Node.js" 的网页.



### :pencil2:npm 简介

- npm ( node package manager for JavaScript ) 是 Node.js 附带的包管理工具, 允许用户从 npm 服务器下载他人编写的第三方包到本地使用, 也允许用户上传自己的应用到 npm 服务器供他人使用.

在 cmd 中查看 npm 是否安装成功 : 

```
npm -v
```

windows下 npm 升级指令 :

```java
npm install npm -g
// 使用淘宝镜像
// npm install -g cnpm --registry=https://registry.npm.taobao.org
```

npm 包的安装分为两种, 即本地与全局, 其中加上 -g 表示为 global 全局安装, 不加则是 local 本地安装.

通过 require( ) 赋值变量的方式来引入本地安装的包 :

```
// 本地安装某模块或第三方包
npm install <Module Name/Package Name>
// 在代码中只需要通过 require() 来加载模块就好, 无需额外配置与指定路径
var module = require('Module Name/Package Name')
```

注意 : 本地安装目录为 /node_modules/ .



### :pencil2:npm 常用指令

```java
npm help <command>                     // 查看某条指令的详细帮助 
npm list -g                            // 查看所有全局安装的模块 
npm list <Module Name> -g              // 查看指定模块的版本信息 -g 为全局
npm uninstall <Module Name>            // 卸载指定模块
npm ls <Module Name>      			   // 查看包是否存在
npm search <Module Name>               // 搜索模块
npm update <Module Name>               // 更新模块
npm cache clear                        // 清空本地缓存
```

自定义( 个人 )模块的创建与发布 :

```java
npm init                               // 初始化一个 node 模块
npm adduser                            // 在 npm 资源库中注册用户
npm publish                            // 发布模块
npm unpublish <Module Name>@<Version>  // 撤销发布指定版本模块
```



### :pencil2:npm 版本号

- npm 使用语义版本号来管理代码, 其代表含义为 : ***主版本号.次版本号.补丁版本号***

仅修复bug, 只需更新**补丁版本号**; 新增功能但能向下兼容, 则更新**次版本号**; 若发生大变动, 不能向下兼容, 则**更新主版本号**.


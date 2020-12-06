CORS:

```js
// 设置跨域和相应数据格式

app.all('/api/*', function(req, res, next) {

 res.header('Access-Control-Allow-Origin', '*')

 res.header('Access-Control-Allow-Headers', 'X-Requested-With, mytoken')

 res.header('Access-Control-Allow-Headers', 'X-Requested-With, Authorization')

 res.setHeader('Content-Type', 'application/json;charset=utf-8')

 res.header('Access-Control-Allow-Headers', 'Content-Type,Content-Length, Authorization, Accept,X-Requested-With')

 res.header('Access-Control-Allow-Methods', 'PUT,POST,GET,DELETE,OPTIONS')

 res.header('X-Powered-By', ' 3.2.1')

 if (req.method == 'OPTIONS') res.send(200) /*让options请求快速返回*/ 

 else next()

})
```

## 在HTTP请求的header头里面，为什么有的时候有X-Powered-By这个值，有的时候没有呢？

这个不是Apache或者Nginx输出的，而是由语言解析器或者应用程序框架输出的。
这个值的意义用于告知网站是用何种语言或框架编写的。例如：
PHP标准输出是：X-Powered-By: PHP/5.2.1，可在php.ini中增加或修改 expose_php = Off关闭。
而使用了ThinkPHP，会输出：X-Powered-By: ThinkPHP 2.0，可修改相关类文件关闭
用.net会输出：X-Powered-By:ASP.NET，可修改web.config 删除

nginx编译的时候可以增加一个模块，HttpHeadersMore，用来统一删除或修改返回的http header。

另外网页服务器本身也会吐出自己的版本号，http header是Server:xxxxx，这个有时会造成有人专门利用特定版本网页服务器漏洞进行攻击，nginx可以在配置文件中增加或修改server_tokens off 来去除版本号。

## 1、隐藏X-Powered-By

  修改 php.ini 文件。添加或修改 expose_php = Off

 

## 2、apache 隐藏 server

  修改httpd.conf 设置 

  ServerTokens Prod

 

## 3、nginx 隐藏 server

  修改nginx.conf 在http里面设置 

  server_tokens off;

修改完之后，重启 Apache（nginx 下也要重启 php-fpm）即可看到效果



## 注意:用 chrome 的调试工具中的 network 就可以查看
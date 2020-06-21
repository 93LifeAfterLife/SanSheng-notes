# HTML 5

## 浏览器向服务器发送数据的两种方式

1. 通过超链接

```html
url/?user=sansheng&password=123123
```

拼接参数提交给服务器.

2. 通过表单

```html
<form action="#" method="POST"></form>
```

  最后还是通过输入的值拼接参数提交给服务器.	

​	  2.1. `action` , 必须属性, 表示指定表单提交的目标 url 

​	  2.2. `method` 用来指定提交方式, 默认是 GET 提交
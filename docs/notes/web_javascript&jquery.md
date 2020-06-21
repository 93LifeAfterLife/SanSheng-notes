# JavaScript

## JS 简介

JS 是一门基于对象和事件驱动的脚本语言, 被用于 web 应用开发, 为网页交互而设计. JS 是一门直译而且弱类型的语言, 直译的意思是可以一边解释一边执行源代码, 不像 Java 那样需要编译成 .class 文件才可以运行. JS 弱类型, 声明变量不需声明是何种变量, 即 JS 的变量可以指向任何类型. 

JS 的优势在于交互、安全 ( 限制只能访问浏览器内部资源, 无法直接访问硬盘 ) 和跨平台性 ( 有浏览器就可以执行 ).



## JS 入门

### 案例一 : 灯泡

无论是 W3school 还是 RUNOOB 上都用一个点亮灯泡的案例作为入门 JS 的尝试.

我们也不妨来试试这个简单的案例, 首先在以下地址中保存两张灯泡的 gif 图. 然后在图片的同一目录下新建 bulb.html 文件, 尝试以下代码.

p.s.个人比较喜欢使用 VScode 作为前端开发 IDE.

代码地址 ( 图片来源互联网 ) 位于 : 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Light up a bulb!</title>
</head>
<body>
    <img id="bulb" onclick="lightUp(this)" src="b01.gif" alt="Sorry, bulb image not found" title="Click to open or close">
    <script>
        /* */
        function lightUp(e) {
            e.src = e.src.match('b02')?"b01.gif":"b02.gif";
        }
    </script>
</body>
</html>
```

在浏览器中打开, 点击灯泡查看效果, 这就是 JS 的一项功能, 捕捉属性, 变更属性, 然而代码量却十分小.



### 案例二 : 计算器

```html

```

 

### 案例三 : 简单函数

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JS - 简单函数</title>
    <style>
        #result {
            border: 1px solid #999;
            width: 500px;
            /* 自动换行 */
            height: auto;
            word-wrap: break-word;
            word-break: break-all;
            text-align: left;
        }
    </style>
    <script>
        function myFn() {
            var x = document.getElementById("x").value;
            var y = document.getElementById("y").value;
            var arr = [];
            if (x <= y) {
                for (let i = x, j = 0; i <= y; i++) {
                    if (i % 3 == 0) {
                        arr[j] = i;
                        j++;
                    }
                }
            } else {
                for (let i = y; i <= x; i++) {
                    if (i % 3 == 0) {
                        arr[j] = i;
                        j++;
                    }
                }
            }
            var result = document.getElementById("result");
            result.innerHTML = arr;
        }
    </script>
</head>

<body>
    <div>请输入两个整数, 然后遍历打印之间的所有 3 的倍数</div>
    <div>输入x :
        <input type="text" id="x">
    </div>
    <div>输入y :
        <input type="text" id="y">
    </div>
    <div>
        <input type="button" value="点击输出结果" id="output" onclick="myFn()"> 结果为: </input>
    </div>
    <div id="result"></div>
    <div>
        <input type="reset" onclick="history.go(0)">    <!-- Browser对象之history对象 -->
    </div>
</body>
</html>
```



### 案例四 : 

## JS 语法

### 数据类型

1. 基本数据类型

JS 的基本数据类型共有五种: 数值类型、字符串类型、布尔类型、undefined 和 null

​	①. 数值类型

​	JS 数值底层都是浮点型, 但是会在整型间自动转换, 例如 : 

```js
1.1+1.9
3
```

​	②. 字符串类型

​	用单引号或者双引号包裹起来.

​	③. undefined

​	当声明了变量但是并没有赋值时, 变量的值为 undefined ( 与 Java 不同, Java 中未赋值初始化就使用变量对象, 会报错 )

```javascript
var i;
console.log(i);
undefined
```

2. 复杂数据类型 ( 主要指对象, JS 中, 数组、函数也属于对象 )

### 变量与运算符

1. 声明变量 ( const, var, let, 其中 const 与 let 为 ES6 标准下的新特性)

   ①. const 定义的为常量, 必须初始化, 不可再修改

   ```javascript
   const a = 1;
   undefined
   alert(a)
   undefined
   a=3;alert(a);
   Uncaught TypeError: Assignment to constant variable.
   at <anonymous>:1:2
   ```

   ②. var 定义的变量可以修改, 若不初始化则输出为 undefined, 不会报错, 特别注意的是, var 声明的变量在windows作用全局, 可能造成全局污染.

   ③. let 是块级作用域, 函数内部使用 let 定义后, 对函数外部没有影响

   一个经典例子: 

   ```javascript
   var a = [];
   for(var i=0; i<10; i++){
       a[i] = function(){
           console.log(i);
       }
   }
   a[6]();	// 10
   ```

   改用 let :

   ```javascript
   var a = [];
   for(let i=0; i<10; i++){
       a[i] = function(){
           console.log(i);
       }
   }
   a[6]();	// 6
   ```

   > 参考阮一峰老师的 ES6 教程 :
   >
   > ​	 https://es6.ruanyifeng.com/#docs/let

2. 运算符

与 Java 中大致相同. 不再赘述



# jQuery

## jQuery 简介

JavaScript Query , 是免费开源的函数库, 而且十分轻量, 能够极大地简化 JS 代码, 它最突出的特点就是可以像 CSS 选择器般方便地获取页面元素并且操作 CSS 属性来达到各种各样的网页效果.

使用它需要引入 JS 文件, 因为它本身就是一个函数库, 要使用它的函数必然需要引入 JS 文件.

>  jQuery 官网: https://jquery.com/
>
> 目前最新的压缩开发版为 3.5.1
>
> 你也可以访问我的 github 备份, 获取 3.4.1 的 jQuery 文件 : ( 地址 )



## jQuery 入门

### 案例一 : 模拟好友列表




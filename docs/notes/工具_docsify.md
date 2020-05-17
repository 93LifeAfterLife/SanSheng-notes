#  📔Docsify

### ✏️前言

作者 : 叁昇 ( 93LifeAfterLife ) 	[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)
公众号:

<div align="center"> <img src="https://i1.fuimg.com/719027/ca0c1d25208ae899.jpg" width="350px"> </div><br>纠错请联系工作邮箱 : tangdingjnust@163.com



###  ✏️Docsify简介

<div align="center"> <img src="https://i1.fuimg.com/719027/5cf1f2292bcaedb7.png" width="150px"> </div><br>

Docsify 是一个动态生成文档网站的工具, 它会在运行时将 .md 文件转为 .html 文件, 而非静态生成方式 ( 如GitBook、Hexo ). 这样的好处在于, 无需生成大量 .html 文件污染 commit 记录, 仅需要创建一个 index.html 即可开始专注于文档的编写, 并直接部署在 GitHub Pages.

#### 🔥特征

- 当存在 .md 文档时, 不再冗余地生成静态 html
- 轻量级
- 拥有一个智能的全文检索插件
- 多主题
- 支持 **' :<emoji>: '** 格式的表情符号 ( 你可以在这里查询Emoji代码 : https://www.webfx.com/tools/emoji-cheat-sheet/ ) 
- 支持服务器端渲染
- 兼容 Internet Explorer 11



### ✏️快速开始

建议使用 npm 全局安装, 更利于本地初始化和预览网站 :

```
npm i docsify-cli -g
```

<div align="center"> <img src="https://i2.tiimg.com/719027/f397cda9c95d4700.png" width="350px"> </div><br>

查看已安装的 docsify 客户端版本 ( 截止*2020年5月14日*, 最新版为 v4.4.0 ) :

```
npm list docsify-cli -g
```

<div align="center"> <img src="https://i2.tiimg.com/719027/bb2ebe8a3e615215.png" width="350px"> </div><br>

:point_right: 如果你还不够了解 **npm** , 可以查看我的文档 [npm]( Nodejs入门.md )



### ✏️初始化

没错, 熟悉 npm / git 的你, 可以轻松知道使用 init 命令来进行初始化 : 

```java
docsify init ./docs              // 在 ./docs 子目录下编写文档, 进行初始化
```

此命令会在本地创建 docsify 的文件仓库.

<div align="center"> <img src="https://i2.tiimg.com/719027/997e7e256155bbbe.png" width="350px"> </div><br>

- `index.html` : 入口文件
- `README.md` : 主页
- `.nojekyll` : 优化文件, 防止GitHub Pages忽略以 "下划线_" 开头的文件

至此, 已经可以很轻松地在 docsify 仓库的 `./docs/README.md` 下更新文档了. 



### ✏️预览网站

docsify 服务器的默认监听端口号为 3000

```
docsify serve docs
```

在浏览器中打开地址 : http://localhost:3000/ 

<div align="center"> <img src="https://i2.tiimg.com/719027/fc45a89197f7fcf8.png" width="350px"> </div><br>



### ✏️加载对话框

为了增强访问游客的体验性, 可以选择给枯燥的加载等待撒上一点趣味的"粉末" —— loading界面.

### ✏️结语

感谢你的查阅, 欢迎 star 我的 github 开源仓库! 或者关注我的个人公众号! 

[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)

<div align="center"> <img src="https://i1.fuimg.com/719027/ca0c1d25208ae899.jpg" width=""> </div><br>

### ✏️更新补充 & FAQ


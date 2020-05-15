#  📔 项目管理工具-Git

###  ✏️ Git 简介

<div align="center"> <img src="https://i2.tiimg.com/719027/d03afd2c69189c17.png" width=""> </div><br>

Git 是一个免费开源的分布式版本控制系统，旨在快速高效地处理不管是大型还是小型的项目的所有内容。

#### 🔥 优势

- **分支与合并** 

  Git允许并鼓励您拥有多个彼此完全独立的本地分支。

- **轻型而敏捷** 

  Git是为在Linux内核上工作而构建的，这意味着从第一天开始，它就必须有效地处理大型存储库。Git用C编写，减少了与高级语言关联的运行时的开销。从一开始，速度和性能就一直是Git的主要设计目标。

- **分布式**

  最大的好处就是" 多重备份 ", 每个用户基本上都具有主服务器的完整备份。在崩溃或损坏的情况下，可以将这些副本中的每个副本向上推以替换主服务器。实际上，除非只有存储库的单个副本，否则Git不会有单点故障。

  其次是" Integration Manager工作流 ".

- **数据保障**

  Git使用的数据模型可确保项目每一位的加密完整性。每个文件和提交都经过校验和，并在检出时通过其校验和进行检索。除了您输入的确切数据位之外，从Git中取出任何东西都是不可能的。

#### 🔰 Git 与 SVN 的区别

1. Git 是分布式的, 而SVN不是;
2. Git 将内容按元数据的方式存储, 而SVN是按文件;
3. Git 的分支概念与 SVN 的分支概念不同, SVN的分支仅仅是一种目录而已;
4. SVN 有全局版本号, 而 Git 没有;
5. Git 的数据安全性 / 内容完整性要优于 SVN. 

<div align="center"> <img src="https://i2.tiimg.com/719027/0d833fda6aaef0f9.jpg" width=""> </div><br>

​																				*图片转载自菜鸟教程*

------



### ✏️ Git 安装与配置

下载地址: https://git-scm.com/downloads

CentOS 下使用 `yum` 的安装命令如下:

```
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel              				  // 安装依赖
$ yum -y install git-core			// 安装 git
$ git --version 					// 查看当前安装的 git 版本
```

#### 🙎 配置用户信息

配置个人的用户名和电子邮件地址 ( `--global` 代表主目录下的配置文件 )

```
$ git config --global user.name "93LifeAfterLife"
$ git config --global user.email tangdingjnust@163.com
```

查看配置信息:

```
$ git config --list					// 查看所有的配置文件信息
$ git config user.name 				// 查看指定的配置信息
```

------



### ✏️ Git 创建仓库

如何创建一个 Git 仓库呢? 主要有两种方式 init 和 clone:

#### ✏ Git init

使用一个已经存在的本地目录作为 Git 仓库, 只需要在当前目录下利用 Git Bash ( windows系统 ) 执行如下初始化指令: 

```
$ git init
```

#### ✒ Git clone

如果已经有现有的 Git 仓库, 可以使用 clone 指令来拷贝项目: 

```
$ git clone <repo>							// 拷贝到当前目录下
$ git clone <repo> <directory>				// 拷贝到指定的目录下
```

`repo` 代表 Git 仓库

**例子,** 

选取一个我自己学习 `RabbitMQ` 消息队列的项目, 该项目地址为:

> https://github.com/93LifeAfterLife/rabbitmq

假如我要拷贝此项目到本地目录下进行管理, 有如下几种协议方式, 你可以在 GitHub 的这里看到:

<div align="center"> <img src="https://i2.tiimg.com/719027/a309ac7c0f24e23a.png" width=""> </div><br>

```
$ git clone git@github.com:93LifeAfterLife/rabbitmq.git		   	// SSH 协议
$ git clone https://github.com/93LifeAfterLife/rabbitmq.git     // HTTPS 协议
$ git clone git://github.com/93LifeAfterLife/rabbitmq.git		// Git 协议
```

最常用的首推 SSH, 因为速度快, 还可以预先配置公钥免输入密码.

------



### ✏️ Git 仓库

git 把需要用来进行版本控制的本地文件目录称作为一个 `仓库`( repository ), git 能在当前仓库中持续跟踪并记录所有发生的操作.

你也可以看到在初始化 `repo` 或者从远程克隆 `git clone <仓库地址> `后, 在本地文件夹中会新增一个隐藏的 `.git`文件夹. 这个文件夹十分重要, 代表了 git 对于本地仓库的控制信息.

#### ⛪ Git 仓库的结构

<div align="center"> <img src="https://i1.fuimg.com/719027/67189a824cb86208.png" width=""> </div><br>

说明:

- `Directory` : Git 所管理的目录 / 仓库, 包含工作空间和Git管理空间
- `Workspace `: 需要通过 Git 管理的目录和文件
- `.git `: 存放 Git 管理信息的目录, 初始化仓库时自动创建
- `Stage `: 暂存区, 或叫待提交更新区, 在提交之前, 可以先把所有的更新放置在这里
- `Local Repo `: 本地仓库, 拥有许多的 Branch 分支, 其中的一个就是本地的某一版本库
- `Stash` : 工作状态保存栈, 用来保存和恢复工作空间中的临时状态

------



### ✏️ 简单的代码提交流程

大多数的情况下, 单个的简易仓库的提交更新操作可以使用如下流程: 

1.  查看工作区`Workspace`代码相对于暂存区`Stage`的差别:

```
git status
```

2.  将当前目录下修改的所有信息从工作区`Workspace`添加到暂存区`Stage`, `.` 代表当前目录

```
git add .
```

3.  将暂存区的代码提交到本地版本库`Local Repo`, 注意加上 `-m` 表示提交的信息

```
git commit -m "<commit message>"
```

4.  将本地版本库`Local Repo` 推送到远程服务器, `origin` 代表远程主机 ,`master` 代表远程服务器上的 `master` 分支, 当然分支名是可以按照需求来修改的.

```
git push origin master
```

<div align="center"> <img src="https://i2.tiimg.com/719027/0ca8d7dc8faa0e1b.png" width=""> </div><br>

如上图所示, 即是我更新当前文档时的 Git 代码提交流程.

------



### ✏️ Git add

Git 的管理方式就如同快照一样, 它的工作就是将前后的快照进行对比, 然后按照指令进行操作.

add 命令可以将文件添加到暂存区 `Stage` 

在添加之前, 可以选择先查看项目的当前状态, 使用 status 命令:

```
$ git status								// 查看当前状态
$ git status -s								// 以精简的方式显示文件状态
$ git status --short						// 同 -s
```

####   ❔ 关于 status -s

标记符占两个位置, 第一个位置表示 `Stage` 状态, 第二个位置表示 `Workspace` 状态

- 新添加的未跟踪文件前面有 		`??` 标记
- 新添加到暂存区的文件前面有     `A[]` 标记
- 修改过的文件前面有                    `[]M` 或 `M[]` 标记
  -  M标记有两种, 一种是靠左, 即占第一个位置, 表示: 该文件被修改了且已经放入了暂存区.
  -  另一种是靠右, 即占第二个位置, 表示: 该文件被修改了但是还未放入暂存区.

如下图:

<div align="center"> <img src="https://i2.tiimg.com/719027/e3ff5c8124abb5ec.png" width=""> </div><br>

M 标记在第二个位置, 表示 `工具_git.md` 文档被修改了, 但是还没有被放入暂存区

?? 标记的两幅 png 格式图片, 则是新添加进工作区的文件.

#### ➕ add

向暂存区添加文件:

```
$ git add <filename>
```

<div align="center"> <img src="https://i2.tiimg.com/719027/06e22832eee81584.png" width=""> </div><br>

add 后的文件将进入暂存区, 显示为 第一个位置的 A.

若此时再次修改该文件的内容, 然后再查看状态, 则会显示为 `AM` 标记, 表示已在缓存区, 但是工作区的文件已经发生了改变. 

往往我们不需要过于细致的 add, 可以选择直接 add 所有的改动:

```
$ git add .
```

------



### ✏️ Git commit

`add` 指令将工作区快照内容存入缓存区, `commit` 指令则将缓存区的内容添加到仓库 `Local Repo` 中

Git 在每一次提交 `commit` 时都会记录用户的账号和邮箱地址, 所以使用 `commit` 前需配置 `user.name` 和 `user.email`. 然后执行提交:

```
$ git commit -m "type your commit message"
$ git commit -am "type your commit message"  			// 跳过缓存处理
```

 **注意 :** 不添加 `-m` 的消息时, 会进入一个 `vim` 界面, 要求用户填写消息, 可以使用 `:wq` 指令来退出!

------



### ✏️ Git rm & mv

rm 删除同 Linux 指令, 为了安全, 可以不了解; 而且日常工作中有远程终端软件来修改与删除文件, 更加的透明, 而且避免了不必要的 rm 错误导致文件无法找回.

------



### ✏️ Git push

type here

------



### ✏️ Git diff

上面的 `status` 旨在查看文件变化, 而更加详细到文件内代码的变化, 则需使用 `diff` 指令

- 尚未缓存的改动 :

```
$ git diff
```

- 查看已缓存的改动:

```
$ git diff --cached
```

- 查看已缓存的与未缓存的全部改动: 

```
$ git diff HEAD
```

- 显示摘要而非整个`diff`:

```
$ git diff -stat
```

<div align="center"> <img src="https://i1.fuimg.com/719027/9b1e0a86746659ab.png" width=""> </div><br>

------



### ✏️ Git 分支管理

Git 最大的特色莫过于它的分支模型.

查看当前分支:

```
$ git branch
```

创建分支命令:

```
$ git branch <branch name>
$ git branch -b <branch name>				// 创建分支并立即切换到新建的分支下
```

切换分支命令:

```
$ git checkout <branch name>
```

合并分支命令:

```
$ git merge
```

不同分支代表了不同的工作环境, 不同的分支各自的修改操作都相当于是独立的, "不共享的". 所以我们才能在不同时间下的不同开发环境中修改代码, 并来回切换, 进行版本控制!

删除分支命令: 

```
$ git branch -d <branch name>
```

当我们在其中之一的分支中修改了代码, 而且希望它能合并到主分支中去, 可以使用 `merge` 指令.

当然, 这时候就存在**合并冲突**问题.

#### ❗ 合并冲突

如下模拟的案例在生产环境中时常可见, 当团队中存在多个分支对同一文件的同一处代码块进行了不同的修改时, 将这些分支合并到一起时, 就会发生合并冲突问题. 解决的方法就是查看冲突的代码块, 一一进行手动修改, 来解决分支合并中引发的冲突!

#### ❕ 解决冲突案例


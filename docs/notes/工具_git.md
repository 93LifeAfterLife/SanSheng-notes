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

<div align="center"> <img src="https://i2.tiimg.com/719027/0d833fda6aaef0f9.jpg" width="350px"> </div><br>

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

<div align="center"> <img src="https://i2.tiimg.com/719027/a309ac7c0f24e23a.png" width="350px"> </div><br>

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

<div align="center"> <img src="https://i1.fuimg.com/719027/67189a824cb86208.png" width="350px"> </div><br>

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

<div align="center"> <img src="https://i2.tiimg.com/719027/0ca8d7dc8faa0e1b.png" width="350px"> </div><br>

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
  
- 更多状态码如下:

`A` : 你本地新增的文件（服务器上没有）.

`C` : 文件的一个新拷贝.

`D` : 你本地删除的文件（服务器上还在）.

`M` : 文件的内容或者mode被修改了.

`R` : 文件名被修改了。

`T` : 文件的类型被修改了。

`U` : 文件没有被合并(你需要完成合并才能进行提交)。

`X` : 未知状态(很可能是遇到git的bug了，你可以向git提交bug report)。



常见的状态码, 如下图:

<div align="center"> <img src="https://i2.tiimg.com/719027/e3ff5c8124abb5ec.png" width="350px"> </div><br>

M 标记在第二个位置, 表示 `工具_git.md` 文档被修改了, 但是还没有被放入暂存区

?? 标记的两幅 png 格式图片, 则是新添加进工作区的文件.

#### ➕ add

向暂存区添加文件:

```
$ git add <filename>
```

<div align="center"> <img src="https://i2.tiimg.com/719027/06e22832eee81584.png" width="350px"> </div><br>

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

<div align="center"> <img src="https://i1.fuimg.com/719027/9b1e0a86746659ab.png" width="350px"> </div><br>

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

在 `tests \ git_test` 文件夹下新建一个 `merge_clash.md` 文件

```
$ touch merge_clash.md					// 新建文件
$ ls									// 查看当前目录下的所有文件
 merge_clash.md
$ vim merge_clash.md					// 修改文件
 // 插入一行一级标题: # Title:Merge clash, 然后esc :wq 保存退出
$ cat merge_clash.md					// 查看文件
 # Title:Merge clash
$ git branch							// 查看当前所在分支
 * master								// 当前处于 master 分支,*选中的表示当前所在分支
$ git checkout -b testing				// 创建并进入 testing 分支
 Switched to a new branch 'testing'		
$ vim merge_clash.md					// 在新分支中修改文件
 // 新增一行二级标题: ## testing
$ cat merge_clash.md
 # Title:Merge clash
 ## testing
$ git commit -am 'changed the .md file at branch testing'		// 快捷提交更改
 [testing 1a7e1f8] changed the .md file at branch testing
  1 file changed, 23 insertions(+)
$ git checkout master					// 回到 master 分支
 Switched to branch 'master'
 // 切换回 master 分支发现, master 还是原样, 只有一行一级标题
$ cat merge_clash.md
 # Title:Merge clash
$ vim merge_clash.md 					// 重新回到 master 分支修改文件
 // 新增一行三级标题: ### master
$ cat merge_clash.md
 # Title:Merge clash
 ### master
$ git diff
 // 可以看到变化
$ git merge testing						// 在当前的 master 分支下合并 testing 分支
 Auto-merging merge_clash.md
 CONFLICT (content): Merge conflict in runoob.php
 Automatic merge failed; fix conflicts and then commit the result.
 // 表示存在 merge conflict 合并冲突, 并且在 merge_clash.md 中自动标注了冲突内容
$ cat merge_clash.md
 <<<<<<< HEAD
 ### master
 =======
 ## testing
 >>>>>>> testing
$ vim merge_clash.md 					// 手动修改冲突, 将两行代码合并
$ cat merge_clash.md
 # Title:Merge clash
 ## testing
 ### master
 // 如上, 手动解决了冲突代码
$ git status -s
 UU merge_clash.md						// 表示文件没有被合并
$ git add merge_clash.md				// 使用 add 告诉 Git 文件冲突已经解决, 并存入到暂存区中
$ git status -s
 M  merge_clash.md
$ git commit
 [master 88afe0e] Merge branch 'testing'
 // 成功解决了合并中的冲突, 并提交了更改
$ git branch -d testing					// 删除不再使用的测试 testing 分支
 Deleted branch testing (was bf0e015).
```

------



### ✏️ Git 日志

回顾提交历史, 使用 `log` 指令:

```
$ git log								// 紧接着使用 q 退出当前日志
```

<div align="center"> <img src="https://i1.fuimg.com/719027/60f7add1eb1423f8.png" width="350px"> </div><br>

```
$ git log --oneline						// 查看历史记录的简洁版本
```

<div align="center"> <img src="https://i2.tiimg.com/719027/b8d7463db3eb3300.png" width="350px"> </div><br>

在此, 整个项目的开发历史, 一目了然!

还可以使用 --graph ( 开启拓扑图 ) 选项, 查看项目中的分支、合并情况

```
$ git log --graph
```

<div align="center"> <img src="https://i2.tiimg.com/719027/4196b07c17d402f4.png" width="350px"> </div><br>

- 更多的 `log` 参数

  - `--reverse` : 逆向显示所有日志

  ```
  $ git log --reverse --oneline
  ```

  - `--author` : Git 源码中 <user.name> 所提交的部分

  ```
  $ git log --author=93LifeAfterLife --oneline -5  			// 显示五行我自己提交的部分
  ```

  - `--decorate` : 可以查看标签
  - 更多杂项可以参考 : https://git-scm.com/docs/git-log

------



### ✏️ Git 版本标签

如果在项目构建过程中, 达到一个重要的阶段, 并希望记录下当前的提交快照, 可以将这个快照称作一个 "版本", 这时, 可以使用 `tag` 给这份快照打上标签.

```
$ git tag -a v1.0.0						// -a 表示创建一个带注解的标签, 推荐使用注解!
$ git log --decorate --oneline --graph 	// 以简约格式查看带有标签的分支合并拓扑图
$ git tag 								// 查看所有标签
$ git tag -d v0.9.0						// 删除标签
$ git show v1.0.0						// 查看此版本所修改的全部内容
```

如果忘记给某个提交打上标签, 可以追加标签:

```
$ git tag -a v1.0.1 3e92c19				// 3e92c19 是 commit 号, 用标签来代替, 便于寻找
```

由此可见, `tag` 实际上就是指向某个 `commit` 的指针, `tag` 是无法移动的, 一旦创建或删除都是立刻执行的!

------



### ✏️ Git 远程仓库

如上所有的 Git 操作都是在本地的客户端内进行的, 如果你想通过 Git 来分享代码或者与团队协作, 则还需要将仓库放到所有人或特定成员都能访问到的服务器端. 比较常用的远程仓库有 `GitHub` 和 `Gitee` ( 码云 ).

#### 🐱 GitHub

首先, 确保已注册了一个 `GitHub` 账号, 并且将本地 Git 仓库和远程 GitHub 仓库通过SSH协议相连通:

1. GitHub 官网: https://github.com/ , 使用邮箱来注册账号, 确保邮箱是安全的, 牢记这个邮箱;
2. 配置验证信息:

```
$ ssh-keygen -t rsa -C "tangdingjnust@163.com"
```

3. 之后会要求确认路径和输入密码, 可以一直回车, 成功就会在`~/` 根目录下 ( windows系统则在 `c/Users/Administrator/` ) 生成 `.ssh` 文件夹, 里面会创建一个 `id_rsa.pub` 文件, 编辑该文件, 复制 `key` ; 回到 github 网站, 在账户 ( `account` ) 下的设置 ( `setting` ) 里

   <div align="center"> <img src="https://i2.tiimg.com/719027/bca75b9f97e3132d.png" width=""> </div><br>

   

   选择 `SSH and GPG keys`, 点击 `New SSH key` 按钮, `title` 随意填写, `key` 为刚才所复制的, 就完成了本地与远程的连接验证了. 当然, 你可以多台客户端同时申请连接, 为自己布下工作网.

<div align="center"> <img src="https://i2.tiimg.com/719027/ea1de85a0004ac10.png" width="350px"> </div><br>

4. 验证是否成功: 

```
$ ssh -T git@github.com
 // 设置好 ssh 后可以看到以下信息
 Hi 93LifeAfterLife! You've successfully authenticated, but GitHub does not provide shell access.
```

##### ✈ Git远程操作

- `remote` 查看远程仓库

```
$ git remote
 origin
$ git remote -v				// 查看实际链接地址以及他们的别名 ( fetch, push )
 origin  https://github.com/93LifeAfterLife/SanSheng-notes.git (fetch)
 origin  https://github.com/93LifeAfterLife/SanSheng-notes.git (push)
```

- `fetch & merge` 提取远程仓库

<div align="center"> <img src="https://i2.tiimg.com/719027/5d5d57810062b127.png" width="350px"> </div><br>

1. 在此之前, 我提前在 github 下新建一个仓库 `New repository`, 名称为 `test-git`, 其他选项保持默认. 点击 `create repository`, 会进入引导页面, 引导用户如何拉取到本地

<div align="center"> <img src="https://i2.tiimg.com/719027/5a70003fd8ff0296.png" width="350px"> </div><br>

2. 在本地选择一个仓库目录, 创建本地仓库, 并将本地库中所修改的文件提交到远程仓库中

<div align="center"> <img src="https://i2.tiimg.com/719027/df2891ba6ec90098.png" width="350px"> </div><br>

所有代码如下, 你可以快捷复制进行操作:

```
$ mkdir HelloGit
$ cd HelloGit
$ echo "# Title: git remote testing" >> README.md
$ ls
$ git init
$ git add README.md
$ git commit -am "2020年5月16日09:54:09 添加一个说明文件"
$ git remote add origin git@github.com:93LifeAfterLife/test-git.git    # 填写自己的仓库, 使用 SSH 协议连接方式
$ git push -u origin master
```

3. 刷新页面, 查看 github 上仓库的变化, 会发现已然通过本地 Git 进行了远程仓库的更新了!

<div align="center"> <img src="https://i2.tiimg.com/719027/1fe45858709dc2c4.png" width="350px"> </div><br>

4. 直接在 GitHub 上在线修改代码, 点击右上角的 '✏' ( 编辑 ) 按钮

​       加上一行 '## 在github上在线修改了文件'

5. 再在本地来更新

```
$ git fetch origin
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 693 bytes | 22.00 KiB/s, done.
From github.com:93LifeAfterLife/test-git
   00a08e2..8bfb92b  master     -> origin/master
```

  **注意: **`master -> origin/master` 表示从名为 `origin` 的远程上拉取名为 `master` 的分支, 到本地分支 `origin/master` 中! 需要严格指明远程名! 当然, 可以一次性拉取多个分支代码!

```
$ git merge origin/master
Updating 00a08e2..8bfb92b
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
 
$ cat readme.md
# Title: git remote testing
## 在github上在线修改了文件
```

  **注意: ** `merge origin/master` 表示合并名为 `origin/master` 的分支到当前所在分支, 与远程名无关, 需要指定的是被合并的分支! 当然, 可以一次性合并多个分支代码!



####  ⛅ Gitee

同理于 Gihub , 操作完全类似! 更适合国内环境, 而且5人以下团队的代码托管完全免费!

你可以访问: https://gitee.com/signup?from=homepage , 注册使用.



####  🏰 搭建自己的私有服务器

以 `CentOS` 所包装的 `Linux` 系统为例, 首先确保已然安装 Git 依赖与 Git 客户端. 

目前我并没有个人搭建过 git 服务器, 使用的都是既定的服务器. 以下流程来源于菜鸟教程, 供参考学习

```
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
$ yum install git
```

1. 创建一个 git 用户组和用户, 用来运行 git 服务

```
$ groupadd git
$ useradd git -g git
```

2. 创建证书登陆

收集所有需要登录的用户的公钥，公钥位于 `id_rsa.pub` 文件中，把我们的公钥导入到 `/home/git/.ssh/authorized_keys` 文件里，一行一个。

如果没有该文件创建它：

```
$ cd /home/git/
$ mkdir .ssh
$ chmod 755 .ssh
$ touch .ssh/authorized_keys
$ chmod 644 .ssh/authorized_keys
```

3. 初始化 git 仓库

首先我们选定一个目录作为Git仓库，假定是 ` /home/gitrepo/runoob.git` ，在 `/home/gitrepo` 目录下输入命令：

```
$ cd /home
$ mkdir gitrepo
$ chown git:git gitrepo/
$ cd gitrepo

$ git init --bare runoob.git
Initialized empty Git repository in /home/gitrepo/runoob.git/
```

以上命令Git创建一个空仓库，服务器上的 Git 仓库通常都以 `.git` 结尾。然后，把仓库所属用户改为git：

```
$ chown -R git:git runoob.git
```

4. 克隆仓库

```
$ git clone git@192.168.45.4:/home/gitrepo/runoob.git
Cloning into 'runoob'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
```

192.168.45.4 为 Git 所在服务器 ip ，你需要将其修改为你自己的 Git 服务 ip。

这样我们的 Git 服务器安装就完成 !



### ✏️ 结语

感谢你的查阅, 欢迎 star 我的 github 开源仓库! 或者关注我的个人公众号! 

[![stars](https://badgen.net/github/stars/93LifeAfterLife/SanSheng-notes?icon=github&color=4ab8a1)](https://github.com/93LifeAfterLife/SanSheng-notes)

<div align="center"> <img src="https://i1.fuimg.com/719027/4d889d6db54526a1.png" width=""> </div><br>

------



### 🚦 更新补充 & FAQ

#### 👉 git clone 速度太慢?

// To do 参考: https://www.jianshu.com/p/3f6477049ece 更改 DNS 的方法

#### 👉 STS / Eclipse & IDEA 中的 Git 操作

// To do 实际应用更多, 视图化操作更便捷
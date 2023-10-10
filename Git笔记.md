## Git

**[git 学习的图形化交互式网站](https://learngitbranching.js.org/?locale=zh_CN)**(**强烈推荐！！！**)

### Git 命令

#### 配置命令

`git config [options]`：配置或读取环境变量

- _options:_
  - `--system` ： 所有用户配置`/etc/gitconfig`
  - `--global` ：当前用户配置`~/.gitconfig`
  - `无` ：当前项目配置`.git/config`
  - `--list` ：检查已有配置

```java
//每一个级别的配置都会覆盖上层的相同配置
```

###### !必须的初始配置!(下载安装 git 后的优先操作)

> 用户信息
>
> > \$ git config --global user.name "your_username"
> > \$ git config --global user.email your_email@example.com

删除配置的用户信息：`$ git config --global --unset user.email`

#### 底层（Linux）命令

_（git 命令窗口可以使用 linux 命令）_
`cd <文件路径>`: 跳转目录到
`ls`: 列出当前目录下的文件
`cat <文件名>`: 查看文件
`mkdir <文件名>`: 创建一个名为 fileName 的文件夹
`pwd`: 查看当前目录的绝对路径
`vim <文件名>`: 编辑文件。按"i"键，开启 insert，即进入编辑模式。编辑完成后，按"Esc"键退出编辑，输入":wq",回车，保存修改。
`wq`: 编辑完成后，保存文件
`clear`: 清屏

`lsof -i:端口号`: (list open file)查看某端口号占用情况

#### 基础操作命令

`git init`: 在当前目录下，新建一个 git 仓库，自定义管理当前目录下的所有文件。在执行命令的目录下，初始化 Git 管理，创建`.git`文件夹。**但是还未开始跟踪管理项目中的文件**。

`git status`: 当前仓库状态，文件是否修改、暂存。红色：已修改，未暂存；绿色：已暂存。

##### add

`git add <file-name>`: 跟踪(track)文件或目录，并暂存修改。()
`git reset HEAD <file-name>`: 将暂存的该文件取消暂存。
`git rm <file name>`: 删除文件并取消跟踪。（`--cache`选项: 取消跟踪，但不删除文件）

##### commit

`git commit`: 将所有暂存(使用了 add 命令)的文件提交仓库，生成一个新版本。(commit 后，会要求编辑提交信息。在命令后加: `-m '提交相关的信息'` ，可以更简洁)。
`git reset head~ --soft`: 撤销最新一次的提交。(无法撤销第一提交)

##### log

`git diff`: 查看文件被修改的校系信息。
`git log`: 查看提交历史。(`--prety=参数` 美化输出，参数:"oneline")
`git log --graph`: 图形化查看历史信息。

##### remote

`git remote add <自定义仓库名> <远程仓库的链接>`: 为本地仓库添加一个远程仓库。
`git remote`: 查看远程仓库，显示的是自定义的仓库名。
`git remote rename <旧仓库名> <新仓库名>`: 修改仓库名。

##### push

`git push <已添加的远程仓库名> <要推送的分支名>`: 推送到远程仓库。一般需要输入远程仓库的用户名和密码(github 现在已经禁止这种认证方式)。可选：使用 token 作为密码登陆(需要现在 github 个人页面生成 token);SSH 协议鉴权(本地电脑生成 ssh 密钥（见<a href="#appendix_01">附录一</a>），将公钥添加到 github 上，)

##### clone

##### !Git 分支操作命令!

`git branch`: 显示 git 库的分支

### Git 概念与常用功能

#### Git 底层概念

##### .git 目录

> hooks :包含客户端或服务端的钩子脚本
> info :包含一个全局性排除文件
> logs :保存日志信息
> **objects :存储所有数据内容** >**refs :存储指向数据提交对象的指针** >_Commt_EDITMSG_ >_config_ :当前项目配置
> _description_ :对仓库的描述信息
> **_Head_ :指示目前被检出的分支** >**_index_ :保存暂存区信息**

##### git 对象

#### commit/pull/push 操作

**`commit > pull > push`**
`commit` 将代码提交到本地仓库
`pull` 将远程仓库代码同步到本地仓库。如遇冲突，解决冲突，重复 commit->pull，直到没有冲突
`push` 将本地仓库代码提交到远程仓库。

#### 常见开发分支设计

##### [master]

一般都是在新建分支上开发

##### [develop]

开发版本,用于搜集不同的 feature 分支，并进行测试。

##### [feature]\*n

开发新特性(一般有多个，建立在 develop 分支上)。

##### [release]

发布版本。

##### [hotfix]

热补丁，修 bug 用的。

#### Git .gitignore 文件忽略规则详解

##### 优先级

```java
优先级
1.从命令行中读取可用的忽略规则
2.当前目录定义的规则
3.父级目录定义的规则，依次递推
4.$GIT_DIR/info/exclude 文件中定义的规则
5.core.excludesfile中定义的全局规则
//.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
//解决方法就是先把本地缓存删除（改变成未track状态），然后再提交
```

##### 匹配语法

忽略相关的标识符与文件名或路径相组合，形成一条忽略规则（需单独一行）

- 表示忽略相关的
  `!前缀` 表示不被忽略。如果排除了文件的父目录，则无法重新包含该文件。出于性能原因，Git 没有列出排除的目录，因此所包含文件上的任何模式都不起作用，无论它们定义在哪里。
  `/后缀` 忽略该文件夹以及在该文件夹路径下的所有内容。但是不忽略同名的文件
  `/点缀` 忽略仓库根目录下的文件或文件夹
  `单独的文件名或路径` 忽略所有路径下的文件或文件夹
- 表示文件名相关的(与正则表达式类似)
  `**` 匹配多级目录，可在开始，中间，结束
  `?` 通用匹配单个字符
  `*` 通用匹配零个或多个字符
  `[]` 通用匹配单个字符列表

<h4 id="appendix_01">附录一</h4>

![创建密钥示例](https://i0.hdslb.com/bfs/note/9a77b6d799842a63d6a3de117cd32dfcc382bddb.jpg@!web-comment-note.webp)

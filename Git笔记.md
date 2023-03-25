## Git
### Git命令
#### 配置命令
`git config [options]`：配置或读取环境变量
- *options:*
    - `--system` ： 所有用户配置`/etc/gitconfig`
    - `--global` ：当前用户配置`~/.gitconfig`
    - `无` ：当前项目配置`.git/config`
    - `--list` ：检查已有配置
```java 
//每一个级别的配置都会覆盖上层的相同配置
```

###### !必须的初始配置!
>用户信息
>>\$ git config --global user.name "your_username"
>>\$ git config --global user.email your_email@example.com

删除配置的用户信息：`$ git config --global --unset user.email`

#### 底层（Linux）命令
`cd targetFileURL`: 跳转目录到targetFile
`ls`: 列出当前目录下的文件
`mkdir fileName`: 创建一个名为fileName的文件夹
`pwd`: 查看当前目录的绝对路径
`vim documentName`: 编辑名为documentName的文件（需要开启insert）
`wq`: 编辑完成后，保存文件
`clear`: 清屏

`lsof -i:端口号`: (list open file)查看某端口号占用情况

#### 基础操作命令
`git init`: 在执行命令的目录下，初始化Git管理，创建`.git`文件夹。**但是还未开始跟踪管理项目中的文件**。

#### !Git分支操作命令!

### Git概念与常用功能
#### Git底层概念
##### .git 目录
>hooks :包含客户端或服务端的钩子脚本
>info :包含一个全局性排除文件
>logs :保存日志信息
>**objects :存储所有数据内容**
>**refs :存储指向数据提交对象的指针**
>*Commt_EDITMSG*
>*config* :当前项目配置
>*description* :对仓库的描述信息
>***Head* :指示目前被检出的分支**
>***index* :保存暂存区信息**

##### git 对象

#### commit/pull/push操作
**`commit > pull > push`**
`commit` 将代码提交到本地仓库
`pull` 将远程仓库代码同步到本地仓库。如遇冲突，解决冲突，重复commit->pull，直到没有冲突
`push` 将本地仓库代码提交到远程仓库。

#### Git分支类型
##### [master]
##### [develop]
##### [feature]
##### [release]
##### [hotfix]

#### Git .gitignore文件忽略规则详解
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
    `!前缀` 表示不被忽略。如果排除了文件的父目录，则无法重新包含该文件。出于性能原因，Git没有列出排除的目录，因此所包含文件上的任何模式都不起作用，无论它们定义在哪里。
    `/后缀` 忽略该文件夹以及在该文件夹路径下的所有内容。但是不忽略同名的文件
    `/点缀` 忽略仓库根目录下的文件或文件夹
    `单独的文件名或路径` 忽略所有路径下的文件或文件夹
- 表示文件名相关的(与正则表达式类似)
    `**` 匹配多级目录，可在开始，中间，结束
    `?` 通用匹配单个字符
    `*` 通用匹配零个或多个字符
    `[]` 通用匹配单个字符列表
# Git Notes

## Start-ups

运行`git`

- Linux: 直接在命令行里敲
- Windows: 在`Git Bash`里敲命令, 当然亲测在`Terminal`里面直接敲命令也是可以的

> Windows Terminal 里面也能打开Git Bash

VS Code有Git插件, 可以方便地用下述某些功能. (不过我还没完全摸清)

### Run git in VS Code

under construction



### User config

安装后第一次运行git时“自报家门”

`git config --global user.email "you@example.com"`
`git config --global user.name "Your Name"`

查看

`git config --global --list`



### 在线教程

[廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600)

[阮一峰](https://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)





## 基本操作

### 新建仓库

`init`

在当前目录下创建一个空仓库

- 可以在非空目录下创建, 但是这个目录下的文件不会自动加入git的记录, 需要手动`add`&`commit`
- (可以不在当前目录而是指定路径吗? 应该是可以的吧, 但是没啥必要好像)

​	

#### 新建裸仓库

`# git init --bare <sample>.git`

- 裸仓库没有工作区
- 用作远程仓库



#### 在GitHub中创建repo

Under construction



## 修改

git的工作逻辑

TBC...



### add

- `git add <file>`
- 用来把文件提交到`stage`(暂存区)



### commit

- `git commit -m "<note>"`
- 把`stage`的所有内容提交到当前分支, 并做注释
- 注意`add`和`commit`分两步, 因为你可以`add`多个文件, 再对这些修改做一次`commit`
- 注意, `commit`只提交被`add`到`stage`的内容, 如果工作区有一些给改动没有被`add`, 那么它们不会被`commit`



### 删除文件

- 注意, 删除文件也算改动

- 两种方式: 

	1. 手动删除, 再用`git add` or `git rm`, or
	2. 直接`git rm`

	再`git commit`



### 重命名

直接操作就行, 他会识别出来你在重命名

### 撤销修改

`restore <file>`

- 在工作区做了修改, 还没被`add`, 不想要了
- 这个操作是和`add`相对的. 比如, 你在工作区做了一个改动, 如果想保留, 就`add`; 不想保留, 就`restore`
- 注意, 这个操作只能丢弃尚未被`add`到暂存区的改动. 比如, 先做了改动1, `add`了, 再做了改动2, 没有`add`. 此时用这个命令只能丢弃改动2
- `checkout -- <file>`作用相同(?)



`checkout -- <file>`

- 用版本库里的版本替换工作区的版本(?



`restore --stage <file>`

- 修改已经被`add`到暂存区, 但是还没有被`commit`. 把已经`add`的内容取消`add`
- 这个操作是和`add`相反的. 比如, 一个改动, 用`add`把改动从工作区添加到暂存区, 而`restore --stage`则把位于暂存区的改动回退到工作区
- `reset HEAD <file>`作用相同(?)



### 查看状态

`status`

- 掌握仓库当前的状态: 
	- 会告诉你当前位于哪个分支
	- 如果关联到了远程库, 会提示与上游分支是否一致
	- 如果新建了文件, 尚未`add`, 会提示“未跟踪的文件”
	- 如果对文件进行了改动, 尚未`add`, 会提示“尚未暂存以备提交的变更”(Changes not staged for commit)
	- 如果已经`add`, 尚未`commit`, 会提示“要提交的变更”(Changes to be committed)
	- 如果已经`commit`, 会提示“无文件要提交，干净的工作区”(nothing to commit, working tree clean)



`log`

- 查看提交历史, 由近到远
- 会显示版本号, 作者信息, 时间, 以及`commit`时写的注释
- 可加`--pretty=oneline`参数, 使输出更整洁
- `git log --all --graph --decorate` 



`reflog`

- 查看命令历史
- 注意这个操作只会记录涉及版本号的改动, 即`commit`和`reset`
- （也会记录分支的切换



`diff`

- 查看具体有什么改动
- 显示的格式是Unix通用的`diff`格式



### 版本变更

`HEAD`

- 表示当前版本
- `HEAD^`或`HEAD~1`表示上一个版本, `HEAD^^`或`HEAD~2`表示上上一个版本, 以此类推
- （好像`HEAD@{n}`也可以用来表示历史版本（？n从0开始，0表示当前版本



`commit id`(版本号)

- 是一个哈希值(SHA1)
- 用的时候不需要写全, 写前几位就行. 一般是给出前7位



`reset --hard`

- 版本回退
- eg. `git reset --hard HEAD^`
- eg. `git reset --hard <commit id>`



## 远程库

### ssh

在自己的用户主目录下(`~`)查看有没有`.ssh`目录

> 注意, 以`.`开头的文件夹是默认隐藏的, 用`ls -ah`查看

再看有没有`id_rsa`和`id_rsa.pub`

没有的话, `ssh-keygen -t rsa -C "youremail@example.com"`, 一路回车就行



在GitHub中添加ssh: 用户的setting下

- 复制`id_rsa.pub`的内容到`key`, 标题随便起



### 基本操作

#### 查看远程库信息

`git remote -v`



#### 取消连接远程库

`git remote rm origin(<remote repo name>)`

- 注意, 这个操作不会删除远程库的内容, 只是取消连接



### 把本地仓库推送到远程

1. 本地仓库关联到远程库

`git remote add origin git@<server>:<dir>.git`

- `origin`: 远程库的名字. 这个名字是你自己取的

> 一个本地库可以关联到多个远程库（怎么操作啊？？？

2. 本地内容推送到远程

`git push [-u] origin(<remote repo name>) master(<local branchname>)`

- 第一次推送需要`-u`
- 之后可以直接`git push`

> 如果不清楚远程repo和本地repo的名字, 可以`git status`一下

> 注意关联到远程库和把本地内容推送到远程分两步进行



### 从远程库克隆

`git clone git@<server>:<dir>.git`

- `<server>`: Github的是`github.com`, 树莓派的是`192.168.31.108` (currently)

> 注意, 在GitHub上创建的仓库的默认分支叫`main`不叫`master`

`clone`的repo会放在当前路径下 (执行命令时的路径)



### 自己搭远程库

首先, 搞个树莓派或者其他电脑作服务器, 在这个服务器上创建一个名为`git`的用户

> 注意, 这个用户不需要登陆, 也不需要权限. 它只是在`git@<server>`的时候用到
>
> 禁用`git`用户的shell登录: 编辑`/etc/passwd`, 找到类似`git:x:1001:1001:,,,:/home/git:/bin/bash`的一行, 改为`git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`. 这样它一登陆成功就会自动退出

收集所有需要登录的用户的公钥, 就是他们自己的`id_rsa.pub`文件, 把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里, 一行一个

> 如果这个文件不存在就创建一个

`# git init --bare <sample>.git` 初始化一个裸仓库. 裸仓库没有工作区, 纯粹是为了共享

`# chown -R git:git <sample>.git` 把owner改为`git`



## 分支

### 创建新的分支

`git branch <branchname>`



### 切换到分支

`git switch <branchname>`

或者`git branch checkout <branchname>`(?)



### 创建并切换

`git switch -c <branchname>`

或者`git branch checkout -b <branchname>`



### 删除分支

`git branch -d <branchname>`



### 查看当前分支情况

`git branch` 这个只能查看分支

`git branch -v` 查看分支, 各分支最新的版本号(前7位), 以及`commit`时的注释



### 合并到当前分支

`git merge <branchname>`

> 注意是合并到当前分支. 也就是说, 如果你当前在`beta`分支, 想把`beta`分支合并到`master`, 那你得先`git switch master`切换到`master`, 再`git merge beta`把`beta`合并到`master`, 再`git switch beta`切换回`beta`
>
> (有没有简便操作啊



### 重命名分支

`git branch -m <old> <new>`




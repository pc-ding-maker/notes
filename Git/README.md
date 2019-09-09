# GIT

----------


### 简介
- git是一款**分布式**源代码管理工具
- git是linux之父李纳斯的第二个伟大作品


### 分布式（git）和集中式（SVN）的区别
- 在集中式下, 开发者只能将代码提交到服务器, 在分布式下, 开发者可以本地提交
- 在集中式下, 只有远程服务器上有代码数据库, 在分布式下, 每个开发者机器上都有一个代码数据库
<br>

## GIT工作原理
- 工作区(working Directory)：仓库文件夹下除了**.git隐藏目录**外的所有内容
- 版本库（Repository）：git目录，用于储存记录版本信息
	- 版本库的暂缓去（stage）
	- 版本库的分支（master）：提交时git会自动创建的一个分支
	- 版本库的HEAD指针：用于指向当前分支（指向哪个分支commit时就会提交到哪一个分支）
- 忽略文件（.gitignore）：用于忽略不想被git管理的文件
<br>

## GIT使用环境
- 多人开发时需要一个共享版本库, 单人开发初始化一个本地库即可
- 库形式：
	+ 共享版本库：自己搭建git服务器/托管到第三方平台(github/oschina等)
	+ 本地库：磁盘上的文件夹
<br>

## GIT常用操作
- `git help`：查看git指令帮助
- `git init`：仓库初始化（初始化个人仓库）
	`git init --bare`：（初始化共享仓库）


 	仓库文件目录
	> HEAD: 指向当前分支的一个提交
		description:  项目的描述信息
		config:   项目的配置信息
		info/:    里面有一个exclude文件，指定本项目要忽略的文件
		objects/: Git对象库(commit/tree/blob/tag)
		refs/:    标识每个分支指向哪个提交
		hooks/:   默认的hook脚本

- `git config -l`:查看配置信息
	 `git config user.name "用户名"`：设置名称，便于追溯修改记录
	 `git config user.email "邮箱地址"`：设置邮箱，用于多人开发间的沟通

- `git status`:查看文件状态（红色(没有被管理或者被修改了)、绿色(在暂缓区)）
	 `git status 文件名`：查看相应文件状态

- `git add`：将文件添加到暂缓区
	`git add 文件名`：将相应文件添加到暂缓区
	`git add .`：（最后是一个点号，将所有文件添加到暂缓区）

- `git commit`：将暂缓区文件提交到HEAD指向的分支
	`git commit -m"注释 文件名"`：将对应的文件提交到分支
	`git commit -m"注释"`：将暂缓区的所有内容提交到分支

- `git log`：查看文件的修改日志
	`git log 文件名`：查看相应文件的修改日志
	`git log`:查看当前路径所有文件的修改日志
	`git log ––pretty=oneline`：用一行的方式查看简单的日志信息
	`git log –N`：查看最近的N次修改（N是一个整数）

- `git diff`:查看文件最新改动的地方
	`git diff 文件名`：查看相应文件最新改动的地方
	`git diff`：查看当前路径所有文件的最新改动的地方

- `git reflog`：查看分支引用记录（能够查看所有的版本号）

- `git rm`:删除文件（删完之后要进行commit操作，才能同步到版本库）
	`git rm 文件路径`：删除暂缓区或分支上的文件和工作区上的文件
	`git rm --cached 文件路径`：删除暂缓区或分支上的文件不删除工作区上的文件

- `git reset`：版本回退（建议加上––hard参数，git支持无限次后退）
	`git reset ––hard HEAD^`：回退到上一个版本
	`git reset ––hard HEAD^^`:回退到上上一个版本
	`git reset ––hard HEAD~N（N是一个整数）`：回退到上N个版本
	`git reset ––hard 版本号`：回退到对应版本号的版本

- `git clone`：下载远程仓库到本地
- `git pull`：下载远程仓库的最新信息到本地仓库
- `git push`：将本地的仓库信息推送到远程仓库
	注意点：提交时如果远程仓库有其它人提交的最新代码, 必须先pull, 再提交

- `git branch`:查看分支
	`git branch 分支名称`：创建指定分支
	`git branch -r`:查看远程服务器上的分支
- `git switch 分支名称`：切换到指定分支

- `git branch -d 分支名称`:删除指定分支
	`git push origin --delete 分支名称`:来删除远程服务器的分支
	注意点：不能在当前分支中删除自己
- `git merge 分支名称`：将当前所在分支和指定名称分支进行合并

## GIT忽略文件（.gitignore）
+ 在工作区中创建一个名为.gitignore的文本文档（windows系统不让创建，可以用git终端命令**touch .gitignore**来创建）
+ 企业开发忽略模板[.gitignore](https://github.com/github/gitignore)

### 忽略规则

>               
              
    *         表示此为注释,将被Git忽略
	*.a             表示忽略所有 .a 结尾的文件
	!lib.a          表示但lib.a除外
	/TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
	build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
	doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt
	 
	bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
	/bin:           表示忽略根目录下的bin文件
	/*.c:           表示忽略cat.c，不忽略 build/cat.c
	debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
	**/foo:         表示忽略/foo,a/foo,a/b/foo等
	a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
	!/bin/run.sh    表示不忽略bin目录下的run.sh文件
	*.log:          表示忽略所有 .log 文件
	config.php:     表示忽略当前路径的 config.php 文件
	 
	/mtk/           表示过滤整个文件夹
	*.zip           表示过滤所有.zip文件
	/mtk/do.c       表示过滤某个具体文件
	 
	被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。
	 
	需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
	!*.zip
	!/mtk/one.txt
	 
	唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
	想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
	/mtk/*
	!/mtk/one.txt
	 
	假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
	注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！
	 
	----------------------------------------------------------------------------------
	还有一些规则如下：
	fd1/*
	说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；
	 
	/fd1/*
	说明：忽略根目录下的 /fd1/ 目录的全部内容；
	 
	/*
	!.gitignore
	!/fw/ 
	/fw/*
	!/fw/bin/
	!/fw/sf/
	说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。

## GIT单人开发流程
> 
	一、准备工作(只做一次):
    1.创建一个工作区
	2.在工作区中的打开git终端
	3.通过git init指令, 初始化版本库
	4.通过git config user.name "姓名"
	      git config user.email "邮箱"
	  设置用户名和邮箱(不设置要挨骂)
	5.通过git config -l查看设置情况
	二、开发阶段(反复执行)
	1.编写代码
	2.通过"git add 文件名称"/"git add ." 添加到版本库的暂缓区中
	3.通过git commit -m"说明" 将暂缓区的文件添加到HEAD指针指向的分支中
	(默认只有一个分支, master分支, 也称之为主分支)
	注意点:
	1.不是写一句代码就add commit一次, 应该是完成一个功能后再add commit
	2.commit时-m注释一定要认真编写, 与当前提交内容保持一致, 否则要挨骂

## GIT多人开发流程
> 
	一、在远程服务器上创建一个共享版本库
    1.项目负责人打开远程的服务器, 然后创建一个工作区
    2.在远程的服务器上打开工作区, 在工作区中打开Git终端工具
    3.在Git终端工具中输入 git init --bare
    4.经过以上几步, 就代表远程服务器上的共享版本库已经创建好了
	二、开发人员下载远程版本库
    1.开发人员在自己的电脑上打开Git终端工具
    2.从远程的服务器上下载当前项目的共享版本库  git clone 远程服务器共享版本库地址
      和单人开发使用Git的区别: 单人开发是自己创建版本库, 而多人开发是从远程服务器下载版本库
	三、进入开发阶段
    和单人开发一样
    1.设置用户名和邮箱
    2.编写代码
    3.git add .添加到暂缓区
    4.git commit -m 添加到HEADER指针指向的分支
	5.git push 提交到共享版本库
+ 注意点：commit是将编写好的代码提交到本地的版本库, 所以其它的开发人员是拿不到我们提交的代码的，如果想让其它开发人员也能拿到我们提交的代码, 还必须将编写好的代码提交到远程的服务器


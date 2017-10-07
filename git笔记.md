# 课程介绍

+ 课时：9天
+ 安排：git（2天）+环境搭建（3天）+功能实现（4天）
+ 特点：典型的后台管理系统，里面会涉及到大量的jquery插件的使用。
+ 目的
  + 掌握分布式版本控制工具（git）的使用
  + 掌握模块化的开发方法（require.js）
  + 复习模板引擎的使用
  + 复习ajax的使用
  + 学习jquery插件的使用
  + 学习打包工具的使用（gulp）
  + 学习接口化开发的模式
+ 线上地址：[http://bxg.botue.com/](http://bxg.botue.com/)  【账号：前端学院  密码：123456】
+ 线上文档：[http://doc.botue.com/](http://doc.botue.com/)

# shell+bash

## shell基本概念

> 在计算机科学中，Shell俗称壳，是指“提供使用者使用界面”的软件（命令解析器）、比如window系统的cmd、linux系统中的bash。**它可以接受用户的命令，然后调用响应的应用程序。** 

![](images/shell.jpg)

创建文件、移动文件、调用计算器等计算机功能最终都依赖于计算机的硬件，用户无法直接操作这些硬件。但是我们可以通过shell，输入响应的命令，就可以让计算机调用响应的硬件去执行响应的操作。因此我们把shell叫做壳。

**shell的实质就是一个软件，它可以接收用户的命令，然后调用计算机相应的应用程序**

## shell分类

1. 图形界面shell：通过提供友好的可视化界面，调用相应应用程序，如windows系列操作系统，Linux系统上的图形化应用程序GNOME、KDE等。
2. 命令行shell：通过键盘输入特定命令的方式，调用相应的应用程序，
   - windows系统的cmd.exe、PowerShell
   - Linux系统的Bourne shell ( sh)、Bourne Again shell ( bash)等。

**图形界面shell操作方便，直观，但是更加消耗计算机的性能。命令行shell因为不用渲染界面，效率更高，在linux服务器上使用非常的广泛** 



## 关于bash

> bash是linux操作系统默认的命令行shell。windows系统中并不支持bash。
>
> 想要在windows中使用bash，需要安装软件，这个软件**模拟集成** 了bash的大部分命令。

学习bash的目的：

1. 将来工作过程中，碰到了linux服务器，能够大致知道如何操作。
2. git bash中需要使用到一些简单的bash命令。

## 常见的bash命令

1. 在任意文件夹中，都可以使用右键，通过git bash here打开命令行窗口，此时的路径就是当前目录。
2. 窗口上右键，通过options-->Text-->select可以调整字体的大小。

- 路径操作

```bash
# cd 改变目录  （change directory）
cd images   #进入images文件夹
cd ..      #进入上一层目录
cd ~       #进入用户根目录

# tab  自动补全，当我们输命令或者目录很长时，可以使用tab键进行自动补全。
# 按两次tab，会把所有符合要求的内容都列出来。

# pwd 打印当前目录的路径 （print work directory）
pwd


# ls 展示当前目录列表（list）
ls         # 展示当前目录
ls -a      # 展示全部内容，包括隐藏文件
ls -l      # 以列表的形式展示内容
ls -al     # 以列表的形式展示所有的内容，包括隐藏文件。
ls --help  # 查看ls所有的参数。
ls -l images   # 展示images目录下的文件，如果没有写目录，默认展示当前目录。


# clear reset清屏
clear  # 清除屏幕内容，滚动条，保留了历史
reset  # 重置，历史记录没了。


# mkdir  创建一个文件夹 （make directory）
mkdir css          # 创建一个css的文件夹
mkdir css img js   # 创建了三个文件夹

# rmdir  删除一个空的文件夹（没啥用）
rmdir img   # 删除文件夹

# touch  创建文件
touch index.html   #创建了一个index.html文件
touch css/index.css # 在css目录下创建idnex.css文件

# rm 删除一个文件获取文件夹
rm index.html # 删除index.html文件
rm js         # 删除空的js文件夹
rm -r css     # 递归删除一个文件夹

# mv 移动文件（move）
mv index.html js            # 将html文件移动到js文件夹中
mv index.html index2.html   # 将index.html重命名为index2.html

# cp 复制文件（cp）
cp index.html index2.html   # 复制index.html文件，命名为index2.html
cp -r css css02             # 如果复制的是文件夹，需要使用-r参数。

# cat 查看文件全部内容
cat index.html
# less 查看文件部分内容
less index.html
# q退出查看


#  >  重定向  将输出结果重定向到某个文件中
ls > info.text                   # 将list的结果重定向到info.text文件中
echo "hello world" > info.text   # 将hello world重定向到info.text文件中 会覆盖原来的内容
echo "hello world1" >> info.text # 将hello world1追加到info.text文件中。 
```

[rm -rf/ 又引发了一个血案](http://www.techug.com/post/a-murder.html)



## vi编辑器

**vi编辑器的使用了解即可，除非真正在linux环境，不然是用不到的**  

```bash
vi info.txt  # 使用vi编辑器打开info.txt文件，如果该文件不存在，将会被创建。
```

vi编辑器的三种模式：



![](images/vi3model.png)

**命令模式** 

使用vi命令，默认进入的是命令模式，此时是无法编辑的。使用`a`或者`i`可以进入输入模式

```bash
# 进入编辑模式，
i #光标前面插入（insert）
I #行首插入
a #光标后插入（append）
A #行末插入
o #当前行下面插入新行
O #当前行上面插入新行

# 保存并退出，大写
ZZ

# 辙销操作，可多次使用
u

# 删除当前行
dd

# 复制当前行
yy

# 粘贴内容
p

# 向前翻页
ctrl+b

# 向后翻页
ctrl+f
```

**输入模式** 

在输入模式下，可以任意的输入内容

在输入模式下，按`esc`进入命令行模式。



**末行模式** 

插入模式无法直接进入末行模式。

在命令行模式下按下`shift+:`进入末行模式，在末行模式下，通常用来保存文件或者撤销保存

```bash
# 使用 vi
vi 文件路径

# 保存，
:w 
# :w filenme #另存为

# 退出
:q

# 保存并退出
:wq

# 撤销更改，返回到上一次保存的状态
:e!

# 不保存强制退出
:q!

# 展示行号
:set nu
```

练习：在js的目录下新建一个文件，静夜思.txt，输入这首诗的内容，并且保存。

[Emacs和Vim：神的编辑器和编辑器之神](http://www.linuxidc.com/Linux/2015-08/121506.htm)

# 版本控制工具

## 什么是版本控制系统？ 

版本控制系统（Version Control System）:是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。版本控制系统不仅可以应用于软件源代码的文本文件，而且可以对任何类型的文件进行版本控制。

常见的版本控制系统有：svn、cvs、git



## 为什么要有版本控制系统? 

1. 在开发过程中，经常需要对一个文件进行修改甚至删除，但是我们又希望能够保存这个文件的历史记录，如果通过备份，那么管理起来会非常的复杂。
2. 在多人开发时，如果需要多人合作开发一个页面，那么修改以及合并也会非常的棘手。容易出现冲突。


## 版本控制系统分类

**本地版本控制系统**

本地版本控制系统就是在一台机器上，记录版本的不同变化，保证内容不会丢失

缺点：如果多人开发，每个人都在不同的系统和电脑上开发，没办法协同工作。

![](images/vcs.png)

**集中式版本控制系統** 

svn/cvs都是集中式的版本控制系统

1. 需要一个中央服务器来管理代码的的版本和备份
2. 所有的用户电脑都是从中央服务器获取代码或者是将本地的代码提交到中央服务器
3. 依赖与网络环境，如果连不上中央服务器，就无法提交和获取代码。
4. 如果中央服务器宕机，所有人都无法工作。

![](images/jzs.png)


**分布式版本控制系统** 

git是分布式的版本控制系统。

1. 需要一台服务器作为代码仓库
2. 每个用户电脑都是一个服务器，并且和代码仓库是镜像的，用户修改和获取代码都是提交到自己的服务器当中。
3. 不需要网络就可以进行工作。
4. 当连接网络时，用户可以选择将自己的服务器与代码仓库进行同步。


![](images/fbs.png)

# git

## git介绍

> Git是一款免费、开源的**分布式** **版本控制系统** ，用于敏捷高效地处理任何或小或大的项目。

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

## git安装

[下载地址](https://git-scm.com/download/win)

安装很简单，一直下一步即可。在任意的目录下右键，能够出现下图，表示安装成功了。

![](images/gitinstall.png)

git是用法方式主要有两种，

- git gui，即图形化界面的方式
- git bash，命令行的方式

**bash是linux系统的命令，因此学习git前，我们先学习一下bash** 



## 基本方法

1. 初始化git仓库`git init`
2. 查看当前git仓库的状态`git status`
3. 将文件添加到git的暂存区`git add 文件名`
4. 将文件由暂存区提交到仓库区`git commit -m '提交说明'`
5. 查看提交日子`git log`

```bash
# 初始化git仓库，会在当前目录生成一个隐藏文件夹 .git  不要去修改这个文件夹下的任意东西。
git init

# 查看git的状态 ,如果此时新建一个文件，那么这个文件是没有被追踪的，说白了git还没有管理这个新建的文件
git status 

# 让git管理这个新建的文件
git add index.html

# 让文件由暂存区提交到仓库区。此时文件才真正的被git管理了。
# 
# 如果提交日志乱码，右键-->options-->Text-->将编码改成utf-8

git commit -m '第一次提交'

# 查看提交日志
git log
```



## 配置邮箱和用户名

如果第一次使用git，会要求设置用户名和邮箱

```bash
# git config  user.name 你的目标用户名
# git config  user.email 你的目标邮箱名
# 这种配置方式只有在当前仓库生效
git config user.name hcc
git config user.email flycc@163.com

# 可以使用--global参数，配置全局的用户名和邮箱，这样别的git仓库就不需要重新配置了。
# 如果同时配置了局部的和全局的，那么局部的用户名和邮箱将会生效。
git config  --global user.name hucc
git config  --global user.email 111111@qq.com

# 查看配置信息
git config --list
```



## git的工作原理

![](images/gityl.png)



## git命令详解

### git add

- 作用：将文件由 工作区 添加到 暂存区，暂存文件
- 命令：`git add 文件名`
  - 例如： `git add index.html`
- `git add --all` 或者 `git add -A`（简写） 添加所有文件
- `git add a.txt b.txt` 同时添加两个文件
- `git add *.js` 添加当前目录下的所有js文件




### git checkout 文件名

- 作用：暂存区的内容恢复到工作区。
- `git checkout 1.txt` 将暂存区中1.txt文件恢复到工作区


### git commit

- 作用：将文件由 暂存区 添加到  仓库区
- `git commit -m "提交说明"`




### git status

+ 作用：查看文件的状态

- 命令：`git status`
- 命令：`git stauts -s` 简化日志输出格式

### git log

+ 作用：查看提交日志
+ `git log` 只能查看当前head以及以前的日志
+ `git log --oneline` 简洁的日志信息
+ `git reflog` 查看所有的提交变更日志



### git reset

+ 作用：版本回退，将代码恢复到已经提交的某一个版本中。
+ `git reset --hard 版本号` 将代码回退到某个指定的版本(版本号只要有前几位即可)
+ `git reset --hard head~1`将版本回退到上一次提交
  + ~1:上一次提交
  + ~2:上上次提交
  + ~0:当前提交

## git忽视文件

> 在仓库中，有些文件是不想被git管理的，比如数据的配置密码、写代码的一些思路等。git可以通过配置从而达到忽视掉一些文件，这样这些文件就可以不用提交了。

+ 在仓库的根目录创建一个`.gitignore`的文件，文件名是固定的。
+ 将不需要被git管理的文件路径添加到`.gitignore`中

```bash
# 忽视idea.txt文件
idea.txt

# 忽视.gitignore文件
.gitignore

# 忽视css下的index.js文件
css/index.js

# 忽视css下的所有的js文件
css/*.js

# 忽视css下的所有文件
css/*.*
# 忽视css文件夹
css
```



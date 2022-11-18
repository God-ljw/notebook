# Git 分布式版本控制工具

## 课程内容

- Git概述
- Git代码托管服务
- Git常用命令
- 在IDEA中使用Git



## 1. 前言

### 1.1 什么是Git

Git是一个分布式版本控制工具，主要用于管理开发过程中的源代码文件（Java类、xml文件、html页面等），在软件开发过程中被广泛使用。

在IDEA开发工具中可以集成Git（后面会讲解Git安装和集成过程）：

![image-20210924171926037](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171634316.png)



集成后在IDEA中可以看到Git相关图标：![image-20210924172028329](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637767.png)



可以通过启动两个IDEA窗口模拟两个开发人员来展示Git的使用：

![image-20210926080623416](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637768.png)



其他的版本控制工具：

- SVN
- CVS
- VSS

### 1.2 使用Git能做什么

- 代码回溯：Git在管理文件过程中会记录日志，方便回退到历史版本
- 版本切换：Git存在分支的概念，一个项目可以有多个分支（版本），可以任意切换
- 多人协作：Git支持多人协作，即一个团队共同开发一个项目，每个团队成员负责一部分代码，通过Git就可以管理和协调
- 远程备份：Git通过仓库管理文件，在Git中存在远程仓库，如果本地文件丢失还可以从远程仓库获取

## 2. Git概述

### 2.1 Git简介

Git 是一个分布式版本控制工具，通常用来对软件开发过程中的源代码文件进行管理。通过Git 仓库来存储和管理这些文件，Git 仓库分为两种：

- 本地仓库：开发人员自己电脑上的 Git 仓库
- 远程仓库：远程服务器上的 Git 仓库

<img src="https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637769.png" alt="image-20210924173708313" style="zoom: 50%;" />

解释说明：

> commit：提交,将本地文件和版本信息保存到本地仓库
>
> push：推送,将本地仓库文件和版本信息上传到远程仓库
>
> pull：拉取,将远程仓库文件和版本信息下载到本地仓库



### 2.2 Git下载与安装

下载地址： https://git-scm.com/download

![image-20210924174639182](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637770.png)

下载完成后得到安装文件：![image-20210924174717172](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637771.png)

直接双击完成安装即可，安装完成后可以在任意目录下点击鼠标右键，如果能够看到如下菜单则说明安装成功：

![image-20210924174934683](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637772.png)



Git GUI Here：打开Git 图形界面

![image-20210924175209242](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637773.png)



Git Bash Here：打开Git 命令行

![image-20210924175314485](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637774.png)



Git安装目录结构如下：

![image-20210926094227522](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637775.png)



git更新指令

- git clone https://github.com/git/git.git

## 3. Git代码托管服务

### 3.1 常用的Git代码托管服务

Git中存在两种类型的仓库，即**本地仓库**和**远程仓库**。那么我们如何搭建Git**远程仓库**呢？

我们可以借助互联网上提供的一些代码托管服务来实现，其中比较常用的有GitHub、码云、GitLab等。

| 名称      | 网址                               | 说明                                                         |
| --------- | ------------------------- | ------------------------------------------------------------ |
| gitHub    | https://github.com/       | 一个面向开源及私有软件项目的托管平台，因为只支持Git 作为唯一的版本库格式进行托管，故名gitHub |
| 码云      | https://gitee.com/        | 国内的一个代码托管平台，由于服务器在国内，所以相比于GitHub，码云速度会更快 |
| GitLab    | https://about.gitlab.com/ | 一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的web服务 |
| BitBucket | https://bitbucket.org/    | 一家源代码托管网站，采用Mercurial和Git作为分布式版本控制系统，同时提供商业计划和免费账户 |



### 3.2 码云代码托管服务

码云网址：https://gitee.com/

![image-20210926082758518](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637776.png)



使用码云的操作流程如下：

1. 注册码云账号
2. 登录码云
3. 创建远程仓库
4. 邀请其他用户成为仓库成员



#### 3.2.1 注册码云账号

注册网址： https://gitee.com/signup

![image-20210926083229013](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637777.png)

#### 3.2.2 登录码云

注册完成后可以使用刚刚注册的邮箱进行登录（地址： https://gitee.com/login ）

![image-20210926083328306](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637778.png)

#### 3.2.3 创建远程仓库

登录成功后可以创建远程仓库，操作方式如下：

![image-20210926083510298](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637779.png)

页面跳转到新建仓库页面：

![image-20210926083629924](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637781.png)



解释说明：

> 仓库名称：必填，每个仓库都需要有一个名称，同一个码云账号下的仓库名称不能重复
>
> 路径：访问远程仓库时会使用到，一般无需手动指定，和仓库名称自动保持一致
>
> 开源：所有人都可以查看此仓库
>
> 私有：只有此仓库的成员可见，其他人不可见



创建完成后可以查看仓库信息：

![image-20210926090131032](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637782.png)

**注意**：每个Git远程仓库都会对应一个网络地址，点击【克隆/下载】按钮，在弹出窗口点击【复制】按钮即可复制网络地址，地址如下：

https://gitee.com/ChuanZhiBoKe/myGitRepo.git



#### 3.2.4 邀请其他用户成为仓库成员

前面已经在码云上创建了自己的远程仓库，目前仓库成员只有自己一个人（身份为管理员）。在企业实际开发中，一个项目往往是由多个人共同开发完成的，为了使多个参与者都有权限操作远程仓库，就需要邀请其他项目参与者成为当前仓库的成员。

点击管理按钮进入仓库管理页面，左侧菜单中可以看到【仓库成员管理】：

![image-20210926090608272](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637783.png)



点击【开发者】菜单，跳转到如下页面：

![image-20210926091027151](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637784.png)



点击【添加仓库成员】菜单下的【邀请用户】菜单，跳转到如下页面：

![image-20210926091204422](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637785.png)

可以看到邀请用户有多种方式：链接邀请、直接添加、通过仓库邀请成员

**注意**：被邀请用户必须为码云的注册用户，否则无法成为仓库成员



#### 3.2.5配置SSH公钥

- 生成SSH公钥
  - ssh-keygen -t rsa
  - 不断回车
    - 如果公钥已经存在，则自动覆盖

- Gitee设置账户共公钥
  - 获取公钥
    - cat ~/.ssh/id_rsa.pub



![image-20220805153056055](C:\Users\10850\Desktop\笔记\工具\Git2.assets\202208051530110-166375360173731.png)

- 验证是否配置成功
  - ssh -T [git@gitee.com](mailto:git@gitee.com)

 



## 4. Git常用命令

### 4.1 Git全局设置

当安装Git后首先要做的事情是设置用户名称和email地址。这是非常重要的，因为每次Git提交都会使用该用户信息。在Git 命令行中执行下面命令：

**设置用户信息** 

  git config --global user.name "itcast"

  git config --global user.email "hello@itcast.cn"

**查看配置信息**

  git config --list

git conﬁg --global user.name 

git conﬁg --global user.email

#### 4.1.1、 为常用指令配置别名（可选）

有些常用的指令参数非常多，每次都要输入好多参数，我们可以使用别名。

1.打开用户目录，创建.bashrc 文件

- 部分windows系统不允许用户创建点号开头的文件，可以打开gitBash,执行touch ~/.bashrc

 

![image-20220805152843356](C:\Users\10850\Desktop\笔记\工具\Git2.assets\202208051528401.png)

2. 在.bashrc 文件中输入如下内容：

```
#用于输出git提交日志
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit' #用于输出当前目录所有文件及基本信息
alias ll='ls -al'
```

3. 打开gitBash，执行 source ~/.bashrc

![image-20220805152853202](C:\Users\10850\Desktop\笔记\工具\Git2.assets\202208051528242.png)

#### 4.1.2 解决GitBash乱码问题

1. 打开GitBash执行下面命令

```
git config --global core.quotepath false
```

2. ${git_home}/etc/bash.bashrc 文件最后加入下面两行

```
export LANG="zh_CN.UTF-8" 
export LC_ALL="zh_CN.UTF-8"
```

 

![image-20210926092820321](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637786.png)

注意：上面设置的user.name和user.email并不是我们在注册码云账号时使用的用户名和邮箱，此处可以任意设置。



### 4.2 获取Git仓库

要使用Git对我们的代码进行管理，首先需要获得Git仓库。

获取Git仓库通常有两种方式：

- 在本地初始化Git仓库（不常用）
- 从远程仓库克隆（常用）

#### 4.2.1 在本地初始化Git仓库

**操作步骤如下**：

1. 在任意目录下创建一个空目录（例如repo1）作为我们的本地Git仓库
2. 进入这个目录中，点击右键打开Git bash窗口
3. 执行命令**git** **init**

如果在当前目录中看到.git文件夹（此文件夹为隐藏文件夹）则说明Git仓库创建成功

![image-20210926093721515](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637787.png)



#### 4.2.2 从远程仓库克隆

可以通过Git提供的命令从远程仓库进行克隆，将远程仓库克隆到本地

**命令格式**：git clone 远程仓库地址

![image-20210926094404332](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637788.png)



### 4.3 工作区、暂存区、版本库

为了更好的学习Git，我们需要了解Git相关的一些概念，这些概念在后面的学习中会经常提到。

**版本库**：前面看到的.git隐藏文件夹就是版本库，版本库中存储了很多配置信息、日志信息和文件版本信息等

**工作区**：包含.git文件夹的目录就是工作区，也称为工作目录，主要用于存放开发的代码

**暂存区**：.git文件夹中有很多文件，其中有一个index文件就是暂存区，也可以叫做stage。暂存区是一个临时保存修改文件的地方

![image-20220805152922830](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051529884.png)

![image-20210926094831386](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637789.png)



### 4.4 Git工作区中文件的状态

Git工作区中的文件存在两种状态：

- untracked 未跟踪（未被纳入版本控制）

- tracked 已跟踪（被纳入版本控制）

  ​     1）Unmodified 未修改状态

  ​     2）Modified 已修改状态

  ​     3）Staged 已暂存状态



**注意**：文件的状态会随着我们执行Git的命令发生变化

### 4.4.1 修改文件内容

命令形式: vi 文件名

修改后：按ESC后 输入 :wq

删除文件 ：rm -rf 文件名

修改文件名称 ： mv 原文件名 新文件名

### 4.5 本地仓库操作

本地仓库常用命令如下：

- git status 查看文件状态
- git add 将文件的修改加入暂存区
- git reset 将暂存区的文件取消暂存或者是切换到指定版本
- git commit 将暂存区的文件修改提交到版本库
- git log  查看日志



#### 4.5.1 git status

git status 命令用于查看文件状态

![image-20210926095623297](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637790.png)



注意：由于工作区中文件状态的不同，执行 git status 命令后的输出也会不同

#### 4.5.2 git add

git add 命令的作用是将文件的修改加入暂存区，命令格式：git add fileName

![image-20210926100003056](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637791.png)

加入暂存区后再执行 git status 命令，可以发现文件的状态已经发生变化。

#### 4.5.3 git reset

git reset 命令的作用是将暂存区的文件**取消暂存**或者是**切换到指定版本**

取消暂存命令格式：git reset 文件名

![image-20210926101346514](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637792.png)



切换到指定版本命令格式：git reset --hard 版本号

![image-20210926101401721](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637793.png)

注意：每次Git提交都会产生新的版本号，通过版本号就可以回到历史版本



#### 4.5.4 git commit

git commit 命令的作用是将暂存区的文件修改提交到版本库，命令格式：git commit -m msg 文件名 **(git commit -m '注释内容')**

![image-20210926101859601](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637794.png)

解释说明：

> -m：代表message，每次提交时需要设置，会记录到日志中
>
> 可以使用通配符*一次提交多个文件



#### 4.5.5 git log

git log 命令的作用是查看提交日志

![image-20210926102305539](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637795.png)

通过git log命令查看日志，可以发现每次提交都会产生一个版本号，提交时设置的message、提交人、邮箱、提交时间等信息都会记录到日志中

**在4.1.1中配置的别名 git-log 就包含了这些参数，所以后续可以直接使用指令 git-log**

- 作用:查看提交记录
- 命令形式：git log [option] 
  - options
    - --all 显示所有分支
    - --pretty=oneline 将提交信息显示为一行
    - --abbrev-commit 使得输出的commitId更简短
    - --graph 以图的形式显示



#### 4.5.7 版本回退（git reset）

- 作用：版本切换

- 命令形式：git reset --hard commitID
  - commitID 可以使用**git-log** 或**git log **指令查看



- 如何查看已经删除的记录？
  - git reﬂog
  - 这个指令可以看到已经删除的提交记录

#### 4.5.8 添加忽略文件

一般我们总会有些文件无需纳入Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以在工作目录中创建一个名为 .gitignore 的文件（文件名称固定），列出要忽略的文件模式。下面是一个示例：

 

```
# no .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in the build/ directory build/
# ignore doc/notes.txt, but not doc/server/arch.txt doc/*.txt
# ignore all .pdf files in the doc/ directory doc/**/*.pdf

```



### 4.6 远程仓库操作

前面执行的命令操作都是针对的本地仓库，本节我们会学习关于远程仓库的一些操作，具体包括：

- git remote  查看远程仓库
- git remote add 添加远程仓库
- git clone 从远程仓库克隆
- git pull 从远程仓库拉取
- git push 推送到远程仓库

#### 4.6.1 git remote

如果要**查看已经配置的远程仓库服务器**，可以执行 git remote 命令，它会列出每一个远程服务器的简称。

如果已经克隆了远程仓库，那么至少应该能看到 origin ，这是 Git 克隆的仓库服务器的默认名字。

![image-20210926103746721](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637796.png)

解释说明：

> 可以通过-v参数查看远程仓库更加详细的信息
>
> 本地仓库配置的远程仓库都需要一个简称，后续在和远程仓库交互时会使用到这个简称



#### 4.6.2 git remote add

- **此操作是先初始化本地库，然后与已创建的远程库进行对接**。

- 命令： git remote add <远端名称> <仓库路径>

  - 远端名称，默认是origin，取决于远端服务器设置
  - 仓库路径，从远端服务器获取此URL
  - 例如: git remote add origin git@gitee.com:czbk_zhang_meng/git_test.git
  - ![](C:\Users\10850\Desktop\笔记\工具\Git2.assets\202208051531135-166375386603034.png)

![image-20210926104723901](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637797.png)



注意：一个本地仓库可以关联多个远程仓库

#### 4.6.3 git clone

如果你想获得一份已经存在了的 Git 远程仓库的拷贝，这时就要用到 git clone 命令。 Git 克隆的是该 Git 仓库服务器上的几乎所有数据（包括日志信息、历史记录等）。

- 如果已经有一个远端仓库，我们可以直接clone到本地。
  - 命令: git clone <仓库路径> [本地目录]
    - 本地目录可以省略，会自动生成一个目录

![image-20220805153227135](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051532206.png)

克隆仓库的命令格式： git clone 远程仓库地址

![image-20210926105017148](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637798.png)

#### 4.6.4 git push

将本地仓库内容推送到远程仓库，命令格式：git push 远程仓库简称 分支名称

- 命令：git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名]]
  - 如果远程分支名和本地分支名称相同，则可以只写本地分支
    - git push origin master
  - -f 表示强制覆盖
  - --set-upstream 推送到远端的同时并且建立起和远端分支的关联关系。
  - 如果**当前分支已经和远端分支关联**，则可以省略分支名和远端名。
    - git push 将master分支推送到已关联的远端分支。



![image-20220805153203703](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051532747.png)

![image-20210926105413681](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637799.png)



在使用git push命令将本地文件推送至码云远程仓库时，如果是第一次操作，需要进行身份认证，认证通过才可以推送，如下：

![image-20210926105913504](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637800.png)

注意：上面的用户名和密码对应的就是我们在码云上注册的用户名和密码，认证通过后会将用户名和密码保存到windows系统中（如下图），后续再推送则无需重复输入用户名和密码。

![image-20210926110810630](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637801.png)



推送完成后可以到远程仓库中查看文件的变化。

解释说明：

> 一个仓库可以有多个分支，默认情况下在创建仓库后会自动创建一个master分支
>





#### 4.6.5 git pull and git fetch

**git** **pull** 命令的作用是从远程仓库获取最新版本并合并到本地仓库

命令格式：git pull 远程仓库简称 分支名称

![image-20210926111013002](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637802.png)



远程分支和本地的分支一样，我们可以进行merge操作，只是需要先把远端仓库里的更新都下载到本 地，再进行操作。

- **抓取** 命令：**git fetch [remote name] [branch name]**
  - 抓取指令就是**将仓库里的更新都抓取到本地，不会进行合并**
    - 如果不指定远端名称和分支名，则抓取所有分支。
  - **拉取** 命令：**git pull [remote name] [branch name]**
    - **拉取指令**就是**将远端仓库的修改拉到本地并自动进行合并**，等同于**fetch+merge**
    - 如果不指定远端名称和分支名，则抓取所有并更新当前分支。

1. 在test01这个本地仓库进行一次提交并推送到远程仓库

![image-20220805153243627](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051532683.png)

2. 在另一个仓库将远程提交的代码拉取到本地仓库

![image-20220805153249393](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051532462.png)



**注意**：如果当前本地仓库不是从远程仓库克隆，而是本地创建的仓库，并且仓库中存在文件，此时再从远程仓库拉取文件的时候会报错（fatal: refusing to merge unrelated histories ）

解决此问题可以在git pull命令后加入参数--allow-unrelated-histories

### 4.7 分支操作

分支是Git 使用过程中非常重要的概念。使用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。

本地仓库和远程仓库中都有分支，同一个仓库可以有多个分支，各个分支相互独立，互不干扰。

通过git init 命令创建本地仓库时默认会创建一个master分支。



关于分支的相关命令，具体命令如下：

- git branch                                     查看分支
- git branch [name]                       创建分支
- git checkout [name]                    切换分支
- git push [shortName] [name]   推送至远程仓库分支
- git merge [name]                        合并分支

#### 4.7.1 查看分支

查看分支命令：git branch

git branch 		列出所有本地分支

git branch -r 	列出所有远程分支

git branch -a 	列出所有本地分支和远程分支

git branch -vv    查看本地分支和远程分支的对应关系

![image-20210926124843275](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637803.png)

#### 4.7.2 创建分支

创建分支命令格式：git branch 分支名称

![image-20210926125053711](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637804.png)

#### 4.7.3 切换分支

一个仓库中可以有多个分支，切换分支命令格式：

- **git checkout 分支名称**

我们还可以直接**切换到一个不存在的分支（创建并切换）**

-  命令：**git checkout -b 分支名**

![image-20210926125259155](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637805.png)

注意：在命令行中会显示出当前所在分支，如上图所示。

#### 4.7.4 推送至远程仓库分支（git push）

推送至远程仓库分支命令格式：git push 远程仓库简称 分支命令

![image-20210926125628894](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637806.png)

推送完成后可以查看远程仓库：

![image-20210926125810878](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637807.png)

#### 4.7.5 合并分支

合并分支就是将两个分支的文件进行合并处理，命令格式：git merge 分支命令

![image-20210926130213015](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637808.png)

注意：分支合并时需注意合并的方向，如上图所示，在Master分支执行操作，结果就是将b3分支合并到Master分支。

  #### 4.7.6 、删除分支

**不能删除当前分支，只能删除其他分支**

**git branch -d b1** 删除分支时，**需要*做各种**检查**

**git branch -D b1** 不做任何检查，**强制删除**



#### 4.7.7、解决冲突

当两个分支上对文件的修改可能会存在冲突，例如同时修改了同一个文件的同一行，这时就需要手动解 决冲突，解决冲突步骤如下：

1. 处理文件中冲突的地方

2. 将解决完冲突的文件加入暂存区(add)

3. 提交到仓库(commit)

冲突部分的内容处理如下所示：



![image-20220805152941780](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051529838.png)

 

#### 4.7.8、开发中分支使用原则与流程

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

在开发中，一般有如下分支使用原则与流程：

- master （生产） 分支

线上分支，主分支，中小规模项目作为线上运行的应用对应的分支；

- develop（开发）分支

是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线 要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支,准备上线。



- feature/xxxx分支,

从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完 成后合并到develop分支。



- hotﬁx/xxxx分支，

从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、develop分支。



- 还有一些其他分支，在此不再详述，例如test分支（用于代码测试）、pre分支（预上线分支）等 等。


![image-20220805153022619](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051530684.png)

 

 

### 4.8 标签操作

Git 中的标签，指的是某个分支某个特定时间点的状态。通过标签，可以很方便的切换到标记时的状态。

比较有代表性的是人们会使用这个功能来标记发布结点（v1.0 、v1.2等）。下面是mybatis-plus的标签：

![image-20210926130452557](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637809.png)

在本节中，我们将学习如下和标签相关的命令：

- git tag                                                查看标签
- git tag [name]                                  创建标签
- git push [shortName] [name]       将标签推送至远程仓库
- git checkout -b [branch] [name]   检出标签

#### 4.8.1 查看标签

查看标签命令：git tag

![image-20210926151333473](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637810.png)

#### 4.8.2 创建标签

创建标签命令：git tag 标签名

![image-20210926151452581](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637811.png)

#### 4.8.3 将标签推送至远程仓库

将标签推送至远程仓库命令：git push 远程仓库简称 标签名

![image-20210926151621286](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637812.png)

推送完成后可以在远程仓库中查看标签。

#### 4.8.4 检出标签

检出标签时需要新建一个分支来指向某个标签，检出标签的命令格式：git checkout -b 分支名 标签名

![image-20210926152111514](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637813.png)

## 5. 在IDEA中使用Git

通过Git命令可以完成Git相关操作，为了简化操作过程，我们可以在IEDA中配置Git，配置好后就可以在IDEA中通过图形化的方式来操作Git。

### 5.1 在IDEA中配置Git

在IDEA中使用Git，本质上还是使用的本地安装的Git软件，所以需要提前安装好Git并在IDEA中配置Git。

Git安装目录：

![image-20210926152847948](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637814.png)

解释说明：

> git.exe：Git安装目录下的可执行文件，前面执行的git命令，其实就是执行的这个文件



IDEA中的配置：

![image-20210926152950420](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637815.png)

说明：如果Git安装在默认目录中（C:\Program Files\Git），则IDEA中无需再手动配置，直接就可以使用。

### 5.2 获取Git仓库

在IDEA中获取Git仓库有两种方式：

- 本地初始化仓库，本质就是执行 git init 命令
- 从远程仓库克隆，本质就是执行 git clone 命令

#### 5.2.1 本地初始化仓库

在IDEA中通过如下操作可以在本地初始化一个本地仓库，其实底层就是执行的 git init 命令。操作过程如下：

1）依次选择菜单【VCS】---【Import into Version Control】---【Create Git Repository】

![image-20210926153806414](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637816.png)



2）在弹出的【Create Git Repository】对话框中选择当前项目根目录，点击【OK】按钮：

![image-20210926154201744](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637817.png)



操作完成后可以看到当前项目根目录下出现了.git隐藏目录：

![image-20210926154757082](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637818.png)

操作完成后可以在IDEA的工具栏中看到Git的相关操作图标：![image-20210926154933876](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637819.png)

#### 5.2.2 从远程仓库克隆

在IDEA中从远程仓库克隆本质就是执行的 git clone 命令，具体操作过程如下：

1）在IDEA开始窗口中点击【Get from Version Control】

![image-20210926155434202](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637820.png)



2）在弹出的【Get from Version Control】窗口中输入远程仓库的URL地址和对应的本地仓库存放目录，点击【Clone】按钮进行仓库克隆操作

![image-20210926155750107](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637821.png)



### 5.3 Git忽略文件

在Git工作区中有一个特殊的文件 .gitignore，通过此文件可以指定工作区中的哪些文件不需要Git管理。我们在码云上创建Git远程仓库时可以指定生成此文件，如下：

![image-20210926161050169](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637822.png)

创建完成后效果如下：

![image-20210926161233052](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637823.png)



解释说明：

> 1）我们在使用Git管理项目代码时，并不是所有文件都需要Git管理，例如Java项目中编译的.class文件、开发工具自带的配置文件等，这些文件没有必要交给Git管理，所以也就不需要提交到Git版本库中
>
> 2）注意忽略文件的名称是固定的，不能修改
>
> 3）添加到忽略列表中的文件后续Git工具就会忽略它



一个参考的.gitignore文件内容如下：

~~~file
.git
logs
rebel.xml
target/
!.mvn/wrapper/maven-wrapper.jar
log.path_IS_UNDEFINED
.DS_Store
offline_user.md
*.class

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
~~~



### 5.4 本地仓库操作

本地仓库操作：

- 将文件加入暂存区，本质就是执行 git add 命令
- 将暂存区的文件提交到版本库，本质就是执行 git commit 命令
- 查看日志，本质就是执行 git log 命令

#### 5.4.1 将文件加入暂存区

当在Git工作区新增文件或者对已有文件修改后，就需要将文件的修改加入暂存区，具体操作如下：

![image-20210926162515597](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637824.png)

#### 5.4.2 将暂存区文件提交到版本库

将暂存区文件提交到版本库，可以选择一个文件进行提交，也可以选择整个项目提交多个文件。在IEDA中对文件的提交进行了简化操作，也就是如果文件修改后，无需再加入暂存区，可以直接提交。

1）提交一个文件：

![image-20210926162809740](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637825.png)

可以看到，如果选中一个文件提交，则菜单名称为【Commit File...】



2）提交多个文件：

![image-20210926162843891](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637826.png)

可以看到，如果提交多个文件，则菜单名称为【Commit Directory...】



由于提交操作属于高频操作，所以为了进一步方便操作，在IDEA的工具栏中提供了提交操作的快捷按钮：![image-20210926163535277](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637827.png)



#### 5.4.3 查看日志

查看日志，既可以查看整个仓库的提交日志，也可以查看某个文件的提交日志。

1）查看整个项目的提交日志：

![image-20210926163902184](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637828.png)

![image-20210926164138430](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637829.png)



2）查看某个文件的提交日志

![image-20210926164210056](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637830.png)

![image-20210926164233935](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637831.png)



### 5.5 远程仓库操作

远程仓库操作：

- 查看远程仓库，本质就是执行 git remote 命令
- 添加远程仓库，本质就是执行 git remote add 命令
- 推送至远程仓库，本质就是执行 git push 命令
- 从远程仓库拉取，本质就是执行 git pull 命令

#### 5.5.1 查看远程仓库

操作过程如下：

![image-20210926165935756](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637832.png)



在弹出的【Git Remotes】窗口中可以看到配置的远程仓库：

![image-20210926170143160](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637833.png)



#### 5.5.2 添加远程仓库

一个本地仓库可以配置多个远程仓库，在【Git Remotes】窗口中点击【+】来添加一个新的远程仓库：

![image-20210926170653126](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637834.png)

#### 5.5.3 推送至远程仓库

可以通过如下操作将本地仓库文件推送至远程仓库：

![image-20210926170908769](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637835.png)



在弹出的【Push Commits】窗口中可以看到本次推送的文件，点击【Push】按钮即可推送至远程仓库：

![image-20210926171058705](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637836.png)



由于推送至远程仓库操作属于高频操作，所以可以通过IDEA工具栏中的提交快捷按钮同时完成提交和推送：

![image-20210926171408649](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637837.png)

点击【Commit and Push...】按钮同时完成提交和推送操作

#### 5.5.4 从远程仓库拉取

可以通过如下操作从远程仓库拉取：

![image-20210926171646041](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637838.png)



由于从远程仓库拉取文件属于高频操作，所以在IDEA的工具栏中提供了对应的快捷按钮：![image-20210926171919288](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637839.png)



在弹出的【Update Project】窗口中点击【OK】：

![image-20210926171950911](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637840.png)



### 5.6 分支操作

分支操作：

- 查看分支，本质就是执行 git branch 命令
- 创建分支，本质就是执行 git branch 分支名 命令
- 切换分支，本质就是执行 git checkout 命令
- 将分支推送到远程仓库，本质就是执行 git push 命令
- 合并分支，本质就是执行 git merge 命令

#### 5.6.1 查看分支

可以通过如下操作查看分支：

![image-20210926172752562](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637841.png)



在弹出的窗口中可以看到本地分支和远程分支：

![image-20210926172903493](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637842.png)



由于分支操作属于高频操作，所以在IDEA的状态栏中提供了分支操作的快捷按钮：

![image-20210926173622605](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637843.png)



点击【master】快捷按钮即可弹出【Git Branches】分支窗口：

![image-20210926173744979](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637844.png)



#### 5.6.2 创建分支

在【Git Branches】分支窗口中点击【New Branch】，弹出如下窗口：

![image-20210926173903894](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637845.png)



在弹出的【Create New Branch】窗口中输入新分支的名称，点击【Create】按钮完成分支创建

#### 5.6.3 切换分支

通过如下操作可以切换分支：

![image-20210926174358500](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637846.png)



#### 5.6.4 将分支推送到远程仓库

通过如下操作可以将分支推送到远程仓库：

![image-20210926175004502](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637847.png)

#### 5.6.5 合并分支

通过下面操作可以进行分支的合并：

![image-20210926175216197](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img2/202209171637848.png)

# 5.1、在Idea中使用Git

## 5.1 、在Idea中配置Git

安装好IntelliJ IDEA后，如果Git安装在默认路径下，那么idea会自动找到git的位置，如果更改了Git的安装位置则需要手动配置下Git的路径。选择File→Settings打开设置窗口，找到Version  Control下的git选项：



![image-20220805153309101](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051533162.png)

点击Test按钮,现在执行成功，配置完成

![image-20220805153315417](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051533458.png)

## 5.2 、在Idea中操作Git

场景：本地已经有一个项目，但是并不是git项目，我们需要将这个放到码云的仓库里，和其他开发人员 继续一起协作开发。

### 5.2.1 、创建项目远程仓库（参照4.3）



![image-20220805153329197](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051533799.png)

 

 

**5.2.2** **、初始化本地仓库**



![image-20220805153340608](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051533694.png)

 

 

### 5.2.3 、设置远程仓库

 

![image-20220805153352201](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051533280.png)



 

**5.2.4** **、提交到本地仓库**



![image-20220805153358743](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051533828.png)

 

### 5.2.6 、推送到远程仓库

 

![image-20220805153407995](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051534066.png)



**5.2.7** **、克隆远程仓库到本地**



![image-20220805153414880](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051534942.png)

### 5.2.8 、创建分支

- 最常规的方式

 

![image-20220805153423298](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051534360.png)



- 最强大的的方式

 

![image-20220805153428887](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051534967.png)



### 5.2.9 、切换分支及其他分支相关操作



![image-20220805153435237](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051534303.png)

 

## 5.2.11、解决冲突

1. 执行merge或pull操作时，可能发生冲突

![image-20220805153441134](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051534211.png)





2. 冲突解决后加入暂存区：略

3. 提交到本地仓库：略

4. 推送到远程仓库：略

## 5.3 、IDEA常用GIT操作入口

1. 第一张图上的快捷入口可以基本满足开发的需求。



![image-20220805153512340](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535430.png)

2. 第二张图是更多在IDEA操作git的入口。

![image-20220805153517568](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535647.png)




## 5.4 、场景分析

基于我们后面的实战模式，我们做一个综合练习

当前的开发环境如下，我们每个人都对这个项目已经开发一段时间，接下来我们要切换成团队开发模 式。

也就是我们由一个团队来完成这个项目实战的内容。团队有组长和若干组员组成（组长就是开发中的项 目经理）。

所有操作都在idea中完成。练习场景如下：

1、由组长，基于本项目创建本地仓库；创建远程仓库，推送项目到远程仓库。



![image-20220805153525683](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535728.png)

 

2、每一位组员从远程仓库克隆项目到idea中,这样每位同学在自己电脑上就有了一个工作副本，可以正 式的开始开发了。我们模拟两个组员(组员A、组员B)，克隆两个工作区。

 

![image-20220805153532392](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535442.png)



3、组员A修改工作区,提交到本地仓库，再推送到远程仓库。组员B可以直接从远程仓库获取最新的代 码。

![image-20220805153540421](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535474.png)

4、组员A和组员B修改了同一个文件的同一行，提交到本地没有问题，但是推送到远程仓库时，后一个 推送操作就会失败。

解决方法：需要先获取远程仓库的代码到本地仓库，编辑冲突，提交并推送代码。



![image-20220805153546632](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535700.png)

# 附:几条铁令

1. **切换分支前先提交本地的修改**

2. 代码及时提交，提交过了就不会丢

3. **遇到任何问题都不要删除文件目录**，第1时间找老师

# 附:疑难问题解决

**1.**   windows下看不到隐藏的文件（.bashrc、.gitignore）

![image-20220805153556967](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051535023.png)



**2.**  **windows下无法创建.ignore|.bashrc文件**

这里以创建.ignore 文件为例：

- 在git目录下打开gitbash

- 执行指令touch .gitignore

 

![image-20220805153607561](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051536617.png)





# 附：IDEA集成GitBash作为Terminal

![image-20220805153614307](https://cdn.jsdelivr.net/gh/God-ljw/picbed/img/202208051536376.png)

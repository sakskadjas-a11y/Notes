# Git

**git完整命令手册**： http://git-scm.com/docs

## git 简介

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件，与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

## 安装

这里提供windows mac linux 安装方法

#### Windows

有个叫做 msysGit 的项目提供了安装包，可以到 GitHub 的页面上下载 exe 安装文件并运行：

安装包下载地址：https://gitforwindows.org/

直接官网下载也可以：https://git-scm.com/download/win。

完成安装之后，就可以使用命令行的 git 工具（已经自带了 ssh 客户端）了，另外还有一个图形界面的 Git 项目管理工具。在开始菜单里找到 **"Git"->"Git Bash"**，会弹出 Git 命令窗口，你可以在该窗口进行 Git 操作。

####  Linux

首先验证自己是否安装过git。那么输入git即可：

```shell
git
```

若输出以下内容则说明没有安装.

```shell
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
```

Ubuntu Git 安装最新稳定版本命令为：

```shell
sudo apt update
sudo apt install git
```

补充;若已经安装，只升级git,可执行下面命令

```shell
sudo apt install git --only-upgrade
```

接着我们进行验证是否安装成功，若成功输出对应版本，则说明安装成功！

```shell
git --version
git version 2.34.1
```

#### Mac

通过 Homebrew 安装：

```
brew install git
```

如果您想要安装 git-gui 和 gitk（git 的提交 GUI 和交互式历史记录浏览器），您可以使用 homebrew 进行安装：

```
brew install git-gui
```

也可以使用图形化的 Git 安装工具，下载地址为：

https://sourceforge.net/projects/git-osx-installer/

## 工作区 暂存区 版本库

### 1.工作区

电脑里看到的目录，实际修改文件的地区，包含了==当前项目==的所有文件和子目录；不进入版本历史；

**特点：**

- 显示项目的当前状态。
- 文件的修改在工作区中进行，但这些修改还没有被记录到版本控制中。

### 2.暂存区

英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，是一个临时存储区域，它包含了即将被提交到版本库中的文件快照，在提交之前，你可以选择性地将工作区中的修改添加到暂存区；不进入版本历史；

**特点：**

- 暂存区保存了将被包括在下一个提交中的更改。

- 你可以多次使用 `git add` 命令来将文件添加到暂存区，直到你准备好提交所有更改。

	**常用命令：**

	```shell
	git add filename       # 将单个文件添加到暂存区
	git add .              # 将工作区中的所有修改添加到暂存区
	git status             # 查看哪些文件在暂存区中
	```

### 3.版本库

版本库包含项目的所有版本历史记录，每次提交都会在版本库中创建一个新的快照，这些快照是不可变的，确保了项目的完整历史记录。**特点：**

- 版本库分为本地版本库和远程版本库。这里主要指本地版本库。
- 本地版本库存储在 `.git` 目录中，它包含了所有提交的对象和引用。

**常用命令：**

```shell
git commit -m "Commit message"   # 将暂存区的更改提交到本地版本库
git log                          # 查看提交历史
git diff                         # 查看工作区和暂存区之间的差异
git diff --cached                # 查看暂存区和最后一次提交之间的差异
```

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![](https://cdn.jsdelivr.net/gh/sakskadjas-a11y/note@master/image/image.Git.2.png)

-  图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
-  图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
-  图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
-  当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
-  当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
-  当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
-  当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
-  当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
-  当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

### 基于工作区、暂存区和版本库之间关系的命令

#### **1、工作区 -> 暂存区**

使用 git add 命令将工作区中的修改添加到暂存区。

```shell
git add filename
```

#### **2、暂存区 -> 版本库**

使用 git commit 命令将暂存区中的修改提交到版本库。

```
git commit -m "Commit message"
```

#### **3、版本库 -> 远程仓库**

使用 git push 命令将本地版本库的提交推送到远程仓库。

```shell
git push origin branch-name #branch——name为本地仓库的分支名
```

#### **4、远程仓库 -> 本地版本库**

使用 git pull 或 git fetch 命令从远程仓库获取更新。

```shell
git pull origin branch-name
# 或者
git fetch origin branch-name
git merge origin/branch-name
```

## 创建仓库

接下来学习如何创建Git仓库，有`git init` 和 `git config`   `git clone` 三个命令

### git init

Git 使用 **git init** 命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以 **git init** 是使用 Git 的第一个命令。

在执行完成 **git init** 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变。

 **使用方法**

第一种是进入你想要创建仓库的目录，使用当前目录作为 Git 仓库，我们只需使它初始化。

```shell
git init
```

第二种也可以先创建一个新的目录：

```
mkdir my-project
cd my-project
```

使用当前目录作为 Git 仓库，我们只需使它初始化。

```
git init
```

也可以使用我们指定目录作为Git仓库。

```
git init newrepo
```

初始化后，会在 nwerepo目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。

```shell
ls -a 
```

> 通过该命令即可看到.git 文件，可判断当前目录是否为git仓库，使用ls命令.git不可见

```
$ git init
Initialized empty Git repository in C:/Users/aksura/sak-git-test/git/Typora/.git/
$ ls -a
./  ../  .git/  Git.md
```

### git config

git 中进行配置的命令为git config

#### 全局配置

全局配置关键词为**global**，指当前电脑上用户的所有git 仓库提交人默认为全局配置的用户

接下来我们进行全局配置，在命令行输入：

```shell
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

配置完成后，输入以下命令检查全局配置信息

```shell
git config --global user.name
git config --global user.email
```

或者查看所有的全局配置

```shell
git config --global --list
```

####  局部配置

如果你的需求是对某个仓库指定特定的用户名和Email地址，你可以采取局部配置，只需命令去掉 `global`即可。

假如你现在有了一个仓库`A`，进入仓库A，然后执行下列命令

```shell
git config user.name "你的名字"
git config user.email "你的邮箱"
```

并且同样，配置完进行查看配置信息

```shell
git config user.name
git config user.email
```

或者查看所有的局部配置信息

```shell
git config --list
```

### git clone

我们使用 **git clone** 从现有 Git 仓库中拷贝项目（类似 **svn checkout**），使用后会自动将远程仓库的所有分支和历史记录复制到本地。

克隆仓库的命令格式为：

```
git clone <repo>
```

如果我们需要克隆到指定的目录，可以使用以下命令格式：

```
git clone <repo> <directory>
```

**参数说明：**

- **repo:**Git 仓库名。
- **directory:**本地目录。

比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：

```
$ git clone git://github.com/schacon/grit.git
```

执行该命令后，会在当前目录下创建一个名为grit的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录。

如果要**自己定义要新建的项目目录名称**，可以在上面的命令末尾指定新的名字

```
$ git clone git://github.com/schacon/grit.git mygrit
```

**只克隆某一分支**：如果你只想拉取某一个分支，而不是所有分支，可以加上 `--single-branch`：

```
git clone -b dev --single-branch https://github.com/user/demo.git
```

此时只克隆了dev分支

## 基本操作

### 提交与修改

#### git add

```shell
git add [file1] [file2] ...#（将一个或多个文件添加到暂存区）
git add . #（将当前目录下的所有文件添加到暂存区）
git add -all #（暂存整个仓库所有已跟踪和未跟踪文件的变更，不包含上一级仓库）
```

#### 查看状态  git status

查看当前git仓库当前状态，可以==查看上次提交之后==是否有对文件进行更改

`git status` 命令会显示以下信息：

- 当前分支的名称。
- 当前分支与远程分支的关系（例如，是否是最新的）。
- 未暂存的修改：显示已修改但尚未使用 `git add` 添加到暂存区的文件列表。
- 未跟踪的文件：显示尚未纳入版本控制的新文件列表。

1. 使用 **-s** 参数来获得简短的输出结果：

```
$ git status -s
AM README
A  hello.php
```

2.使用 -b 显示当前分支信息

3.--ignored ： 显示被忽略的文件

4.--untracked-files=<mode> ： 控制未跟踪文件的显示方式

```
git status -s -- git/typora笔记（指定路径）
```

**第一列**字符表示**版本库**与**暂存区**之间的比较状态。
**第二列**字符表示**暂存区**与**工作区**之间的比较状态。

“    ” 表示文件未发生更改

`M` 表示文件发生改动。
`A` 表示新增文件。
`D` 表示删除文件。
`R` 表示重命名。
`C` 表示复制。
`U` 表示更新但未合并。
`?` 表示未跟踪文件。
`!` 表示忽略文件。

#### 查看修改内容 git diff

git diff 命令显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别，即比较文件在暂存区与工作区的差异

- 尚未缓存的改动：**git diff**
- 查看已缓存的改动： **git diff --cached****（或git diff – staged)（最后一次提交到版本库中的文件和暂存区中文件的修改对比）
- 查看已缓存的与未缓存的所有改动：**git diff HEAD**（已经提交到版本库中的文件和未提交到版本库中的文件的所有修改对比）
- 显示摘要而非整个 diff：**git diff --stat**


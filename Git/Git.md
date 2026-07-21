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

执行该命令后，会在当前目录下创建一个名为`grit`的目录，其中包含一个 .git 的目录，用于保存下载下来的所有版本记录。

如果要**自己定义要新建的项目目录名称**，可以在上面的命令末尾指定新的名字

```
$ git clone git://github.com/schacon/grit.git mygrit
```

**只克隆某一分支**：如果你只想拉取某一个分支，而不是所有分支，可以加上 `--single-branch`：

```
git clone -b dev --single-branch https://github.com/user/demo.git
```

此时只克隆了dev分支

**只克隆目标文件**

```
git clone <URL> --depth 1
```

`depth`可以控制克隆深度，`depth`为1即只克隆远程仓库文件及最近一次的提交记录，但是此时只会克隆远程仓库的main分支，

如果想看历史提交记录可以使用以下命令

```
git pull --unshallow
```

此时就可以pull 历史的commit

## 关联githugb仓库

首先要创建`SSH`密钥，使用以下命令生成`SSH key` ，在终端中输入以下命令

```
ssh-keygen -t rsa -C "youremail@example.com"
```

> 后面的 **your_email@youremail.com** 改为你在 Github 上注册的邮箱

之后终端会问你几个问题，全部按`Enter`接受默认值即可

- `Enter file in which to save the key...`：直接按 `Enter`。
- `Enter passphrase...`：直接按 `Enter` (不需要密码)。
- `Enter same passphrase again...`：再次按 `Enter`。

指令完成后，画面上会告诉你公钥存储的路径，例如 `/home/你的用户名/.ssh/id_rsa.pub` 或 `id_ed25519.pub`

钥匙有分公钥 (可以给别人) 和私钥 (绝对要自己保管好)。我们要将“公钥”复制到 GitHub。

在终端机输入以下指令来显示公钥内容。请注意，文件名可能是 `id_rsa.pub` 或 `id_ed25519.pub`，这取决于系统默认值，两个都可以。

```shell
cat ~/.ssh/id_rsa.pub
```

**完整复制**画面上出现的所有内容，它应该是以 `ssh-rsa` 或 `ssh-ed25519` 开头。

前往 GitHub 网站，登录你的账号。

点击右上角的个人头像，选择 **Settings**。

在左边的菜单中，找到并点击 **SSH and GPG keys**。

点击绿色的 **New SSH key** 按钮。

**Title**：给这个钥匙取一个好记的名字，例如 `My Ubuntu VM`。

**Key**：将你刚刚复制的公钥内容，完整地粘贴到这个栏位。

最后点击 **Add SSH key**。

完成SSH KEY创建后接着创建Github仓库，在右上角找到“Create a new repo”按钮，创建一个新的仓库,在Repository name填入`你的仓库名词`，这里我叫做`notebooktemp`其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

目前，在GitHub上的这个仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

第一次提交到github远程仓库必须使用

```shell
git remote add origin <URL> #origin为远程仓库名，URL为仓库地址
git push -u origin master #origin为你远程仓库名，master为当前本地仓库分支名
```

之后就成功上传了文件到远程仓库

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

1. 使用 **-s** 参数来获得**简短的输出结果**：

```shell
$ git status -s
AM README
A  hello.php
```

2.使用 -b 显示**当前分支信息**

3.`--ignored` ： 显示被忽略的文件

4.`--untracked-files=<mode>` ： 控制未跟踪文件的显示方式

```shell
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

```shell
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   Git.md #此时使用 git status 可以看到Git.md文件修改了
```

<img src="https://cdn.jsdelivr.net/gh/sakskadjas-a11y/note@master/image/image.Git.3.png" style="zoom:150%;" />



在修改后没有放到暂存区时候即没有执行 `git add`，第一列是红色的M，执行 `git add`后变成绿色的M

#### 查看修改内容 git diff

 git diff 命令显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别，即比较文件在暂存区与工作区的差异

- 尚未缓存的改动：**git diff**
- 查看已缓存的改动： **git diff --cached****（或git diff – staged)（最后一次提交到版本库中的文件和暂存区中文件的修改对比）
- 查看已缓存的与未缓存的所有改动：**git diff HEAD**（已经提交到版本库中的文件和未提交到版本库中的文件的所有修改对比）
- 显示摘要而非整个 diff：**git diff --stat**

使用`git diff` 之后可以看到具体修改内容

```shell
+这里新增内容1   #加号后面是较原来add 存储区版本多的内容
-这里新增内容1   #减号后面是较原来add 存储区版本少的内容
```

**git diff内容查阅操作**

| `q`       | 退出               |
| --------- | ------------------ |
| `空格`    | 向下翻一页         |
| `b`       | 向上翻一页         |
| `回车`    | 向下滚动一行       |
| `↑` / `↓` | 上下移动           |
| `/关键词` | 搜索关键词         |
| `n`       | 跳到下一个搜索结果 |
| `N`       | 跳到上一个搜索结果 |

#### 使用外部工具查看修改内容 git difftool

`git difftool` 命令是 git diff 命令的一个扩展，提供了更直观的可视化工具来解决文件之间的差异。适用于那些更喜欢使用图形化工具而不是命令行工具来处理文件差异的用户。

##### 基本语法

```shell
git difftool [<options>] [<commit> [<path>...]]
```

- **`<commit>`**：指定要比较的提交，默认为当前工作目录的状态。
- **`<path>`**：指定要比较的文件路径。

**常用选项和用法:**

| **选项**               | **说明**                                                     | **用法示例**                        |
| :--------------------- | :----------------------------------------------------------- | :---------------------------------- |
| `--tool=<tool>`        | 指定使用的外部差异工具。默认情况下，Git 使用配置的默认差异工具。 | `git difftool --tool=meld`          |
| `--tool-help`          | 显示可用的差异工具列表。                                     | `git difftool --tool-help`          |
| `--dir-diff`           | 在目录中进行比较，而不是逐个文件。                           | `git difftool --dir-diff`           |
| `--no-prompt`          | 跳过对每个文件的确认提示，自动使用工具查看差异。             | `git difftool --no-prompt`          |
| `--staged`             | 比较已暂存的更改与上一个提交之间的差异。                     | `git difftool --staged`             |
| `--cached`             | 与 `--staged` 相同，用于比较已缓存的更改与上一个提交的差异。 | `git difftool --cached`             |
| `--merge`              | 用于合并工具，处理冲突时查看差异。                           | `git difftool --merge`              |
| `--find-copies`        | 查找文件复制和移动。                                         | `git difftool --find-copies`        |
| `--find-copies-harder` | 更加严格地查找文件复制和移动。                               | `git difftool --find-copies-harder` |

#### 比较两个提交范围之间的差异 git range - diff

```
git range-diff <old-range> <new-range>
```

- **`<old-range>`**：旧的提交范围或分支。
- **`<new-range>`**：新的提交范围或分支。

#####  常见用法

**1、比较两个提交范围**

比较 branch1 的提交范围与 branch2 的提交范围之间的差异：

```
git range-diff branch1 branch2
```

示例：

```
git range-diff feature/old-branch feature/new-branch
```

**2、比较两个提交系列**

比较两个提交系列的差异，查看在特定时间段内的变更：

```
git range-diff <start-old>..<end-old> <start-new>..<end-new>
```

示例：

```
git range-diff HEAD~10..HEAD~5 HEAD~5..HEAD
```

HEAD 10..HEAD~5 表示旧的提交范围，HEAD~5..HEAD 表示新的提交范围。

####  将暂存区内容添加到本地仓库中 git commit

```
git commit -m [message]
git commit [file1] [file2] ... -m [message]（提交暂存区指定文件到仓库区）
git commit -am #（设置-a参数后不用git add 直接提交）
```

#### 删除文件 git rm

git rm 用于删除文件

1. 将文件从暂存区和工作区中删除：

```
git rm <file>
```

2.强行从暂存区和工作区中删除修改后的文件：

```
git rm -f <file>
```

3.把文件从暂存区移除，但仍在工作目录中（仅在跟踪清单中删除）

```
git rm --cached <file>
```

4.想把远程和本地的文件夹删除

```shell
git rm -r <文件夹路径> #远程和本地的都删除
git rm -r --cached <文件夹路径> #只删除远程，本地保留
```

####  移动或重命名文件 git mv

git mv 命令用于移动或重命名一个文件、目录或软连接。

```
git mv [file] [newfile]
```

如果新文件名已经存在，但还是要重命名它，可以使用 **-f** 参数：

```
git mv -f [file] [newfile]
```

#### 提交时附加注释 git notes

允许用户将附加注释添加到提交中

```
git notes <subcommand> [options] [arguments]
```

- **`<subcommand>`**：具体的操作子命令（如 `add`, `show`, `list`, `remove`, `edit`, `merge` 等）。
- **`[options]`**：命令的选项或参数。
- **`[arguments]`**：命令的附加参数，如提交哈希值等。

#####  1.git notes add

**将新的注释添加到提交中。**

- **`-m`**：指定注释的内容。

- **`<commit-hash>`**：指定要添加注释的提交（可选，默认为当前提交）。

	```shell
	# 添加注释到当前提交
	git notes add -m "This commit fixes the login bug"
	
	# 添加注释到特定提交
	git notes add -m "Added new feature X" <commit-hash>
	```

#####  2.git notes show

**显示最新提交的注释。**

- **`<commit-hash>`**：指定要显示注释的提交（可选，默认为当前提交）。

	```shell
	# 查看当前提交的注释
	git notes show
	
	# 查看特定提交的注释
	git notes show <commit-hash>
	```

##### 3.git notes list

**列出当前分支上所有提交的注释。**

##### 4.git notes remove

**删除提交的注释。**

```
git notes remove <commit-hash>
```

##### **5.git notes edit**

**编辑提交的注释。与 `git notes add` 类似，但在编辑模式下打开默认编辑器进行注释编辑。**

##### **`6.git notes merge`**

**合并不同的注释记录**。

##### **`7.refs/notes/*`**

**用于指定注释的引用路径**。用于推送和拉取注释。

```
# 推送注释到远程仓库
git push origin refs/notes/*
```

### 分支管理

使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作，一条分支代表一条独立的开发线

####  创建新分支并切换到该分支：

```
git checkout -b <branchname>
```

#### 切换分支命令:

##### git check out

用于在不同的分支之间切换、恢复文件、创建新分支等操作。

```shell
git checkout <branch-name> #（从当前分支切换到指定分支）
git checkout -b <new-branch-name> #（创建新分支并切换）
git checkout - #（切换到前一个分支）
git checkout <commit> -- <file> #（将指定文件恢复到某次提交时的状态，丢弃未提交的更改）
git checkout <commit-hash> #（切换到指定提交）
git checkout tags/<tag-name> #（如果你有一个标签 <tag-name>，你可以使用这个命令来切换到该标签所指向的提交状态。）
```

##### git switch

作用与 [git checkout](https://www.runoob.com/git/git-checkout.html) 类似，但提供了更清晰的语义和错误检查。

```shell
git switch <branch-name> #（从当前分支切换到指定的分支 <branch-name>）
git switch -c <new-branch-name> #（创建新分支并切换到该分支）
git switch - #（切换到前一个分支）
git switch <commit_hash> #（恢复工作目录到某个特定的提交状态）
git switch -d #（使切换到某个提交的操作更明确，即使存在同名分支也不会切换到分支上。）
git switch -f #（强制执行切换，即使有更改未提交）
git branch #（列出可用的本地分支和标签，便于选择切换目标）
```

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

#### 查看分支

##### 查看所有分支：

```
git branch
```

##### 查看远程分支：

```
git branch -r
```

##### 查看所有本地和远程分支：

```
git branch -a
```

#### 合并分支

将其他分支合并到当前分支：

```
git merge <branchname>
```

例如，切换到 main 分支并合并 feature-xyz 分支：

```
git checkout main
git merge feature-xyz
```

#### 解决合并冲突

当合并过程中出现冲突时，Git 会标记冲突文件，你需要手动解决冲突。

打开冲突文件，按照标记解决冲突。

标记冲突解决完成：

```
git add <conflict-file>
```

提交合并结果：

```
git commit
```

#### 删除分支

删除本地分支：

```
git branch -d <branchname>
```

强制删除未合并的分支：

```
git branch -D <branchname>
```

删除远程分支：

```
git push origin --delete <branchname>
```

如果我们要手动创建一个分支。执行 **git branch (branchname)** 即可，当你以此方式在上次提交更新之后创建了新分支，如果后来又有更新提交， 然后又切换到了 **testing** 分支，Git 将还原你的工作目录到你创建分支时候的样子。

### 远程操作

#### 管理git中的远程仓库 git remote

- `git remote`：列出当前仓库中已配置的远程仓库。
- `git remote -v`：列出当前仓库中已配置的远程仓库，并显示它们的 URL。
- `git remote add <remote_name> <remote_url>`：添加一个新的远程仓库。指定一个远程仓库的名称和 URL，将其添加到当前仓库中。
- `git remote rename <old_name> <new_name>`：将已配置的远程仓库重命名。
- `git remote remove <remote_name>`：从当前仓库中删除指定的远程仓库。
- `git remote set-url <remote_name> <new_url>`：修改指定远程仓库的 URL。
- `git remote show <remote_name>`：显示指定远程仓库的详细信息，包括 URL 和跟踪分支。

#### 从远程仓库获取代码库 git fetch

用于从远程获取代码库，执行`git fetch` 后 执行`git merge` 合并

```
git fetch origin
git merge origin/master
```

#### 远程获取代码并合并本地的版本 git pull

用于从远程获取代码并**合并本地**的版本,为`git fetch` 和`git merge` 合并

```
git pull [远程仓库名] [分支名]
```

- `[远程仓库名]` 通常是 `origin`，是默认的远程仓库名。
- `[分支名]` 是你要合并的远程分支，比如 `main` 或 `master`。

#### 将本地版本上传到远程仓库并合并 git push

命令格式如下：

```
git push <远程主机名> <本地分支名>:<远程分支名>
```

如果本地分支名与远程分支名相同，则可以省略冒号：

```
git push <远程主机名> <本地分支名>
```

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

```
git push --force origin master
```

删除主机的分支可以使用 --delete 参数，以下命令表示删除 origin 主机的 master 分支：

```
git push origin --delete master
```

### 提交日志

#### 显示从最新提交到最早提交的所有信息 git log

it log** 显示了从最新提交到最早提交的所有提交信息，包括提交的哈希值、作者、提交日期和提交消息等。

```
git log [选项] [分支名/提交哈希]
```

- `-p`：显示提交的补丁（具体更改内容）。
- `--oneline`：以简洁的一行格式显示提交信息。
- `--graph`：以图形化方式显示分支和合并历史。
- `--decorate`：显示分支和标签指向的提交。
- `--author=<作者>`：只显示特定作者的提交。
- `--since=<时间>`：只显示指定时间之后的提交。
- `--until=<时间>`：只显示指定时间之前的提交。
- `--grep=<模式>`：只显示包含指定模式的提交消息。
- `--no-merges`：不显示合并提交。
- `--stat`：显示简略统计信息，包括修改的文件和行数。
- `--abbrev-commit`：使用短提交哈希值。
- `--pretty=<格式>`：使用自定义的提交信息显示格式。

#####  常用选项

限制显示的提交数:

```
git log -n <number>
```

以一行格式显示提交信息

```
git log --oneline
```

显示自指定日期之后的提交：

```
git log --since="2024-01-01"
```

显示指定日期之前的提交：

```
git log --until="2024-07-01"
```

只显示某个作者的提交：

```
git log --author="Author Name"
```

#### 通过作者进行分类的查看历史汇总 git shortlog

总结 Git 仓库的提交历史，提供对每位作者的提交计数以及每个提交类别的概览。

```
git shortlog [<options>] [<revision-range>]
```

- **`<revision-range>`**：指定要生成摘要的提交范围，默认为当前分支的全部提交。

- **`<options>`**：用于定制输出格式或行为的选项。

**常用选项和用法：**

| **选项**                  | **说明**                                                     | **用法示例**                               |
| :------------------------ | :----------------------------------------------------------- | :----------------------------------------- |
| **`-n` 或 `--numbered`**  | **按提交数量对作者进行排序，显示每个作者的提交数量。**       | **`git shortlog -n`**                      |
| **`-s` 或 `--summary`**   | **仅显示作者及其提交数量，不显示提交信息。**                 | **`git shortlog -s`**                      |
| **`-e` 或 `--email`**     | **显示作者的电子邮件地址。**                                 | **`git shortlog -e`**                      |
| **`-c` 或 `--committer`** | **按提交者（committer）统计提交数量，而不是作者（author）。** | **`git shortlog -c**`                      |
| `-a` 或 `--all`           | 包括所有作者（包括那些没有任何提交的作者）。                 | `git shortlog -a`                          |
| **`--no-merges`**         | **排除合并提交，只显示非合并提交的摘要。**                   | **`git shortlog --no-merges`**             |
| **`--pretty`**            | **自定义输出格式。**                                         | **`git shortlog --pretty=format:"%h %s"`** |
| **`--since`**             | **仅显示指定时间范围内的提交。**                             | **`git shortlog --since="2023-01-01"`**    |
| `--until`                 | 仅显示指定时间之前的提交。                                   | `git shortlog --until="2023-01-01"`        |
| `--reverse`               | 逆序显示提交摘要，按作者提交数量的升序排列。                 | `git shortlog --reverse`                   |

#### 追踪每一行的变更历史 git blame

 可以追踪文件中每一行的变更历史，包括作者、提交哈希、提交日期和提交消息等信息。

```
git blame [选项] <文件路径>
```

- `-L <起始行号>,<结束行号>`：只显示指定行号范围内的代码注释。
- `-C`：对于重命名或拷贝的代码行，也进行代码行溯源。
- `-M`：对于移动的代码行，也进行代码行溯源。
- `-C -C` 或 `-M -M`：对于较多改动的代码行，进行更进一步的溯源。
- `--show-stats`：显示包含每个作者的行数统计信息。

##### 确定文件路径 使用`git ls-files` 指令

```
git ls-files
```

例如输出：

```
README.md
css/style.css
js/main.js
src/App.java
```

然后直接把对应路径作为 `git blame` 的参数：

```
git blame src/App.java
```

#### 生成版本号 git describe 

`git describe` 命令命令通常用于生成版本号，帮助识别特定的提交，并能够在构建、发布或追踪特定版本时使用，一般是有标签之后才能使用。

### 基本语法

```
git describe [<options>] [<commit>]
```

- **`<commit>`**：指定要描述的提交。默认为当前提交。
- **`<options>`**：用于定制输出格式或行为的选项。

| **选项**                | **说明**                                                     | **用法示例**                  |
| :---------------------- | :----------------------------------------------------------- | :---------------------------- |
| **`--tags`**            | **使用所有标签（包括轻量标签）来生成描述。默认情况下，`git describe` 只考虑附注标签。** | `git describe --tags`         |
| `--abbrev=<n>`          | 设定要显示的提交哈希的最小长度为 `<n>` 个字符。              | `git describe --abbrev=8`     |
| **`--long`**            | **强制显示长格式的描述，包括提交的哈希和距离标签的提交数。** | `git describe --long`         |
| **`--exact-match`**     | **仅在当前提交与标签完全匹配时返回标签名称。**               | `git describe --exact-match`  |
| **`--dirty`**           | **如果工作目录有更改（未提交的更改），则在描述中添加 `-dirty` 后缀。** | `git describe --dirty`        |
| **`--match=<pattern>`** | **仅使用符合指定模式的标签进行描述。**                       | `git describe --match "v1.*"` |
| `--candidates=<n>`      | 限制描述所考虑的标签数量，默认为 10 个。                     | `git describe --candidates=5` |
| **--always`**           | **即使没有找到标签，也会显示提交的哈希。**                   | `git describe --always`       |

### 回退版本

####  恢复工作区和暂存区文件 ： git restore

> 与 `git checkout` 恢复文件时功能一样

用于恢复或撤销文件的更改，可以恢复工作区和暂存区中的文件，也可以用于丢弃未提交的更改，不能撤销提交。

```
git restore [<options>] [<pathspec>...]
```

- **`<pathspec>`**：要恢复的文件或目录路径。
- **`<options>`**：用于定制恢复行为的选项

**常用选项和用法**

| **选项**             | **说明**                                                     | **用法示例**                            |
| :------------------- | :----------------------------------------------------------- | :-------------------------------------- |
| `--source=<commit>`  | 从指定的提交中恢复文件内容。默认为 HEAD，即当前提交。        | `git restore --source=HEAD~1 file.txt`  |
| `--staged`           | 恢复暂存区中的文件内容到工作区，而不是恢复工作区中的内容。   | `git restore --staged file.txt`         |
| `--worktree`         | 恢复工作区中的文件内容到当前工作区状态。                     | `git restore --worktree file.txt`       |
| `--ours`             | 在合并冲突时，恢复为当前分支的版本（即"我们"的版本）。       | `git restore --ours file.txt`           |
| `--theirs`           | 在合并冲突时，恢复为另一个分支的版本（即"他们"的版本）。     | `git restore --theirs file.txt`         |
| `--conflict=<style>` | 指定合并冲突的样式，例如 `merge` 或 `diff3`。                | `git restore --conflict=diff3 file.txt` |
| `--dry-run`          | 显示将要恢复的文件和路径，而不实际进行恢复。                 | `git restore --dry-run`                 |
| `-s`, `--source`     | 与 `--source` 相同，用于从指定提交中恢复文件。               | `git restore -s HEAD~1 file.txt`        |
| `-W`, `--worktree`   | 恢复工作区中的文件内容到当前工作区状态（与 `--worktree` 相同）。 | `git restore -W file.txt`               |
| `-S`, `--staged`     | 恢复暂存区中的文件内容到工作区，而不是恢复工作区中的内容（与 `--staged` 相同）。 | `git restore -S file.txt`               |

#### 重置当前分支到特定提交 git reset

git reset 命令可以更改当前分支的提交历史，它有三种主要模式：--soft、--mixed 和 --hard。

**--soft**：只撤销某次提交，暂存区和工作区保持不变，文件还是暂存状态。

```
git reset --soft <commit>
```

**使用场景：**

- commit 信息写错了
- 想撤销最近一次提交后重新提交
- 想保留已经 `git add` 的内容

**--mixed（默认）**：撤销某次提交，同时暂存区重置，文件未暂存，但工作区保持不变。

```
git reset --mixed <commit>
```

**使用场景：**

- 想撤销 commit
- 也想撤销 `git add`
- 但不想丢失代码修改

**--hard**：撤销某次提交，暂存区和工作区都重置。

```
git reset --hard <commit>
```

**使用场景：**

- 本地修改和提交都不要了
- 想彻底恢复到某个稳定版本
- 实验代码全部丢弃

例如，将当前分支重置到 abc123 提交：

```
git reset --hard abc123
```

| 命令                | 是否移动 HEAD | 是否改暂存区 | 是否改工作区 | 说明                        |
| ------------------- | ------------- | ------------ | ------------ | --------------------------- |
| `git reset --soft`  | 是            | 否           | 否           | 撤销 commit，保留暂存区内容 |
| `git reset --mixed` | 是            | 是           | 否           | 撤销 commit 和暂存区        |
| `git reset --hard`  | 是            | 是           | 是           | 彻底回退，工作区也恢复      |

###### reset 的使用建议

| 场景                  | 是否推荐 |
| --------------------- | -------- |
| 本地提交，还没 push   | 推荐     |
| 已经 push，只有自己用 | 谨慎使用 |
| 已经 push，多人协作   | 不推荐   |

**原因：**

`git reset` 会改写历史。  
如果已经 push 到远程，尤其是多人协作分支，不建议随意使用后再强推。

#### 撤销某次提交但不改变提交历史 git revert

git revert 命令创建一个新的提交，用来撤销指定的提交，它不会改变提交历史，适用于已经推送到远程仓库的提交。

```
git revert <commit>
```

例如，撤销 abc123 提交：

```
git revert abc123
```

###### revert 的效果

1. 例如历史：


```bash
A --- B --- C
```

执行：

```bash
git revert C
```

会变成：

```bash
A --- B --- C --- D
```

其中：

- `C` 还在
- `D` 是新的反向提交
- 最终代码效果等于撤销了 `C`，文件内容和上一个`A-B`版本一样，但提交历史不同

2.假如历史为

```
A --- B --- C --- D
```

执行

```
git revert C
```

会变成

```
A --- B --- C --- D --- E
```

那么此时文件内容是什么呢

第一种情况是==D提交时没有对C修改==，那么`git` 就可以自动撤销C

例如：

```
B
name=Tom
age=18
```

C 修改：

```
name=Jerry
age=18
```

D 新增一行：

```
name=Jerry
age=18
city=Beijing
```

撤销 C 后：

```
name=Tom
age=18
city=Beijing
```

没有冲突。

第二种情况是D对C修改了，那么此时就不能自动撤销，会产生冲突 需要手动解决

如果 D 也修改了 C 修改过的位置，例如：

```
B
hello
C
hello world
D
hello git
```

现在撤销 C。

Git想把：

```
hello world
```

恢复成

```
hello
```

可是当前已经变成

```
hello git
```

Git不知道到底应该变成什么，于是会提示：

```
CONFLICT (content): Merge conflict
```

需要你手动解决冲突。

#### 查看历史操作记录 git reflog

git reflog 命令记录了所有 HEAD 的移动。即使提交被删除或重置，也可以通过 reflog 找回。

```
git reflog
```

例如：

```
git reset --hard HEAD~2
```

结果发现：

> 完了，删错了！

这时：

```
git reflog
```

可能看到：

```
8d3fae1 HEAD@{0}: reset: moving to HEAD~2
91b2c4d HEAD@{1}: commit: fix bug
3af5e77 HEAD@{2}: commit: add login
```

想恢复到 `fix bug`：

```
git reset --hard 91b2c4d
```

或者：

```
git reset --hard HEAD@{1}
```

`HEAD@{0}`：当前HEAD

`HEAD@{1}`：上一次HEAD所在位置

`HEAD@{2}`：上上次HEAD所在位置

####  reset、revert、reflog  reestore 对比

| 对比项               | git reset | git revert | git reflog     | git restore            |
| -------------------- | --------- | ---------- | -------------- | ---------------------- |
| 是否回退代码         | 是        | 是         | 否             | 是                     |
| 是否改写历史         | 是        | 否         | 否             | 否                     |
| 是否新增提交         | 否        | 是         | 否             | 否                     |
| 是否适合已 push 提交 | 不推荐    | 推荐       | 用于补救       | 否                     |
| 是否适合多人协作     | 谨慎使用  | 适合       | 补救工具       | 可以                   |
| 核心作用             | 移动 HEAD | 反向提交   | 查看 HEAD 历史 | 恢复本地仓库的文件内容 |

#####  常见使用情况总结

######  本地提交信息写错了，还没 push

推荐：

```bash
git reset --soft HEAD~1
git commit -m "新的提交说明"
```

---

###### 本地提交想撤销，但代码保留

推荐：

```bash
git reset HEAD~1
```

或者：

```bash
git reset --mixed HEAD~1
```

---

######  本地提交和代码都不要了

推荐：

```bash
git reset --hard HEAD~1
```

---

######  已经 push 到远程，想安全撤销某次提交

推荐：

```bash
git revert <提交ID>
git push origin <分支名>
```

---

######  reset 错了，想找回来

推荐：

```bash
git reflog
git reset --hard <提交ID>
```

### 标签

如果你达到一个重要的阶段，并希望永远记住提交的快照，你可以使用 **git tag** 给它打上标签。Git 标签（Tag）用于给仓库中的特定提交点加上标记。比如说，我们想为我们的项目发布一个 "1.0" 版本，我们可以用 **git tag -a v1.0** 命令给最新一次提交打上（HEAD） "v1.0" 的标签。-a 选项意为"**创建一个带注解的标签**"，不用 -a选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的，也不会让你添加个标签的注解，我们推荐一直创建带注解的标签。

#### 标签语法格式

```
git tag <tagname>（使用该命令为轻量标签）
```

**-a** 选项可以添加注解：

```
$ git tag -a v1.0 （标签为附注标签）
```

####  推送标签到远程仓库

默认情况下，git push 不会推送标签，你需要显式地推送标签。

```
git push origin <tagname>
```

推送所有标签：

```
git push origin --tags
```

####  删除标签

轻量标签为只有哈希值，无其他信息

本地删除：

```
git tag -d <tagname>
```

远程删除：

```
git push origin --delete <tagname>
```

####  附注标签

附注标签存储了创建者的名字、电子邮件、日期，并且可以包含标签信息。附注标签更为正式，适用于需要额外元数据的场景。

创建附注标签语法：

```
git tag -a <tagname> -m "message"
```

PGP 签名标签命令：

```
git tag -s <tagname> -m "runoob.com标签"
```

查看标签信息：

```
git show <tagname>
```

## git 文件状态

**文件状态分为三种**：工作目录（Working Directory）、暂存区（Staging Area）、本地仓库（Local Repository）。

### 工作目录

为在计算机上可以看到的项目文件，是实际操作文件的地方，包括查看，编辑，删除和创建文件，对文件的更改都在工作目录中

工作目录中的文件两种状态

- **未跟踪（Untracked）**：新创建的文件，未被 Git 记录。
- **已修改（Modified）**：已被 Git 跟踪的文件发生了更改，但这些更改还没有被提交到 Git 记录中。

### 暂存区

保存提交到本地仓库中的更改

```
git add <filename>  # 添加指定文件到暂存区
git add .           # 添加所有更改到暂存区
```

### 本地仓库

本地仓库是一个隐藏在 `.git` 目录中的数据库，用于存储项目的所有提交历史记录。每次你提交更改时，Git 会将暂存区中的内容保存到本地仓库中。

使用 `git commit -m "commit message"` 命令将暂存区中的更改提交到本地仓库。

```
git commit -m "commit message"  # 提交暂存区的更改到本地仓库
```

##  文件状态的转换

### **未跟踪（Untracked）**

 新创建的文件最初是未跟踪的。它们存在于工作目录中，但没有被 Git 跟踪。新建文件后没有任何操作就是未跟踪

### **已跟踪（Tracked）**

通过 `git add` 命令将未跟踪的文件添加到暂存区后，文件变为已跟踪状态。

### **已修改（Modified）**

 对已跟踪的文件进行更改后，这些更改会显示为已修改状态，但这些更改还未添加到暂存区。

```
echo "Hello, World!" > newfile.txt  # 修改文件
git status                          # 查看状态，显示 newfile.txt 已修改
```

### **已暂存（Staged）**

使用 `git add` 命令将修改过的文件添加到暂存区后，文件进入已暂存状态，等待提交。

### **已提交（Committed）**

使用 `git commit` 命令将暂存区的更改提交到本地仓库后，这些更改被记录下来，文件状态返回为已跟踪状态。
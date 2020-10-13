# Git教程

Git是一个分布式版本控制系统

## 安装Git

在linux上安装Git

`sudo apt-get install git`

## 工作区、暂存区和版本库

* 工作区：就是电脑里能看到的目录。

* 暂存区：英文叫stage或index。一般存放在`.git` 目录下的index文件。

* 版本库：工作区有一个隐藏目录`.git` ，这个不算工作区，而是Git的版本库。

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

* 图中的左侧为工作区，右侧为版本库。`index` 的区域是暂存区，`master` 的区域是master分支所代表的目录树。
* 图中的`HEAD` 实际是指向master分支的一个游标，所以命令出现HEAD的地方可以用master来替换。
* 图中的`objects` 的区域为Git的对象库，实际位于".git/objects"目录下，里面包含了各种创建的对象以及内容。
* 当对工作区修改（或新增）的文件执行`git add` 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
* 当执行提交操作`git commit` 时，暂存区的目录树写到版本库中，`master` 指向的目录树就是提交时暂存区的目录树。

## 创建仓库

1. 使用命令`git init`创建一个空的Git仓库，使用`ls -a` 可以看到有个.git文件。

2. 使用命令`vim readme.txt` 创建一个文档进行编辑，内容如下：

   ```
   git is a version control system
   git is free software
   ```

3. 使用命令`git add readme.txt` 告诉git，把文件添加到仓库。

4. 使用命令`git commit` 告诉git，把文件提交到仓库。（可以加-m后面输入本次提交的说明，可以输入提示内容，便于从历史记录方便的找到改动记录，`git commit` 命令执行成功会告诉你，`1 file changed` :1个文件被改动；`2 insertions` :插入了两行内容）

5. 因为`commit` 可以一次提交很多文件，所以可以多次`add` 不同的文件。    
## 基本操作

### 创建仓库的命令

| 命令     | 说明 |
| -------- | ---- |
| git init |   初始化仓库   |
| git clone[^1] | 拷贝一份远程仓库，也就是下载一个项目 |

[^1]:git clone [要拷贝的项目]

### 提交和修改

| 命令    | 说明           |
| ------- | -------------- |
| git add[^2] | 添加文件到仓库 |
| git status[^3] | 查看仓库当前的状态，显示有变更的文件 |
| git diff[^4]| 比较文件的不同，即暂存区和工作区的差异 |
| git commit | 提交暂存区到本地仓库 |
| git reset[^5] | 回退版本 |
| git rm[^6] | 删除工作区文件 |
| git mv | 移动或重命名工作区文件 |
| git checkout -- [文件名]| 撤销修改|
[^2]:git add [文件名]，将文件添加到仓库
[^3]:通常使用-s参数来获得简短的输出结果
[^4]:git diff HEAD -- [文件名] ，比较两次提交版本之间的差异
[^5]:git reset --hard HEAD^/[版本号] 指定回到上一个版本/哪个版本
[^6]:要把文件从暂存区删除，但工作区保留使用命令`git rm --cached`

### 提交日志

| 命令    | 说明                                   |
| ------- | -------------------------------------- |
| git log | 查看提交历史，以便确定要回退到哪个版本 |
| git reflog| 查看历史命令，以便确定要回到未来哪个版本 |
| git blame [文件名] | 以列表形式查看指定we年的历史修改记录 |

## 远程仓库

### 添加远程仓库

* 添加远程仓库`git remote add`，例如：`git remote add origin git@github.com:932883626/git.git` (932883626是用户，origin是Git的默认叫法)
* 使用命令`ssh-keygen -t rsa -C "github注册邮箱"` 生成SSH Key，（过程中回车即可），成功的话在~/下生成`.ssh` 文件夹，进去打开`id_rsa.pub` ，复制里面的key。
* 在GitHub界面，右上角进去settings（账户配置），左边选择`SSH and GPS keys` 然后点击`New SSH key` 设置标题，黏贴生成的key。
* 为了验证是否成功，输入一下命令：`ssh -T git@github.com` 
* 关联之后，使用命令`git push -u origin master` 就可以把本地推送到远程仓库。

### 从远程库克隆

+ 使用命令`git clone [连接地址]` 
+ 使用命令`cd [目录名]` ，进去查看克隆的文件

## 分支管理

创建分支/查看当前分支命令：`git branch`(加-d表示删除分支，在合并之后删除创建的分支即可)

切换分支命令：`git checkout`（加-b表示创建并切换）

分支合并命令：`git merge`（合并某分支到当前分支）

## 标签管理

### 创建标签

+ 切换到需要打标签的分支上面，`git checkout master`
+ 使用命令`git tag [标签名] [ID]` 就可以打一个标签，比如要对`add merge`这次提交打标签，它对应的commit id ，后面加ID号就可以。（-m可以指定说明名字）
+ 使用命令`git tag`可以查看所有标签
+ 使用命令`git show [标签名]`可以看到说明文字

### 操作标签

+ 删除标签命令`git tag -d`
+ 推送标签到远程命令`git push origin [标签名]`（一次性全部推送使用`--tags`）
+ 删除远程标签命令`git push origin :refs/tags/[标签名]`
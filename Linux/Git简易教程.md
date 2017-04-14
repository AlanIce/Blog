Git简易教程
==========

## 创建版本库

通过`git init`命令把这个目录变成Git可以管理的仓库


## 把文件添加到版本库

用命令`git add`告诉Git，把文件提交到仓库
`git add .`则是将该目录下的所有的文件或文件夹递归地提交到仓库

`git status`命令可以让我们时刻掌握仓库当前的状态

`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式

`git commit`提交修改到版本库

`git log`查看提交的历史记录


## 分支管理

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：
`git branch dev` 创建分支
`git checkout dev` 切换分支

`git branch`命令查看当前分支,该命令会列出所有分支，当前分支前面会标一个*号。

## 远程仓库（GitHub）

### 第1步：创建SSH Key。
在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

`$ ssh-keygen -t rsa -C "youremail@example.com"`

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

### 第2步：在GitHub中添加SSH Key

登陆GitHub，打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容点“Add Key”，你就应该看到已经添加的Key。

## 远程仓库的推送和抓取

`git remote add origin git@github.com:michaelliao/learngit.git`添加远程仓库

`git push -u origin master`第一次推送master分支的所有内容

`git push origin master`把本地master分支的最新修改推送至GitHub

`git clone`克隆一个本地库

### git pull 和 git fetch 的区别
1. `git fetch`相当于是从远程获取最新版本到本地，不会自动merge

    ```
    Git fetch origin master
    git log -p master..origin/master
    git merge origin/master
    ```

    以上命令的含义：
    首先从远程的origin的master主分支下载最新的版本到origin/master分支上
    然后比较本地的master分支和origin/master分支的差别,最后进行合并
    上述过程其实可以用以下更清晰的方式来进行：

    ```
    git fetch origin master:tmp
    git diff tmp 
    git merge tmp
    ```
    从远程获取最新的版本到本地的test分支上,之后再进行比较合并
    
2. `git pull`相当于是从远程获取最新版本并merge到本地
    `git pull origin master`
    上述命令其实相当于`git fetch` 和 `git merge`
    在实际使用中，`git fetch`更安全一些
    因为在merge前，我们可以查看更新情况，然后再决定是否合并

### 新建、删除远程分支
`git push origin <branch-name>` 可以把本地分支推送到远程分支
`git push --delete origin <branch-name>` 可以删除指定的远程分支

## bug分支

`git stash` 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作

`git stash list` 命令看看刚才的工作现场

`git stash apply` 恢复工作现场，但是恢复后`stash`内容并不删除，需要用`git stash drop`来删除

`git stash pop` 恢复的同时把stash内容也删了


## 只提交一次的文件

`git update-index --assume-unchanged FILENAME`忽略已经提交的文件

`git update-index --no-assume-unchanged FILENAME`重新track相关文件


## 删除远程分支和tag

在Git v1.7.0 之后，可以使用这种语法删除远程分支：
`git push origin --delete <branchName>`
可以使用这种语法，推送一个空分支到远程分支，其实就相当于删除远程分支：
`git push origin :<branchName>`

删除tag的方法，推送一个空tag到远程tag,两种语法作用完全相同。：
`git push origin --delete tag <tagname>`
`git tag -d <tagname>`
`git push origin :refs/tags/<tagname>`

## 删除不存在对应远程分支的本地分支

使用 `git remote prune origin` 可以将其从本地版本库中去除。

更简单的方法是使用这个命令，它在fetch之后删除掉没有与远程分支对应的本地分支`git fetch -p`

## 重命名远程分支

在git中重命名远程分支，其实就是先删除远程分支，然后重命名本地分支，再重新提交一个远程分支。

例如下面的例子中，我需要把 devel 分支重命名为 develop 分支：

删除远程分支：
`git push --delete origin devel`

重命名本地分支：
`git branch -m devel develop`

推送本地分支：
`git push origin develop`

## 把本地tag推送到远程

`git push --tags`

## 获取远程tag

`git fetch origin tag <tagname>`
Git简易教程
======

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
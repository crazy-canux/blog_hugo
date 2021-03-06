---
title: "Git"
date: 2016-04-02T21:11:33
categories: ["DevOps"]
tags: ["git"]
keywords: []
author: "Canux"
draft: false
---

# Git

GIT: 分布式版本控制系统。

SVN: subversion并发式版本控制系统。

Mercurial: hg分布式版本控制。

和git相关的产品：

1. github
2. bitbucket
3. sourceForge
4. gitlab

Git安装：

    $ sudo apt-get install git
    $ yum install git

# git config (Git配置)

<https://git-scm.com/book/zh/v2>

git配置文件：

1. /etc/gitconfig 系统级的配置文件，通过git config --system设置
2. ~/.gitconfig 用户级的配置文件，通过git config --global设置
3. .git/config 仓库级的配置文件，通过git config --local设置

git配置：

使用git config --global命令配置,或者直接修改~/.gitconfig文件。

    # 查看帮助
    git help config
    man git-config
    git config --help
    git config -l/--list # 查看所有配置
    git config --system
    git config --global
    git config --local
    git config --global user.name "your_name"
    git config --global user.email "your_email"
    git config --global core.editor vim

多配置文件：

在~/.gitconfig添加include可以调用其它配置文件

    [include]
        path = ~/myCode/pydeveloper/etc/git/gitcofig

***

# git remote (git 协议)

<https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E5%8D%8F%E8%AE%AE>

git有四个协议：

1. 本地协议file

        file:///path/to/project.git

2. SSH协议

    拷贝ssh的公钥后不需要输入用户名和密码

        ssh://git@github.com/<username>/<project>.git
        git@github.com:<username>/<project>.git

3. http协议

    默认需要手动输入远程仓库的用户名和密码

        https://github.com/<username>/<project>.git
        # 不用手动输入密码
        https://<username>:<password>@github.com/<username>/<project>.git

    设置http代理：

        git config --global http.proxy "http://<server>:<port>"
        git config --global http.proxy "https://<username>:<password>@<proxy>:<port>"
        git config --global https.proxy "https://<server>:<PORT>"
        # In gitconfig file
        [http]
            proxy = http://<server>:<port>

4. Git协议

        git://git@github.com/<username>/<project>.git

    设置git代理：

        git config --global core.gitproxy ...

设置git协议：

如果公司的22端口被封，不能SSH到外网，用set-url将协议改成http即可。

如果使用代理，配置http.proxy即可。

    # 查看帮助
    git remote --help
    git help remote
    man git-remote
    git remote -v # 查看远程仓库的详细信息
    git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=<fetch|push>] <name> <url>
    git remote add origin <url> # 给远程仓库origin添加url, 一个仓库可以添加多个url。
    git remote rename <oldname> <newname> # 重命名远程仓库
    git remote remove/rm <name> # 删除和远程仓库相关的所有信息
    git remote set-head
    git remote set-branches
    git remote [-v | --verbose] show [-n] <name>...
    git remote -v show origin # 查看远程仓库origin详细信息
    git remote prune
    git remote update
    git remote set-url [--push] <name> <newurl> [<oldurl>]
    git remote set-url origin <newurl> # 修改远程仓库origin的url
    git remote set-url --add [--push] <name> <newurl>
    git remote set-url --delete [--push] <name> <url>

***

# projcet/.gitignore

忽略文件

<https://github.com/github/gitignore>

# project/.gitattributes

github显示编程语言

    * linguist-language=Python

<https://github.com/github/linguist>

***

# git相关项目

## GUI

Linux：

git-gui gitk

<http://repo.or.cz/w/git-gui.git/>

    sudo apt-get install gitk

windows:

<https://git-scm.com/downloads/guis>

## git-for-windows

windows的git

<https://github.com/git-for-windows/git>

## post-git

powershell的git

<https://github.com/dahlbyk/posh-git>

## git-extras

GIT utilities -- repo summary, repl, changelog population, author commit percentages and more

<https://github.com/tj/git-extras>

    $sudo apt-get install git-extras

## git-sweep

A command-line tool that helps you clean up Git branches that have been merged into master.

<https://github.com/arc90/git-sweep>

    $sudo -E pip install git-sweep
    $cd git-repo
    $git-sweep preview # 查看哪些远程的branch已经merge到master.
    $git-sweep cleanup # 删除远程已经merge到master的branch.
    $git-sweep cleanup --skip integration, sandbox

## git-imerge

<https://github.com/mhagger/git-imerge>

## git-standup

Recall what you did on the last working day. Psst! or be nosy and find what someone else in your team did ;

<https://github.com/kamranahmedse/git-standup>

    $npm install -g git-standup

## git-lfs

git的大文件管理

<https://github.com/git-lfs/git-lfs>

## Gerrit

Gerrit is a code review and project management tool for Git based projects.

<https://github.com/gerrit-review/gerrit>

***

# 搭建私有git服务器

    $sudo groupadd git
    $sudo adduser git -g git
    $ cd /home/git
    $ mkdir src
    $ git init --bare src
    $ sudo chown -R git:git src

## gitolite

<https://github.com/sitaramc/gitolite>

## gitosis

<https://github.com/res0nat0r/gitosis>

***

# Git命令

git的结构：

1. working directory(工作目录),通过git init初始化
2. staging area(暂存区),通过git add添加index到暂存区
3. repository(仓库),通过git commit提交到repository

        git help <verb>
        git <verb> --help
        man git-<verb>

        git help -a
        git help -g
        
## archive

download one file from git server(github/gitlab/gerrit)
        
    git archive --remote=ssh://user@gerrit.domain.com:port/group/project.git HEAD:path/to file | tar -x

## init

Create an empty Git repository or reinitialize an existing one

    git init # 创建一个空repository

## add

Add file contents to the index

    git add .  # 不包括删除的文件
    git add -u # 不包括新建的文件
    git add -A # 添加working tree下的所有文件到index

## commit

Record changes to the repository

    git commit -m "[1.0.0.0]init repository."
    git commit --amend

## bisect

Find by binary search the change that introduced a bug

## branch

List, create, or delete branches

    git branch [branch-name]
    git branch -d [branch-name] # 删除已经merge到master的分支
    git branch -D [branch-name] # 强制删除分支
    git branch -m [old-name] [new-name] # 重命名
    git branch -M [old-name] [new-name] # 强制重命名
    git branch -vv   # 查看上游
    git branch --unset-upstream [branch] # 取消上游
    git branch -u <origin/branch> [branch] # 设置上游

## blame

Show what revision and author last modified each line of a file

    git blame filename # 查看文件的历史记录

## checkout

Checkout a branch or paths to the working tree

    git checkout [branch-name]
    git checkout -b [branch-name] [start-point] # 创建并切换到分支
    git checkout -B [branch-name] [start-point] # 强制执行
    git checkout -- file/path # 撤销工作目录中文件的修改
    git checkout [branch-name] -- file/path # 获取远程仓库的文件到当前工作目录

## clone

Clone a repository into a new directory

    git clone [url]

## diff

Show changes between commits, commit and working tree, etc

    git diff # 查看工作目录变化
    git diff --staged # 查看暂存区变化

## fetch

Download objects and refs from another repository

    git fetch
    git fetch -p

## grep

Print lines matching a pattern reinitialize an existing one

    git grep [<pathspec>...]

## log

Show commit logs

    git log # 显示commit的log
    git log --pretty=format:'%h' -n 1 // 查看commit id.

## merge

merge的5中策略:

* resolve
* recursive
* octopus
* ours
* subtree

Join two or more development histories together

    git merge [branch-name]
    git merge --ff # fast-forward,默认直接两个分支合并，不会产生新的commit,看不到开发分支的分叉．
    git merge --ff-only
    git merge --no-ff # no fast-forward,每次合并会创建新的commit.可以看到开发分支的分叉
    git merge --squash

## mv

Move or rename a file, a directory, or a symlink

    git mv [old] [new] # 重命名暂存区文件

## pull

Fetch from and integrate with another repository or a local branch

    git pull

## push

Update remote refs along with associated objects

    git config --global push.default simple

    git push origin --delete <branch>
    git push origin :<branch>

    git push origin --tags -f # push所有tag
    git push origin [tagname] # push单个tag
    git push origin --delete tag [tagname]
    git push origin :refs/tags/[tagname]

## rebase

Forward-port local commits to the updated upstream head

    git rebase -i [commit] # 修改提交的信息
    pick 
    reword
    edit
    squash
    fixup 
    exec
    drop
    label
    reset
    merge
    git rebase --continue
    git rebase --abort
    git rebase --skip
    git rebase --edit-todo

## reset

Reset current HEAD to the specified state

    // 硬重置, 之后的commit和 修改的文件都没了
    git reset --hard [commit] # 全部回退

    // 软重置, 之后commit没了，修改的文件还在
    git reset --soft [commit]

    git reset --mixed [commit]

    git reset --merge [commit]

    git reset --keep [commit]

## rm

Remove files from the working tree and from the index

    git rm <file> # 删除工作目录和暂存区的文件
    git rm --cached <file> # 不删除工作目录的文件,　只删除暂存区.
    git rm -r --cached <file>

## revert

Revert some existing commits

    git revert [commit] # 撤销一次merge

## reflog

Manage reflog information

    // 查看所有执行过的动作的日志
    git reflog show 

    git reflog expire

    git reflog delete

## show

Show various types of objects

    git show

## status

Show the working tree status tag Create, list, delete or verify a

    git status
    git status -s # 显示状态的简介信息, 默认是--long
    
    // 查看gitignore忽略了哪些文件。
    git status --ignored

## stash

Stash the changes in a dirty working directory away

    git stash # 暂存当前工作目录
    git stash list # 查看暂存列表
    git stash show [stash]
    git stash pop --index stash@{N} # 恢复暂存区N和工作目录
    git stash apply --index stash@{N} # 同上

## tag

Create, list, delete or verify a tag object signed with GPG

    git tag -l # 查看所有本地tag
    git tag -d v1.0.0 # 删除本地tag
    git tag -a v1.0.0 -m "release 1.0.0." -f

## submodule

Initialize, update or inspect submodules

会在项目产生.gitmodules文件，而且不被.gitignore忽略，所以不要在URL添加用户名和密码．

添加子模块：

    // push只会push创建的子模块空目录和.gitmodules文件.
    git submodule add <repository> [<path>]

克隆子模块：

    // git clone 只会clone空目录
    git submodule init
    git submodule update 
    // 或者
    git clone --recurse-submodules <sub-URL>
    // 或者
    git clone <URL>
    git submodule update --init
    // 或者
    git submodule update --init --recursive

## mergetool

Run merge conflict resolution tools to resolve merge conflicts.

    git mergetool --tool=meld

***

# Best Prictise

merge远程tag

    git checkout -b influxdata-1.5.2 master
    git pull https://github.com/influxdata/influxdb.git v1.5.2

gitignore忽略文件和目录

    path/to/file
    path/
    # 如果已经push：
    git rm -r --cached path/to/file


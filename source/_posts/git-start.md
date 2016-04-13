---
title: git 指南
date: 2016-04-06 23:54:03
tags:
- git
---

![git]( https://git-scm.com/images/logo@2x.png)
分布式、版本控制系统， 

- 为什么要使用Git？

能够对文件版本控制和多人协作开发
拥有强大的分支特性，所以能够灵活地以不同的工作流协同开发
分布式版本控制系统，即使协作服务器宕机，也能继续提交代码或文件到本地仓库，当协作服务器恢复正常工作时，再将本地仓库同步到远程仓库。
当团队中某个成员完成某个功能时，通过 `pull request` 操作来通知其他团队成员，其他团队成员能够 `code review` 后再合并代码。

- Git有哪些特性？

git 有以下两个特点。
1. 每个开发机都是一个仓库（Repository），代码的提交可以先提交到本地 ，然后再从本地
推送到另外一个远程（Remote）， 也叫做分布式开发。不像 SVN，只有服务器上有一个，开发机是一个副本，开发机必须和服务器有网连接才能提交。
2. 每次提交 （Commit）都会生成一个快照 （Snapshot），一个快照是所有被修改文件的一个副本，而不是增量（Delta)。 这决定了 git 的操作速度比快，因为每次分支间的切换不需要根据增量重新算出最终文件，而是直接从快照中提取出文件。

<!-- more -->

## git 基本使用

![](http://static.codeceo.com/images/2015/12/git-command-list.png)

 - Workspace：工作区 
 - Index / Stage：暂存区 
 - Repository：仓库区（或本地仓库） 
 - Remote：远程仓库

文件三种状态(modified, staged, committed)

### 安装

[官网： https://git-scm.com/](https://git-scm.com/)

### 配置
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
>注： github 上面的主邮箱和本地的git配置相同


### git 仓库目录

git 仓库存放在当前工作目录的 `.git` 文件夹下，主要子目录包括以下几项。
```
├── HEAD
├── branches
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── prepare-commit-msg.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```
 
`.git/objects`: 所有的对象,包括 commit、tree、blob。
`.git/refs/heads`: 所有的分支,分支是一个地址引用,每个引用指向分支中最后一次提交 。 
`.git/refs/tags`: 所有的(tags),可以 某次提交打上 ,方便以后查看。
`.git/logs/HEAD`: HEAD 的变化历史。
`.git/logs/refs/heads`: 除master分支外其他分支的HEAD变化历史。


### 忽略文件
git 的忽略文件列表在 `.gitconfig`，每个文件夹下可单独设置。

忽略文件规则：  
自动生成的中间文件，如 `.pyc`  ，确保每次提交的更改只包含自己修改的内容。


### 一、新建代码库
```
# 在当前目录新建一个 Git 代码库
$ git init

# 新建一个目录，将其初始化为 Git 代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```

### 二、配置
```
Git 的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

# 显示当前的 Git 配置
$ git config --list

# 编辑 Git 配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

### 三、增加/删除文件
```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

### 四、代码提交
```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次 commit 之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有 diff 信息
$ git commit -v

# 使用一次新的 commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次 commit 的提交信息
$ git commit --amend -m [message]

# 重做上一次 commit，并包括指定文件的新变化
$ git commit --amend   ...
```


### 五、分支
```
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定 commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个 commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete 
$ git branch -dr
```

### 六、标签
```
# 列出所有 tag
$ git tag

# 新建一个 tag 在当前 commit
$ git tag [tag]

# 新建一个 tag 在指定 commit
$ git tag [tag] [commit]

# 查看 tag 信息
$ git show [tag]

# 提交指定 tag
$ git push [remote] [tag]

# 提交所有 tag
$ git push [remote] --tags

# 新建一个分支，指向某个 tag
$ git checkout -b [branch] [tag]
```


### 七、查看信息

```
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示 commit 历史，以及每次 commit 发生变更的文件
$ git log --stat

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次 diff
$ git log -p [file]

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个 commit 的差异
$ git diff --cached []

# 显示工作区与当前分支最新 commit 之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```

### 八、远程同步
```
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```

### 九、撤销
```
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个 commit 的指定文件到工作区
$ git checkout [commit] [file]

# 恢复上一个 commit 的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次 commit 保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次 commit 保持一致
$ git reset --hard

# 重置当前分支的指针为指定 commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的 HEAD 为指定 commit，同时重置暂存区和工作区，与指定 commit 一致
$ git reset --hard [commit]

# 重置当前 HEAD 为指定 commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个 commit，用来撤销指定 commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]
```

###  十、其他

```
# 生成一个可供发布的压缩包
$ git archive
```

## git 开发规范

团队合作开发，遵循一个合理、清晰的 git 使用流程，是非常重要的。
否则，每个人都提交一堆杂乱无章的commit，项目很快就会变得难以协调和维护。

### 分支管理

**master**： 主分支，线上稳定版本。
**release**： 发布版本，可能是不稳定存在 bug
的，待稳定后合并到主分支。
**develop**： 开发分支，所有团队成员基于该分支进行开发任务（dev-aa、dev-bb）。
**feature**： 开发相对比较独立的新功能。

- **禁止**在`master`上直接开发
均`checkout`到`develop`分支做开发，一般建议在`develop`上修改Bug;
- 如果有**新需求**、**大的特性**，建议checkout一个本地分支开发；完成`feature`后，再`merge`回`develop`分支；
- 在commit提交代码前，希望再次确认代码（diff一下）修改记录。**确保只提交自己修改的代码**，禁止提交其他人修改的代码；**确保只提交应该提交的代码**，禁止提交自己调试的代码； `git log` 随时可以查到谁提交了错误的东西；
- **git commit的注释信息一定要认真写**，需要对自己的任何一个提交负责任；


相关链接：
[掘金-Git标签](http://gold.xitu.io/#/tag/Git)
[git magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/index.html)
[廖雪峰 Git 教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[团队开发 Git 流程](http://www.jianshu.com/p/87e34894a9f9)
[gitignore 模板](https://github.com/github/gitignore)



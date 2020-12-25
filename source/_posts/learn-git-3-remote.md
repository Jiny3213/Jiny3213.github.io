---
title: 重新认识git【其三】远程仓库
tags: git
date: 2020-09-04 18:13:38
---

## git remote 查看远程仓库
列出远程仓库的简称, 通常远程仓库被命名为origin
```
git remote
git remote -v # verbose 列出更繁杂的信息
```

### 添加远程仓库
```
git remote add <shortname e.g. origin> <url e.g. https://github.com/yourname/somerepo.git>
```

### 查看origin远程仓库的详细信息
```
git remote show origin
```

### 重命名远程仓库
把远程仓库名从origin改为gitee (我喜欢用这种方式来区分放在gitee中的仓库)
```
git remote rename origin gitee
```

### 删除远程仓库
```
git remote rm origin
```

### 修改远程仓库地址
```
git remote set-url origin <url>
```

## git clone 克隆仓库
当你第一次获取远程仓库时使用, 比如你要拉取vue的源码来学习
```
git clone https://github.com/vuejs/vue.git
```
克隆之后查看`git remote` , 你会发现origin远程仓库被默认添加了

## git fetch 拉取代码
当你第二次或以上获取远程仓库时使用, 比如vue的源码更新了, 你要拉取新的代码
```
git fetch <remote e.g. origin>
# 单独拉取某个分支
git fetch origin <branch name>:<local-branch-name>
```

假如你很久没有更新vue 的源码了, 你使用`git status`只会显示你添加的修改, 而不会显示你落后了远程仓库
```
On branch dev
Your branch is up to date with 'origin/dev'. # 其实你已经落后远程仓库多个commit了
```

于是你使用`git fetch origin`指令, 再次查看status
```
On branch dev
Your branch is behind 'origin/dev' by 19 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```
此时你能看到你落后远程版本库19个commit, 但实际上你的代码没有改变, `fetch`只是把远程的代码拉到本地, 并没有和本地的git仓库合并, 你还需要手动merge

但是git会在下方提示你使用git pull, pull指令相当于fetch和merge的集合体(远程仓库 => 工作区), 并不推荐使用, 因为可能你需要确认你拉回来的代码的情况, 而不是一股脑地往本地仓库中合并

使用fetch而不使用merge的话, 你将不会看到除status之外的变化

## git merge 合并远程代码
远程仓库 + 本地仓库 => 工作区 => 本地仓库提交一个merge

当你的本地仓库和远程仓库没有冲突时, git将为你自动合并代码, 但如果出现冲突, 则需要手动解决冲突才能合并

比如你正在修改a文件, 但远程仓库中的a文件被同事修改了, 此时你无法merge, git提示你需要commit或stash来储存你的代码, 否则merge操作将会复写你当前正在修改的代码

```
error: Your local changes to the following files would be overwritten by merge:
        a.txt
Please commit your changes or stash them before you merge.
```

我们尝试把a文件commit到本地代码库, 再执行merge就可以了
```shell script
git commit -a -m test && git merge
```

此时再使用`git log`指令, 会发现在远程仓库的commit后面跟上了test, 而最新的commit则是一个merge的commit, git自动为我们合并了代码冲突(可能不是我们想要的处理方式)

## git push 推送代码
将你的代码推送到远程仓库
```
git push <remote> <branch>
```
比如本博客源码存放在dev分支, 我每次部署博客之后就会使用`git push origin dev`把博客源码推送到dev分支

在github创建仓库时, 会提示你使用-u选项来完成初次push, 此后使用git push时会记住你要push的分支

```
# 第一次
git push -u origin master
# 以后
git push
```

---
title: 重新认识git【其二】查看历史记录与状态
date: 2020-09-04 12:46:41
tags: git
---
## git log 查看提交历史
```
git log --stat # 查看简要的统计信息(一些加号和减号表示该提交对前一次提交的修改)
git log --pretty=oneline # 把一个提交列在一行的简要信息
```

### 查看不同分支的提交的差异
两点语法
```
git log master..dev # 查看在 dev 分支而不在 master 分支中的提交
git log origin/master..HEAD # 查看在当前分支而不在远程仓库 master 分支中的提交

# 以下三种功能相同, 语法不同, 而后两个语法可以选择多个分支
git log refA..refB
git log ^refA refB 
git log refB --not refA
```

三点语法
```
git log master...dev # 查看在两个分支 之一 包含但又不被两者同时包含的提交
--left-right # 这个选项可以使得打印的结果标示该提交属于左右哪个分支
```

## git show 查看提交详情
使用前4个以上没有歧义的SHA-1字符串来表示某个提交
```
git show 1e08ad
```

### 基于某个提交的历史提交
`HEAD` 代表当前分支, 可以用提交对应的 SHA-1 字符串代替, 指代某个提交

`^` 标识符用于查找第一二父提交, `~` 标识符用于查找历史提交记录
```
# 合并提交的第一父提交是你合并时所在分支（通常为 master），而第二父提交是你所合并的分支（例如 dev）
# 非合并提交没有第二父提交
git show HEAD # 查看最近一次提交
git show HEAD^ # 查看最近一次提交的父提交, 在window上要用^^代替^, 或使用双引号"HEAD^"
git show HEAD^2 # 查看最近一次提交的第二父提交

# 查找历史最常用~
git show HEAD~n # 查看最近一次提交的前n次提交
```

## reflog 引用日志
每当HEAD指向的位置发生变化, 就被引用日志记录下来, 这个日志只存在于本机, 相当于linux的shell历史记录, 只有你自己能看的见
```
git reflog
```

这个命令的一个重要的用途是在你回滚版本时, 可以找回你后面的版本的SHA-1

## git status 查看状态
```
git log 
git log -s # 输出更简短的结果
git log --pretty=oneline # 一行一个提交输出更好看的结果
```

git status 中的常见描述:

`Changes not staged for commit` 代表你放在暂存区还没提交到git仓库的文件

`Untracked files` 代表你在工作区而没添加到暂存区的文件

> **注意:** git不会跟踪空目录, 添加一个空目录, `git status` 命令并不会发现到, 也不会被纳入git仓库中保存 [相关文章](https://www.cnblogs.com/cuihongyu3503319/p/11283347.html)

## git diff 列出差异
```
git diff # 工作区 <=> 暂存区
git diff <--staged | --cached> # 暂存区 <=> 最近一次commit
git diff HEAD # 工作区 <=> 最近一次commit

git diff master..dev # 列出两个分支的差异, 也可以是两个提交的差异
git diff master..origin/master # 列出本地仓库与远程仓库的差异
git diff master...test # 找出 master , test 的共有父分支和 test 分支之间的差异
```
[https://www.cnblogs.com/poloyy/p/12214435.html](https://www.cnblogs.com/poloyy/p/12214435.html)

## git grap 查找信息
```
git grap abc # 查找版本库中的abc字符串, 默认会查找工作目录的文件
```

查看某个字符串在哪个提交里引入了
```
git log -S submit --oneline # 查看sumit是什么时候引入的
```




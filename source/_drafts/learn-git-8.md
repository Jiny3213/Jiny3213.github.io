---
title: learn-git-8
tags: git
---
## 使用SHA-1 引用提交
使用前4个以上没有歧义的SHA-1字符串来表示某个提交
```
git show 1e08ad
```

## reflog 引用日志
每当HEAD指向的位置发生变化, 就被引用日志记录下来, 这个日志只存在于本机, 相当于linux的shell历史记录, 只有你自己能看的见
```
git reflog
```

## 基于某个提交的历史提交
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

## 查看不同分支的提交的差异
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

## 交互式暂存
```
git add -i
```

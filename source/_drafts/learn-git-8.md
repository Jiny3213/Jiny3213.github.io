---
title: 重新认识git【其八】查找历史与临时保存
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
这个命令的一个重要的用途是在你回滚版本时, 可以找回你后面的版本的SHA-1

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

## git stash 临时保存
当你想要切换分支去干别的事情但是又不想提交当前的工作(因为这些工作太琐碎, 你不想占用一个commit), 你就需要临时保存
```
git stash # 会立即把你工作区的改动保存, 并把工作区恢复到上一个提交的样子
git stash list # 列出所有临时保存的快照
git stash apply # 恢复最近的一个快照, 但不会删除这个快照
git stash drop # 扔掉一个快照
git stash pop # 恢复快照的同时删除快照
# 在上面3个命令后加 stash@{0} 可以指定某个快照进行操作, 快照id使用list来查询
git stash --patch # 交互式地提示你需要保存那些文件
git stash branch <new branchname> # 新建一个分支来拉取临时保存的快照
```
stash 通常只会保存已跟踪的文件
```
--include-untracked 或 -u # 保存未跟踪的文件
--all 或 -a # 保存所有文件, 包括ignore的文件
```

## git clean 清除所有未跟踪文件和空的子目录
```
-d # 递归地查找所有文件中的内容
--dry-run 或 -n # 给你提示你将会失去什么
git clean -d -n
-f # 强制删除, 当你真正觉得要清理的时候, 使用-f来代替-n
git clean -d -f
-i # interactive 交互式地删除指定的文件
```

## git grap 查找信息
```
git grap abc # 查找版本库中的abc字符串, 默认会查找工作目录的文件
```

查看某个字符串在哪个提交里引入了
```
git log -S submit --oneline # 查看sumit是什么时候引入的
```

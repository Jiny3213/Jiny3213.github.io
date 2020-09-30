---
title: 重新认识git【其八】stash 临时保存与clean 清理工作区
tags: git
date: 2020-09-25 14:14:31
---

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
有时你需要reset到上一个版本, 但是未追踪的文件还在, 这时候要用clean
```
-d # 递归地查找所有文件中的内容
--dry-run 或 -n # 给你提示你将会失去什么
git clean -d -n
-f # 强制删除, 当你真正觉得要清理的时候, 使用-f来代替-n
git clean -d -f
-i # interactive 交互式地删除指定的文件
```


---
title: 重新认识git【其七】维护一个开源项目
tags: git
---
## 什么是 pr
## 合并别人的 pr
检出别人的远程仓库
```
$ git remote add other git://github.com/other/fork-form-mine.git
$ git fetch other
$ git checkout -b newissue other/newissue
```
确定引入了那些东西
```
git log newissue --not master # 查看不在主分支中的提交, 也就是别人的提交

# diff 命令会对比 newissue 分支和 master 分支最后的结果, 然而别人fork你的 master 之后你的 master 可能往前走了几个commit, 因此要找到最初分离出去的那个 commit, 从而排除你新增的 commit 产生的差异
$ git merge-base contrib master # 找到最初分离出去的位置
36c7dba2c95e6bbb78dfa822519ecfec6e1ca649
$ git diff 36c7db
```

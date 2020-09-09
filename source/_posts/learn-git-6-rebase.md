---
title: 重新认识git【其六】变基
date: 2020-09-07 15:54:14
tags: git
---
rebase 是一种不产生分叉的 merge, 相当于把当前分支接到目标分支的后面, 所谓的基即是"基底分支", 变基即是"变换基底分支", 相当于把目标分支作为当前分支的基底分支。

变基使得提交历史更加整洁, 成一条直线, 不会产生难看的分支。
## 基础的变基操作流程
```
# 更旧的分支 -- experiment
             \ master

git checkout experiment # 当前分支
git rebase master # 目标分支
# 相当于
git rebase master experiment # 不需要切换分支


# 更旧的分支 -- master -- experiment 
# 此时 master 分支需要再进行 merge 来与 experiment 合并  

git checkout master
git merge experiment     
          
最终 experiment 和 master 分支都指向同一个提交(最后的提交)
```

## --onto
```
# 取出 client 分支，找出它从 server 分支分歧之后的补丁， 然后把这些补丁在 master 分支上重放一遍，让 client 看起来像直接基于 master 修改一样
git rebase --onto master server client
```

## 避免协作时推送rebase
与他人协作时需要谨慎使用变基操作, 最后只对不会离开你电脑的提交执行变基

比如你推送了经过变基的提交，并丢弃了别人的本地开发所基于的一些提交, 他可以使用变基来解决你的变基, 但这会让事情变得复杂
```
git pull --rebase
# 或者
git fetch # 拉取你的代码一看, 干, 你变基了, 我本地开发的"基"被你丢弃了
git rebase master # 把自己的代码变基到你变基的提交上面, git 会自动识别
```

> onto指令和协助变基操作比较难理解, 以上是我简化的理解, 可以[参考原文](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)

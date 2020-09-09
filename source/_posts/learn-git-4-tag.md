---
title: 重新认识git【其四】打标签
date: 2020-09-07 11:14:12
tags: git
---
## 列出标签
```
git tag
git tag -l `v1.8.5*` # 列出以v1.8.5开头的tag
git show v1.2 # 查看1.2标签的详细信息
```

## 创建标签
标签会默认创建在最近提交的一个commit上, 一个commit可以创建多个tag
```
# 附注标签, 可以添加message
git tag -a "v1.2" -m "some message" 

# 轻量标签
git tag v1.2
```

## 后期打标签
```
git log --pretty=oneline # 查看commit历史记录
6a16e2d2aaeff4ee5c172a4502dca64809bc2deb (HEAD -> dev, tag: v1.1, tag: v1.0, origin/dev) publish learn-git-3 # 已经打了2个tag的commit
5a3ec40f6dce52bb0ffe4f6453b5173233c39c97 save learn git 3
7d1d3eb4d99a9180fa13ebd3d26db4e61db91b34 write learn-git-2 not finish
4daa0e4b5a5053243f468b426377ad8b2322e809 'learngit-2' # 为这个commit打一个tag吧

git tag -a v0.9 4daa0e4 # 默认编辑器会弹出, 输入附加信息保存即可
git tag v0.91 7d1d3e # 轻量标签
```

## 推送标签到远程仓库
默认使用 git push 并不会推送标签, 需要手动推送
```
git push origin <tagname> # 推送某个标签
git push origin --tags # 推送所有标签
```

## 删除标签
```
git tag -d <tagname> # 本地删除标签
git push origin --delete <tagname> # 远程删除标签(不会删除本地)
git push origin :refs/tags/v0.9 # 远程删除标签(不会删除本地) 不推荐的方式
```

## 检出标签
这会使你的仓库处于“分离头指针（detached HEAD）”的状态(暂时还不完全理解) [详细](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)
```
git checkout v1.0
```

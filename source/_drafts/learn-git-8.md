---
title: learn-git-8
tags: git
---
## reflog 引用日志
每当HEAD指向的位置发生变化, 就被引用日志记录下来, 这个日志只存在于本机, 相当于linux的shell历史记录, 只有你自己能看的见
```
git reflog
```

## 查看历史记录
```
git show HEAD # 查看最近一次提交
git show HEAD^ # 查看最近一次提交的前一次提交, 在window上要用^^代替^
```

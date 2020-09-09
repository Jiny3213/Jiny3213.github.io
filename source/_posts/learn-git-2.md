---
title: 重新认识git【其二】历史记录与撤销操作
date: 2020-09-04 12:46:41
tags: git
---
## git log 查看提交历史
常见指令
```
git log dev --not master # 列出 dev 分支中 master 未包含的提交
```
待补充
## 撤销操作

### 修补 git commit 
在commit之后, 发现还有某个文件没有添加进去, 于是你需要这个commit的选项 `--amend`

```shell script
git commit -m 'add a post'
# 忘记添加图片了
git add forget.jpg
git commit --amend
```
使用这个commit指令之后, 会弹出默认的文本编辑器, 你可以在里面修改commit的message, 比如上面的`add a post`, 可以把它改为`add a post with image`

之后再使用`git log`指令查看历史记录, 发现只有`add a post with image`而没有`add a post`, 因为我们用修补了这个commit, 就不会因为遗漏某些东西而产生多余的commit了

### git reset 
暂存区 => 工作区(不会覆盖工作区)
```shell script
git reset HEAD should_not_add.md
```

### git checkout
本地仓库 => 工作区(会覆盖工作区, 谨慎操作)

当我们修改了某个文件时, 查看`git status`会有如下提示

```
(use "git restore <file>..." to discard changes in working directory)
    modified:   source/_posts/learn-git-2.md
```

其中提示你使用`restore`指令来撤销对工作区的修改, 执行这个命令可以把工作区的某个文件回退到上一个版本

在以前, 这里提示的命令是`checkout`, 很多比较旧的教程 [包括git官方教程](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%92%A4%E6%B6%88%E6%93%8D%E4%BD%9C) 都仍然认为此处会提示使用`checkout`指令

> `restore`和`switch`指令是Git 2.23引入的一个新命令, 目的是解决`checkout`指令在**分支转换**和**恢复文件**功能的耦合

#### 警告
checkout 是一个危险的指令, 你将会丢失当前对改文件的所有修改, 用起来丝般顺滑, 按下去就没了, 甚至不给你输出任何东西, 请务必谨慎使用!!!

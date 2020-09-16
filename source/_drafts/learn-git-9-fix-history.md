---
title: 重新认识git【九】改变历史提交
tags: git
---
## 改变最近一次提交
在commit之后, 发现还有某个文件没有添加进去, 于是你需要这个commit的选项 `--amend` 来修改最后一次提交

```
git commit -m 'add a post'
# 忘记添加图片了
git add forget.jpg
git commit --amend
# 或者
git commit --amend --no-edit # 不再修改commit message
```

之后再使用`git log`指令查看历史记录, 发现只有`add a post with image`而没有`add a post`, 因为我们用修补了这个commit, 就不会因为遗漏某些东西而产生多余的commit了

## 改变更多的提交
其实就是rebase自己
```
git rebase -i HEAD~3 # 修改最近的3个提交, 将会打开默认编辑器, 按照提示操作
```

- 重新排序: 改变历史提交的顺序
- 压缩提交: 多个提交压缩成一个提交 squash
- 拆分提交: edit => 提交多次 => `git rebase --continue`

## filter-branch 通过脚本的方式改写大量提交
一个例子就是你上传了password文件, 这个文件存在于以往的每一个版本里, 怎样从所有版本中删除这个文件呢
```
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
Rewrite 6b9b3cf04e7c5686a9cb838c3f36a8cb6a0fc2bd (21/21)
Ref 'refs/heads/master' was rewritten
```
--tree-filter 选项在检出项目的每一个提交后运行指定的命令然后重新提交结果。最好在测试分支中干这个事情, 防止出错

> 参考文章[https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2)

## reset 对提交
我们知道 git 在本地分为三个区域: 工作区(workspace), 暂存区(index), 本地仓库(repository), 其中HEAD指向仓库中最近的一个提交
```
git reset [选项] [某个提交]
```
### --soft
```
git reset --soft HEAD~
```
将 HEAD 指向最近一个提交的上一个提交, 而暂存区和工作区不变, 此时 commit 的话, 其效果与 git commit --amend 相似; 相当于回到执行了git add 而未执行 git commit 的状态

### --mixed (默认行为)
```
git reset [--mixed] HEAD~
```
将 HEAD 指向最近一个提交的上一个提交, 暂存区内容变为该提交的内容, 工作区不变, 相当于回到尚未执行 git add 和 git commit 的状态

### --hard (危险)
将 HEAD, 暂存区, 工作区都回滚到指定的提交, 会清除工作区的所有改动

### 通过 reset 压缩提交
通过 reset 到前几个提交, 再 commit 来压缩提交

### 对文件进行 reset
```
git reset file.txt # 默认 --mixed 
```
这个指令会从最近一个提交拿取 file.txt 放在暂存区, 相当于取消暂存的效果

```
git reset eb43bf file.txt 
```
这个指令将从指定的提交中拿取file.txt 放在暂存区


## git checkout
本地仓库 => 工作区(会覆盖工作区, 谨慎操作)

当我们修改了某个文件时, 查看`git status`会有如下提示

```
(use "git restore <file>..." to discard changes in working directory)
    modified:   source/_posts/learn-git-2.md
```

其中提示你使用`restore`指令来撤销对工作区的修改, 执行这个命令可以把工作区的某个文件回退到上一个版本

在以前, 这里提示的命令是`checkout`, 很多比较旧的教程 [git官方教程](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%92%A4%E6%B6%88%E6%93%8D%E4%BD%9C) 都仍然认为此处会提示使用`checkout`指令

> `restore`和`switch`指令是Git 2.23引入的一个新命令, 目的是解决`checkout`指令在**分支转换**和**恢复文件**功能的耦合

#### 警告
checkout 是一个危险的指令, 你将会丢失当前对改文件的所有修改, 用起来丝般顺滑, 按下去就没了, 甚至不给你输出任何东西, 请务必谨慎使用!!!

## reset 与 checkout 的差异
reset 会改变分支的历史指向, 而 checkout 只会改变 HEAD 的指向

使用 checkout 来移动分支, 使用 reset 来回溯历史

|                             | HEAD | Index | Workdir | WD Safe? |
| :-------------------------- | :--- | :---- | :------ | :------- |
| **Commit Level**            |      |       |         |          |
| `reset --soft [commit]`     | REF  | NO    | NO      | YES      |
| `reset [commit]`            | REF  | YES   | NO      | YES      |
| `reset --hard [commit]`     | REF  | YES   | YES     | **NO**   |
| `checkout <commit>`         | HEAD | YES   | YES     | YES      |
| **File Level**              |      |       |         |          |
| `reset [commit] <paths>`    | NO   | YES   | NO      | YES      |
| `checkout [commit] <paths>` | NO   | YES   | YES     | **NO**   |

[https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)


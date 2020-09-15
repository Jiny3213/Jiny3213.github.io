---
title: 重新认识git【九】改变历史
tags: git
---
## 改变最近一次提交
之前的文章已经提到过, 使用 `--amend` 选项来修改最后一次提交
```
git commit --amend --no-edit # 不再修改commit message
```

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

## reset
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

### 对文件进行 reset
```
git reset file.txt # 默认 --mixed 
```
这个指令会从最近一个提交拿取 file.txt 放在暂存区, 相当于取消暂存的效果

```
git reset eb43bf file.txt 
```
这个指令将从指定的提交中拿取file.txt 放在暂存区

### 通过 reset 压缩提交
通过 reset 到前几个提交, 再 commit 来压缩提交

### reset 与 checkout 的差异
reset 会改变分支的历史指向, 而 checkout 只会改变 HEAD 的指向

使用 checkout 来移动分支, 使用 reset 来回溯历史

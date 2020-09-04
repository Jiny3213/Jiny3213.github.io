---
title: '重新认识git【其一】git中的文件流'
tags: git
date: 2020-09-04 11:32:16
---

## git 的三个区
- 工作区: 你正在编辑的文件
- 暂存区: git add 之后文件的所处的区域
- git仓库: git commit 之后文件所处的区域

## 概览git的lifecycle
![git文件循环](learn-git-1/lifecycle.png)

## 文件在三个区中转换的指令
把工作区中的文件状态保存到暂存区


```
git add <file>
``` 

> `git add .` 快速地把所有文件状态保存到暂存区

把暂存区的文件状态保存到git仓库
```
git commit -m <message>
``` 
> 如果message含有空格, 则要使用双引号包裹, 而不是单引号!!

> `-a` 选项: a for add, 相当于自动在commit之前运行`git add`指令, 把所有已经跟踪过的文件暂存起来一起提交 


## git status 状态查询
> `-s` 选项: s for short, 可以输出更为简短的结果

git status 中的常见描述:

`Changes not staged for commit` 代表你放在暂存区还没提交到git仓库的文件

`Untracked files` 代表你放在工作区而没添加到暂存区的文件

**注意:** git不会跟踪空目录, 添加一个空目录, `git status` 命令并不会发现到, 也不会被纳入git仓库中保存[相关文章](https://www.cnblogs.com/cuihongyu3503319/p/11283347.html)

## git diff 列出差异
列出工作区与暂存区之间的差异

`--staged | --cached` 列出暂存区与最后一次提交的git仓库之间的差异

## git rm 移除文件
此指令将会把文件从工作区, 暂存区中移除, 如果在上一个版本库中存在这个文件, 回溯版本仍然可以找回这个文件, 可以使用类似.gitignore的匹配规则
```
git rm <filePath>
```

> `--cached` 选项: 只删除暂存区中的文件, 而不删除工作区中的文件, 主要用于忘记配置gitignore的情况

## git mv 移动文件
该指令也用于重命名文件
```
git mv <fileForm> <fileTo>
``` 

## .gitignore 文件
在项目根目录下创建`.gitignore`文件, 描述了git需要忽略的文件


> [本文参考文档](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)

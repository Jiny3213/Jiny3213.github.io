---
title: '重新认识git【其一】几个区域和移动指令'
tags: git
date: 2020-09-04 11:32:16
---

## git 的四个区域
- 工作区(workspace): 你正在编辑的文件
- 暂存区(index): git add 之后文件的所处的区域
- 本地仓库(repository): git commit 之后文件所处的区域
- 远程仓库(remote): 如github, gitee

<div style="background-color: #ffffff">
<img src="learn-git-1/gitcyc.png" />
</div>

## 文件在不同区域中转换
工作区 => 暂存区

```
git add <file>
git add . # 快速地把所有文件状态保存到暂存区
```

暂存区 => 本地仓库

```
git commit -m <message> # 如果message含有空格, 则要使用双引号包裹, 而不是单引号!!
git commit # 将会使用默认编辑器来编辑message

 
```

工作区 => 暂存区 => 本地仓库
```
# -a for add, 自动在commit之前运行`git add`指令, 把所有已经跟踪过的文件暂存起来一起提交
git commit -a -m <message> 
```

## git rm 移除文件
此指令将会把文件从工作区, 暂存区中移除, 如果在上一个版本库中存在这个文件, 回溯版本仍然可以找回这个文件, 可以使用类似.gitignore的匹配规则
```
git rm <filePath>
--cached # 只删除暂存区中的文件, 而不删除工作区中的文件, 主要用于忘记配置gitignore的情况
```

## git mv 移动文件
该指令也用于重命名文件
```
git mv <fileForm> <fileTo>
```

## .gitignore 文件
在项目根目录下创建`.gitignore`文件, 描述了git需要忽略的文件


> [本文参考文档](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)

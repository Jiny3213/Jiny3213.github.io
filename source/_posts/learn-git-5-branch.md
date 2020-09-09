---
title: 重新认识git【其五】分支
date: 2020-09-07 14:19:32
tags: git
---
> git的分支管理是git区分于其他版本管理系统的最重要的特性之一, 创建分支仅仅只需要创建一个指针, 而不需要复制文件, 大大方便了大型项目的管理。

## 假设你要在issue分支上修复一个问题

### 第一步 创建分支
使用 `-b` 参数快速创建分支并切换到该分支上
```
git checkout -b issue # 创建 issue 分支
=
git branch <branch name> # 创建分支
&& 
git checkout <branch name> # 切换分支
```
创建分支后我们在这个分支上修复了某个bug

### 第二步 合并分支
修复完 bug 当然要合并到 master 分支上面啦, 注意分支的主次关系
```
git checkout master # 切换到主分支
git merge issue # 与issue分支合并
```

#### 发生冲突
并不是所有合并都一帆风顺, 如果你的 master 分支与 issue 分支对同一个文件有不同的修改, 则会产生冲突, 需要手动合并
```
$ git merge issue # 尝试合并
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result. # 发生冲突, 自动合并失败

$ git status # 查看状态
On branch master
You have unmerged paths. # 存在未合并的路径
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html # 你要合并的两个分支都修改了这个文件, 导致了冲突

no changes added to commit (use "git add" and/or "git commit -a")
```

#### 使用最原始的方法解决冲突
git 会为你的冲突文件打上标记
```
<<<<<<< HEAD:index.html # 这是当前分支的内容
<div id="footer">contact : email.support@github.com</div>
======= # 这是分界线
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html # 上面是要合并的分支的内容
```
只需要把<<< === >>> 这些标记去除, 保留你真正需要的部分, git add 即可解决冲突

```
git add index.html 

git status # 再次查看状态
On branch master
All conflicts fixed but you are still merging. # git提示你冲突已经解决, 但是合并还没结束
  (use "git commit" to conclude merge)

Changes to be committed: # 你需要 commit 来完成最终的合并操作

    modified:   index.html 
```

当然, 这种原始的方法不容易操作, 因此也有不少可视化的工具可以帮助我们解决冲突, 比如idea内置的git插件, `git mergetool` 获取图形化解决方案的提示

### 第三步 删除分支
```
git branch -d issue53 # 当合并完成后, 可以删除issue分支
```

### 第四步 把这个issue的修改同步到dev分支
假设我们平时工作的分支是 dev 分支, 在 master 分支上修复的 bug 应该怎样同步到 dev 分支呢?
```
git checkour dev # 切换到开发分支
git merge master # 合并刚刚修复的bug
```
当然, 这个bug对你开发没有影响的话, 也可以等 dev 分支要合并的时候直接将 dev 合并到master, 还省下一步

## git branch 查看所有分支
```
git branch # 查看所有分支
git branch --no-merged # 查看未合并的分支(基于当前分支)
git branch --no-merged master # 查看未合并的分支(基于master)
```


> [本文参考文章](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

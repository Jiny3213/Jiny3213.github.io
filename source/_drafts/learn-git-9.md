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

## filter-branch 通过脚本的方式改写大量提交
一个例子就是你上传了password文件, 这个文件存在于以往的每一个版本里, 怎样从所有版本中删除这个文件呢
```
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
Rewrite 6b9b3cf04e7c5686a9cb838c3f36a8cb6a0fc2bd (21/21)
Ref 'refs/heads/master' was rewritten
```
--tree-filter 选项在检出项目的每一个提交后运行指定的命令然后重新提交结果。最好在测试分支中干这个事情, 防止出错

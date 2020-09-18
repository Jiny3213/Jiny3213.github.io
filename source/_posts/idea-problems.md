---
title: idea 相关问题汇总
tags: 环境配置
date: 2020-09-17 10:19:21
---

## 在 idea 终端中使用 git 无法显示中文
中文显示如下
```
<E4><BB><80><E4><B9><88><E6><98><AF>
```
## 解决方法1
直接在终端运行命令, 这个方法的缺点是下次打开项目的时候就失效了
```
set LESSCHARSET=utf-8
```

## 解决方法2
打开 idea 的 setting 手动配置, 这个方法的缺点是打开另一个项目就失效了
settings => Tools => Terminal => Environment Variables => 填入`LESSCHARSET=utf-8` 或按右侧图标添加
[参考文章](https://blog.csdn.net/Xu_XiaoXiao_Ji/article/details/107719176)

## 终极解决方案
上面两个方法都是治标不治本, 打开 setting 在方法二的那个位置, 右侧会有一个图标, 点开发现 idea 能够自动引入系统变量

只要把 `LESSCHARSET=utf-8` 写进系统变量即可在所有项目中解决这个问题

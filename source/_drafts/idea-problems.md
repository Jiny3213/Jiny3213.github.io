---
title: idea 相关问题汇总
tags: 环境配置
---
## 在 idea 终端中使用 git 无法显示中文
中文显示如下
```
<E4><BB><80><E4><B9><88><E6><98><AF>
```
解决方法:

settings => Tools => Terminal => Environment Variables => 填入`LESSCHARSET=utf-8` 或按右侧图标添加

[参考文章](https://blog.csdn.net/Xu_XiaoXiao_Ji/article/details/107719176)

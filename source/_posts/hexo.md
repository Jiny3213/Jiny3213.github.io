---
layout: post
title: 从零开始使用 hexo 博客
date: 2020-09-02 14:38:16
tags: 
- 划水
categories: 摸鱼
---
## 写博客的经历
一直都想写自己的博客，最早在博客园写过一两篇，后来喜欢用md来写文章，于是去掘金写过几篇文章。但是奈何我写的文章几乎没有阅读量，也没有人来互动，于是渐渐就没有了写文的动力。

但是在日常学习中经常会深入一些问题，不写个文章记录一下总觉得可惜，同时也可以方便自己以后翻找（经常有些技术问题曾经研究的很深入，后来忘记了，又要再研究一遍）。

于是我开始思考博客的本质，既然是写给自己看的，就应当抛弃自己的文章可能会被很多人阅读（不存在的）的虚荣心，专注于学习技术和记录本身，才能够更加纯粹地写文章。希望以博客记录自己的学习轨迹，回过头来看的时候也不会觉得自己浪费了大好时光。

于是开始尝试搭建自己的博客，几个月前也曾经用过hexo，当时由于图片引用插件的bug不会修复，再加上想做成动态网站可以结合服务器实现更多事情，就暂时放下了hexo。但是苦于找不到一个正在维护中的md富文本编辑器，自己写md转换规则又力不从心，于是又开始搁置写博客的计划。

最后还是想以最简单的方式写文章，静态网站就好了，有回到了hexo。

言归正传，开始初始化博客吧！teeest

## 初始化hexo博客

```
npm install hexo-cli -g
hexo init my-blog
cd my-blog
npm run serve
```

## 配置文件
使用hexo博客之前, 先要通读一遍配置文件的选项, 了解有哪些可配置项: [文档地址](https://hexo.io/zh-cn/docs/configuration)

不了解yml文件的话请看: [阮一峰老师介绍yml文件非常详细](http://www.ruanyifeng.com/blog/2016/07/yaml.html)

打开下面这个选项后, 将为每个文章创建一个文件夹, 可以把图片等资源放在其中, 集中管理一篇文章的静态资源, 也是启用后面的图片插件的前置条件

```_config.yml
post_asset_folder: true
```

## 创建一个文章
```
hexo new <layout> <title>
```
> hexo会根据你创建文章时的title来命名文件名和路由, 如果不想用中文做路由的话, 创建时不要用中文, 至于文章的标题创建之后再改为中文即可

一共有三种默认layout, 对应`/scaffolds`中的三个模板文件

- post: 最常用的layout, 创建之后可以直接在博客首页看到文章
- draft: 草稿, 创建之后不能直接在首页看到, 可以配置config文件来显示, 也可以用`hexo publish <layout> <filename>`的方式来发布
- page: 页面, 创建后不能直接看到, 需要配置主题下的config文件, 添加到menu中, 就能够显示在页面的菜单栏中

## 使用图片
尝试插入一张图片, 要不就hexo预览无法显示, 要不就本地编辑器无法显示, 怎样才能两边都看到呢

![yyx](hexo/yyx.jpg)

安装hexo-asset-image插件
```shell script
npm install https://github.com/CodeFalling/hexo-asset-image --save
```
为什么不直接`npm install hexo-asset-image` 呢?

因为这样安装的是1.0.0版本, 存在bug导致图片无法正常显示, 这样装的插件需要修改源码来修复这个bug[修改源码参考](https://blog.csdn.net/xjm850552586/article/details/84101345)

而直接从github安装的话, 版本是0.0.5, 这个才是最终的版本, 可能是因为作者没有更新npm包，因此直接从github链接安装会比较方便 [关于问题产生原因](https://www.jianshu.com/p/db02d775aed0)

目前这个插件的github仓库已被作者封存, 作者是国人, 代码提交仍然活跃, 至于为何要封存这个库就不得而知了 

## 尝试使用一些插件

### 插件实现引用块
{% blockquote %}
是这是hexo的语法，在常规md编辑器中无法显示这样的格式
默认皮肤中的引用格式是居中显示，个人不喜欢
{% endblockquote %}

### 插入pull Quote (靠左的引用)
{% pullquote %}
同样是使用blockquote标签, 不同于上面的是, 这里会加上一个pullquote的class, 使得文字靠左排列, 但默认皮肤仍然难以让人满意
{% endpullquote %}

### 使用HTML标签
<a href="https://baidu.com">手写一个a标签, 在hexo和md编辑器中都兼容</a>

使用img标签 <img src="https://www.baidu.com/img/flexible/logo/pc/result.png"/>

### 插入jsFiddle 
- 这是一个官方自带的在线代码编辑器插件，大概要先在jsFiddle上上传代码示例才能拿到链接
`{% jsfiddle shorttag [tabs] [skin] [400] [400] %}`

### 使用gist
gist 是轻量化的带有git版本控制的文本管理系统，可以方便地引用到各种地方

{% gist caca3d0cdb2ab0c5ed545428e008ac59 gistfile1.txt %}

> hexo插件的方式和无法在md编辑器中显示, 离开了hexo就无法正常显示, 因此还是推荐使用原生md语法来写作

> 最后立一个flag，每周至少更新一个文章（认真）。

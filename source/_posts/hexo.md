---
layout: post
title: 初始化 hexo 博客
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

言归正传，开始初始化博客吧！

## 初始化hexo博客

- `npm install hexo-cli -g`
- `hexo init my-blog`
- `cd my-blog`
- `npm run server`

## 创建第一篇文章
- 创建文章前，先编辑`_config.yml`文件配置一下
    - 不了解yml文件的话请看: [阮一峰老师介绍yml文件非常详细](http://www.ruanyifeng.com/blog/2016/07/yaml.html)
    - 配置`post_asset_folder: true # 为每个文章创建一个文件夹, 可以把图片放在其中, 方便管理`方便后续管理图片
- `hexo new post my-post`

## 使用图片
- 尝试插入一张图片, 要不就hexo预览看不到, 要不就本地看不到, 怎样才能两边都看到呢
- ![hexo](hexo/yyx.jpg)
- 需要安装hexo-asset-image插件, 但是有bug, 需要修改其源码 
    - [解决方法1](https://blog.csdn.net/xjm850552586/article/details/84101345)
    - [问题产生原因及解决方法2](https://www.jianshu.com/p/db02d775aed0)
    - 不知为何这个插件的github被作者封存了, 最终版本是0.0.5, 但通过`npm install`下载的是1.0.0版本, 可能是因为作者没有更新npm包吧，因此直接从github链接安装会比较方便

## 尝试使用标签插件——实现引用块
{% blockquote David Levithan, Wide Awake %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}
> 是这是hexo的语法，在常规md编辑器中无法显示这样的格式
> 默认皮肤中的引用格式是居中显示，个人不喜欢，以后搞个皮肤吧

## 插入一段代码
```javascript title https://baidu.com 跳转到百度
console.log(111)
```
> 代码可以很好地使用md格式

## 插入pull Quote
{% pullquote %}
同样是使用blockquote标签, 不同于上面的是, 这里会加上一个pullquote的class, 使得文字靠左排列
{% endpullquote %}

## 使用HTML标签
<a href="https://baidu.com">手写一个a标签, 在hexo和md编辑器中都兼容</a>

但我为什么不用md格式的链接呢?

## 插入jsFiddle 
- 这是一个官方自带的在线代码编辑器插件，大概要先在jsFiddle上上传代码示例才能拿到链接
`{% jsfiddle shorttag [tabs] [skin] [400] [400] %}`

## 使用gist
gist 是轻量化的带有git版本控制的文本管理系统，可以方便地引用到各种地方

{% gist caca3d0cdb2ab0c5ed545428e008ac59 gistfile1.txt %}

> hexo插件的方式和无法在md编辑器中显示, 破坏了md的格式美感(笑)

> 最后立一个flag，每周至少更新一个文章（认真）。

---
title: 在既有项目中引入 vuetify 的问题
tags: 前端
date: 2020-09-15 16:26:31
---

## 背景
假设你有一个既有的项目(这个项目还没引入过ui组件库), 需要使用pagination这个组件, 你可能会想到element的按需引入, 于是你尝试按需引入pagination, 发现总会报错, 但是引入按钮就啥事都没有; 其实并不需要考虑单独引入, tree-sharking默认会在vue-cli插件中开启

## 安装
于是你开始使用 `vue add vuetify` 安装vuetify插件, 插件给你三个选项:
- 推荐的方式
- 原型
- 手动添加

### 推荐的方式
你选择了推荐的方式, vuetify把你的app.vue给覆盖了! 很生气, 于是你回退版本

### 手动添加的方式
你选择了手动添加的方式, lang选择了中文, 避免了覆盖app.vue和创建没用的HelloWord.vue, 但是你发现i18n中缺少pagination相关的图标, 你的pagination没有了左右箭头, active状态还没有颜色, 而且你的vue.config.js中的配置被删除了一部分! 

你很生气, 把vuetify.js中的选项清空了, 这次看到左右箭头了, 但是pagination的active状态还是没有颜色, 设置color也不能改变颜色; 你开始怀疑人生, 打开了以前使用的vuetify项目(这个项目从一开始就使用vuetify), 在这个项目中使用pagination, 一切如常

查阅资料发现, pagination的active颜色建立在根元素使用的v-app上, 但是你不想改变原有的项目, 于是你回退版本

## 原型的方式
再试试第二个选项吧, 第二个选项还是改变了你的app.vue并添加了HelloWorld, 但他没有修改你的vue.config.js, 而且没有通过cdn的方式来引入样式和字体, 而是直接在main.js中引入, 而且没有使用vuetify-loader; 在vuetify.js中引入的路径也不一样; 这些配置似乎都很奇怪, 但又不知道奇怪在哪, 而且这仍然没有解决你的问题

破罐子破摔, 你把app.vue的根元素换成了v-app, 哈哈, pagination有颜色了; 但是你项目中的某些元素由于全局的样式而改变了, 导致很多地方页面崩坏, 你又不得不把v-app去掉

最后, 祭出最终武器`::v-deep`来手动改变pagination的背景颜色, 你感觉很恶心, 为了用这个小组件, 引入了一个巨大的ui库, 还要手动修改样式, 这什么鬼

但是如果后续要使用更多组件的话还是值得的

> 总结: 在既有项目上使用vuetify会比较麻烦, 因为其对项目的侵入性很大, 而且会覆盖掉你的部分文件, 务必保存你的工作内容再引入vuetify!!

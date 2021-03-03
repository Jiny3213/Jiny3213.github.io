---
title: vue2高阶组件
date: 2021-02-01 16:17:59
tags: 前端
---

## v-model
v-model 是 vue 的一个标志性功能, 通过一个简单的指令即可完成双向绑定到输入组件, 不再需要 setState 等繁琐的绑定操作。
v-model 通常被用在 input, checkbox, radio 等原生 html 元素上, 通过高级组件化的应用, 可以把 v-model 使用在自己的组件上, 使得自己的输入组件具有原生的 v-model 绑定体验, element-ui中的组件就是这样实现的。 
```
<el-input v-model="username">
```
> 参考文档: [vue官方文档](https://cn.vuejs.org/v2/guide/components-custom-events.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E7%9A%84-v-model)

### v-model 双向绑定规则
v-model 默认使用 input 事件和 value 属性进行双向绑定, 即监听组件的 input 事件, 把事件返回的值绑定到 v-model 指定的值中, 同时当 v-model 绑定的值发生变化时, 将其值反映到输入组件中, 从而做到双向绑定。

如果不使用 v-model 的话, 我们可以这样进行双向绑定, 和 v-model 做的事情一样, 可以说, v-model 只是一个语法糖, 帮助我们更简洁地进行双向绑定
```vue
<input v-model="username">
// 等同于
<input :value="username" @input="username = $event">
```


```
// 默认的情况
  model: {
    prop: 'value',
    event: 'input'
  } 
```

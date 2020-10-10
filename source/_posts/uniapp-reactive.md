---
title: 在uniapp中完善vue的响应式
date: 2020-10-10 11:33:14
tags: 前端
---
## 前言
在uniapp使用vue的api, 使得小程序的开发更加贴近vue原生开发, 摆脱了原生小程序开发一个页面需要在四个文件中左右横跳的问题, 但仍然有一些不足需要我们手动优化, 比如还不支持v-model双向绑定, globalData缺乏响应式等

## 双向绑定
由于不能使用v-model, 因此需要手动绑定

```
// html
<input @click="handleInput" :value="someData"/>

// js
data() {
  return {
    someData: ''
  }
},
methods: {
  handleInput() {
    this.someData = e.detail.value
  }
}
```

## 响应式的globalData
使用getApp().globalData来存放全局数据, 但是却没有响应式

假设我们需要根据globalData中的hasLogin字段来判断用户登录组件是否显示, 已vue的思路我们使用一个computed
```
// html
<login-component v-if="!hasLogin">

// js
computed: {
  hasLogin() {
    return getApp().globalData.hasLogin  
  }
}
```

如果我们在其他地方实现了登录, 改变了hasLogin的值, 这个组件并不会响应式地知道登录状态改变了, 因为globalData没有响应式, 这时我们就要用到一个比较底层的vue的api来实现globalData的响应式了

```
// App.vue
globalData: Vue.observable({
  hasLogin: false
})
```

vue在底层实现响应式就是通过这个api, 平时使用vue开发的时候完全没有用过, 毕竟常规的地方都已经是响应式的了

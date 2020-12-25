---
title: js模块标准总结
date: 2020-12-25 17:37:11
tags:
---


想将ts作为工具引入到使用vue2.x的项目中, 而项目仍然使用js开发, 引入ts只是为了获得api的类型推断, 于是考虑为api写一个声明文件; 在阅读ts声明文件的文档时发现理解不了各种模块化规范, 于是总结了以下的js模块化规范

> 参考文章 [https://www.davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/](https://www.davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/)
> [https://dev.to/iggredible/what-the-heck-are-cjs-amd-umd-and-esm-ikm](https://dev.to/iggredible/what-the-heck-are-cjs-amd-umd-and-esm-ikm)
> http://blog.chinaunix.net/uid-26672038-id-4112229.html 底部的相关链接非常有价值
## CommonJs

node使用CJS的模块标准, 适用于后端的场景

```
//importing 
const doSomething = require('./doSomething.js'); 

//exporting
module.exports = function doSomething(n) {
  // do something
}
```

## AMD (Asynchronous Module Definition) 异步模块定义

适用于前端的场景, 需要 amd loader, 所以在console中 define 是 undefined 的

RequireJS 是一个遵循AMD规范的模块化管理工具库

```javascript
define(['dep1', 'dep2'], function (dep1, dep2) {
    //Define the module value by returning a value.
    return function () {};
});
```

### CMD (Common Module Definition)

seaJS 遵循CMD规范

## UMD (Universal Module Definition) 通用模块定义

支持以上两种风格的通用模式, 适用于前后端

```
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery'], factory);
    } else if (typeof exports === 'object') {
        // Node, CommonJS-like
        module.exports = factory(require('jquery'));
    } else {
        // Browser globals (root is window)
        root.returnExports = factory(root.jQuery);
    }
}(this, function ($) {
    //    methods
    function myFunc(){};

    //    exposed public method
    return myFunc;
}));
```

## ESM (ES Module)

只有部分浏览器支持

```
import vue from 'vue'

export default {}

export const a = 1
```

> AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
> CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

## 回到最开始的问题

怎样在vue项目中引入ts工具链? 我使用的是WebStorm

安装 `@typescript-eslint/eslint-plugin`, `@typescript-eslint/parser`, `typescript` 开发依赖

修改 .eslintrc
```
parserOptions: {
    // parser: 'babel-eslint'
    parser: '@typescript-eslint/parser'
},
```

在api文件的同级目录下引入声明文件
```
login.js // 原来的api
login.d.ts // 声明文件

interface LoginOptions {
    username: string,
    pwd: string
}
export function Login(options: Login): any;
```

这样就可以在vue文件中获取类型了, 但这样做你要为 login.js 中导出的每一个方法声明, 否则没有声明的方法将会无法跳转定义
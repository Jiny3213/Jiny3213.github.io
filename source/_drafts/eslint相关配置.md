---
title: eslint相关配置
tags: 
---
## 禁止单个文件的eslint检查(文件顶部)
```
/* eslint-disable */
```

## 禁止一段代码的检查
```
/* eslint-disable */
alert('foo');
/* eslint-enable */
```

## 禁用单行检查
```
consle.log("foo"); // eslint-disable-line
```

## 禁用下一行检查
```
// eslint-disable-next-line
alert('foo');
```

## 禁用 ts 检查
```
// @ts-ignore
```
> 似乎乱入了什么


---
title: nest-解决循环依赖
date: 2020-12-11 14:48:35
tags: nest
---

> [https://docs.nestjs.com/fundamentals/circular-dependency](https://docs.nestjs.com/fundamentals/circular-dependency)

## 什么是 barrel files ?
A barrel is a way to rollup exports from several modules into a single convenience module. The barrel itself is a module file that re-exports selected exports of other modules.

即是一个文件把所有依赖的文件 import 进来, 再作为一个模块统一 export 出去

桶文件可能会引起循环依赖

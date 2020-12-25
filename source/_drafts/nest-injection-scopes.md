---
title: nest-依赖注入的范围
date: 2020-12-11 12:48:08
tags: nest 后端
---
## nest 的 Provider 有三种 scope

1. DEFAULT: 只有在应用创建的时候被实例化一次, 直到应用结束才被销毁
2. REQUEST: provider 的实例在请求的时候创建, 在请求结束的时候销毁
3. TRANSIENT: 一个短暂的实例, 不会与任何 consumer share

> controller 也有 scope

## scope 层级及其影响

`CatsController <- CatsService <- CatsRepository`

当 CatsService 的 scope 是 request 时, CatsController 的 scope 也会变成 request, 而 CatsRepository 将会保持不变

## 性能

request-scoped 的 provider 由于会在每次请求入站的时候创建实例, 因此性能会有所降低

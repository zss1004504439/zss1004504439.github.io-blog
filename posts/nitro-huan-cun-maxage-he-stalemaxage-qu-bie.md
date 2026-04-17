---
id: yncnnH
title: Nitro 缓存 maxAge 和 staleMaxAge 区别
createdAt: "2023-11-06 13:57:34"
updated: "2026-04-17 14:00:54"
tags:
    - Nitro
    - 缓存
tag_ids:
    - ruSqJm
    - AqlvyI
categories: []
published: true
hideInList: false
feature: ""
isTop: true
---

简单理解：
- **maxAge**：缓存正经“保质期”。
  这段时间内，数据直接用，不用重新计算/请求，干净又新鲜。

- **staleMaxAge**：过期“宽限期”。
  过了 maxAge 但还在 staleMaxAge 里，先给你旧数据用着，后台悄悄去更新，不卡请求、不报错。

一句话总结：
**maxAge 管新鲜多久，staleMaxAge 管过期了还能先用多久。**

实际用的时候，一般这么配：
先设一个正常更新周期 `maxAge`，再给一段容错时间 `staleMaxAge`，既能保证速度，又不至于在服务波动时直接崩掉。
```js
export default defineCachedFunction(
  () => {
    // 生成响应的逻辑
    return "Cached response";
  },
  {
    maxAge: 60, // 缓存1分钟（60秒）
    staleMaxAge: 300, // 过期后仍可用5分钟（300秒）
  }
);
```
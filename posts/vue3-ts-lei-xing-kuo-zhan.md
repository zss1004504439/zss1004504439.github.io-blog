---
id: Z8c0um
title: vue3 ts 类型扩展
createdAt: "2021-05-31 10:37:51"
updated: "2026-04-17 11:19:41"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

```js
declare module '@vue/runtime-core' {
  interface ComponentCustomProperties {
    $test: number
  }
}
```
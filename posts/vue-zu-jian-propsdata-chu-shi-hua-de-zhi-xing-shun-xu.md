---
id: zWGs56
title: vue组件 props、data 初始化的执行顺序
createdAt: "2020-11-16 13:33:17"
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
beforeCreate  ->inject -> Props ->  Methods ->  Data -> Computed -> Watch ->provide-> created
```
```js
      console.log('🚀  this', this)
      Object.assign(this.$data, this.$options.data.call(this))
      console.log('🚀  this', this)
```
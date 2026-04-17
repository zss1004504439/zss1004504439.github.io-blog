---
id: KJdYL1
title: $options.data 重置data
createdAt: "2021-06-22 09:44:42"
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
      console.log('🚀  this', this)
      Object.assign(this.$data, this.$options.data.call(this))
      console.log('🚀  this', this)
```
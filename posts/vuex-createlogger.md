---
id: a25Jwl
title: vuex createLogger
createdAt: "2021-06-30 14:02:26"
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
import Vuex, { createLogger } from 'vuex'
const debug = process.env.NODE_ENV !== 'production'
export default new Vuex.Store({
    // ...
    plugins: debug ? [createLogger()] : []
})
```
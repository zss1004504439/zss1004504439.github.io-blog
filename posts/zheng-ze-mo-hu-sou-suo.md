---
id: PpEZiX
title: 正则 模糊搜索
createdAt: "2021-07-11 00:38:35"
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
//value就是要搜索的值
let str = ['',...value,''].join('.*'); 
let reg = new RegExp(str);
let newData = data.filter(item => reg.test(item.title));
```
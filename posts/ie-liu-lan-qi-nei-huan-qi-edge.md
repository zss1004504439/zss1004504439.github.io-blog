---
id: AgvBIk
title: ie浏览器内唤起edge
createdAt: "2021-04-28 14:23:41"
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
if (isWin10IE) {
    window.location.href = `microsoft-edge:${document.URL}`;
}
```
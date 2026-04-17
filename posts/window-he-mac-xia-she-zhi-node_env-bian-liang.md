---
id: x1hkez
title: window和mac下设置NODE_ENV变量
createdAt: "2021-03-10 09:45:52"
updated: "2026-04-17 11:19:41"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## mac `export`设置变量
```json
"scripts": {
   "start": "export NODE_ENV=development && nodemon -w src --exec \"babel-node src\"",
   "build": "babel src --out-dir dist",
   "run-build": "node dist",
 }
```
## window `set`设置变量
```json
"scripts": {
   "start": "set NODE_ENV=development && nodemon -w src --exec \"babel-node src\"",
   "build": "babel src --out-dir dist",
   "run-build": "node dist",
 }
```
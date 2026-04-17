---
id: tqYyL6
title: axios 兼容 promise 浏览器
createdAt: "2019-09-29 10:00:27"
updated: "2026-04-17 11:19:42"
tags:
    - axios
    - Vue cli 配置
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## 引入 babel-polyfill
```
npm install --save babel-polyfill
```
```js
// vue.config.js
module.exports = {
    configureWebpack: config => {
        return {
            entry: {
                app:['babel-polyfill', './src/main.js']
            }
        }
    }
} 
```
---
id: qsPVp1
title: create-react-app typescript模板
createdAt: "2021-04-23 10:10:29"
updated: "2026-04-17 11:19:41"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## 解决别名配置问题
```json
// tsconfig.json
{
    ...
    "extends": "./paths.json"
}
```
```json
// paths.json
{
    "compilerOptions": {
        "baseUrl": "src",
        "paths": {
            "@/*": ["*"]
        }
    }
}
```
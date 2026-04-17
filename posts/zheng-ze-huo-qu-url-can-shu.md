---
id: 0juTZA
title: 正则获取url参数
createdAt: "2019-09-10 11:46:00"
updated: "2026-04-17 11:19:43"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

``` javascript
function query(url){
  let reg=/([^=?&]+)=([^=?&]+)/g,
  obj={};
  url.replace(reg,function(){
    obj[arguments[1]]=arguments[2]
  })
  return obj
}

query(location.href)
```
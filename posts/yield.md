---
id: mUq5wW
title: yield
createdAt: "2021-07-27 22:06:11"
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
function* search(node){ 
    if(!node){
         return 
         };  
         yield node; 
         yield* search(node.firstChild); 
         yield* search(node.nextSibling); 
}
for (let node of search(document)){ 
    if(node.localName === 'title')
    { console.log(node.textContent());
    break; 
    } 
}
```
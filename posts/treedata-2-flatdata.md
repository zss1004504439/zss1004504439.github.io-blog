---
id: rFWI2Q
title: treeData 2 flatData
createdAt: "2020-08-26 17:46:04"
updated: "2026-04-17 11:19:42"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

```js
let res = []        // 用于存储递归结果（扁平数据）
// 递归函数
const fn = (source)=>{
    source.forEach(el=>{
        res.push(el)
        el.children && el.children.length>0 ? fn(el.children) : ""        // 子级递归
    })
}
 
// 树形数据
const arr = [
    { id: "1", rank: 1 },
    { id: "2", rank: 1,
        children:[ 
            { id: "2.1", rank: 2 },
            { id: "2.2", rank: 2 } 
        ] 
    },
    { id: "3", rank:1,
        children:[ 
            { id: "3.1", rank:2, 
                children: [ 
                    { id:'3.1.1', rank:3,
                        children:[ 
                            { id: "3.1.1.1", rank: 4, 
                                children:[
                                    { id: "3.1.1.1.1", rank: 5 }
                                ] 
                            } 
                        ] 
                    } 
                ] 
            } 
        ] 
    }
]
 
fn(arr)             // 执行递归函数
console.log(res)    // 查看结果
```
![https://img-blog.csdnimg.cn/20191107094651823.png](https://img-blog.csdnimg.cn/20191107094651823.png)
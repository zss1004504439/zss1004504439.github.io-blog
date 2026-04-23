---
id: cuMT6k
title: throw 可以抛任何东西
createdAt: "2024-04-23 09:45:49"
updated: "2024-04-23 09:45:49"
tags: []
tag_ids: []
categories: []
published: false
hideInList: false
feature: ""
isTop: false
---

在 JavaScript 中：
```
// 全部都合法！！！
throw 123;
throw "字符串";
throw { name: "对象" };
throw new Response(); // 注意这个
throw null;
throw undefined;
```
**没有任何语法限制！**
平时我们抛 throw new Error("xxx") 只是习惯，不是规定。

```js
try {
  await yourHandler();
} catch (err) {
  // 如果 catch 到的是 Response，直接返回给前端
  if (err instanceof Response) {
    return err; 
  } else if(xxx){
    return xxx
  }
  // 否则才当成真正的错误处理
}
```
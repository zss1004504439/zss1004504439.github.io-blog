---
id: Gd5i1h
title: axios记录请求响应时间
createdAt: "2026-06-09 10:54:55"
updated: "2026-06-09 10:54:55"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

在网页客户端中，也存在与文章中 Node.js 接入层类似的场景，下面从中间件顺序、请求态传递、限流策略三个方面为你详细分析：

### 中间件顺序
在网页客户端中，请求拦截器和响应拦截器的执行顺序就类似于 Node.js 接入层的中间件顺序。例如在使用 Axios 进行网络请求时，拦截器的注册顺序会影响其执行顺序。

```javascript
import axios from 'axios';

// 创建一个 Axios 实例
const instance = axios.create();

// 第一个拦截器：添加请求头
instance.interceptors.request.use(config => {
    config.headers['Authorization'] = 'Bearer token';
    return config;
});

// 第二个拦截器：记录请求时间
instance.interceptors.request.use(config => {
    config.metadata = { startTime: new Date() };
    return config;
});

// 响应拦截器：记录响应时间
instance.interceptors.response.use(response => {
    const endTime = new Date();
    const startTime = response.config.metadata.startTime;
    console.log(`Request took ${endTime - startTime}ms`);
    return response;
});

// 发起请求
instance.get('https://api.example.com/data')
   .then(response => {
        console.log(response.data);
    })
   .catch => {
        console.error(error);
    });
```
在上述代码中，请求拦截器的执行顺序是按照注册顺序依次执行的，响应拦截器也是如此。如果顺序错误，可能会导致请求头添加失败或者时间记录不准确等问题。
 
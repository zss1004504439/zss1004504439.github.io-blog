---
id: VKZ84N
title: res.finish vs 重写 res.end
createdAt: "2025-04-23 14:50:27"
updated: "2026-04-23 18:52:49"
tags:
    - nodejs
tag_ids:
    - Z2j88k
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## 监听 res.finish 事件
res.finish 是 Node.js HTTP 原生事件，响应数据全部发送到客户端后触发，无需修改原生 API，安全规范。
```js
export function logger() {
  return async (req, res, next) => {
    const start = Date.now();
    const { method, url } = req;

    await next();

    res.on('finish', () => {
      const duration = Date.now() - start;
      const status = res.statusCode;
      const color = status >= 500 ? '\x1b[31m' : status >= 400 ? '\x1b[33m' : '\x1b[32m';
      console.log(`${color}${status}\x1b[0m ${method} ${url} — ${duration}ms`);
    });
  };
}
```

## res.end 是发送响应的核心方法，重写它可在响应发送前，捕获传入的响应数据（chunk），获取完整业务返回内容。

```js
export function logger() {
  return async (req, res, next) => {
    const start = Date.now();
    const { method, url } = req;
    const originalEnd = res.end; // 保存原生方法

    res.end = function (chunk, encoding) {
      res.end = originalEnd; // 立即恢复，避免污染
      const result = originalEnd.call(this, chunk, encoding);
      
      const responseBody = chunk?.toString(encoding) || '';
      const duration = Date.now() - start;
      const status = res.statusCode;
      const color = status >= 500 ? '\x1b[31m' : status >= 400 ? '\x1b[33m' : '\x1b[32m';
      
      console.log(`${color}${status}\x1b[0m ${method} ${url} — ${duration}ms`, '响应体：', responseBody);
      return result;
    };

    await next();
  };
}
```
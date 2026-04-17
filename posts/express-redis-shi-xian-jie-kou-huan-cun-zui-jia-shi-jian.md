---
id: mxjx7Q
title: express + redis 实现接口缓存最佳实践
createdAt: "2025-03-05 14:23:31"
updated: "2025-03-05 14:23:31"
tags:
    - redis
    - 缓存
tag_ids:
    - R7tksN
    - AqlvyI
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

在 Node.js 环境下，使用 **Express** 配合 **Redis** 进行接口缓存是提升系统性能、降低数据库压力的经典方案。

以下是实现这一机制的最佳实践指南。

-----

## 1\. 核心架构逻辑

接口缓存的基本逻辑是 **“命中即返回，不中即查询并存入”**。

1.  **客户端请求**：进入中间件。
2.  **查询 Redis**：检查是否存在对应的 `Key`（通常由 URL 和参数组成）。
3.  **命中（Cache Hit）**：直接从 Redis 返回数据，请求结束。
4.  **未命中（Cache Miss）**：执行业务逻辑（查询数据库），将结果存入 Redis（设置过期时间），然后返回给客户端。

-----

## 2\. 核心代码实现

### 初始化 Redis 客户端

首先，建议封装一个单例的 Redis 连接模块。

```javascript
const redis = require('redis');
const client = redis.createClient({ url: 'redis://localhost:6379' });

client.on('error', (err) => console.error('Redis Client Error', err));
client.connect();

module.exports = client;
```

### 编写通用缓存中间件

这是最关键的部分。我们需要重写 `res.send` 方法，以便在数据发送给客户端的同时，自动将其捕获并存入 Redis。

```javascript
const redisClient = require('./redisConfig');

const cacheMiddleware = (duration) => {
  return async (req, res, next) => {
    // 1. 生成唯一的 Key（根据路径和查询参数）
    const key = `cache:${req.originalUrl || req.url}`;

    try {
      const cachedData = await redisClient.get(key);
      
      if (cachedData) {
        // 2. 如果命中缓存，直接返回
        console.log('Cache Hit!');
        return res.status(200).json(JSON.parse(cachedData));
      }

      // 3. 如果未命中，拦截 res.send
      console.log('Cache Miss!');
      const originalSend = res.send;
      
      res.send = function (body) {
        // 只有在状态码为 200 时才缓存
        if (res.statusCode === 200) {
          redisClient.setEx(key, duration, JSON.stringify(body));
        }
        return originalSend.call(this, body);
      };

      next();
    } catch (err) {
      console.error('Redis Error:', err);
      next(); // Redis 报错时不影响业务，直接跳过缓存
    }
  };
};
```

-----

## 3\. 最佳实践要点

### A. 合理设置过期时间 (TTL)

不要设置永久缓存。根据数据的实时性要求设置过期时间：

  * **静态配置类**：1 小时或更久。
  * **热门排行榜**：5-10 分钟。
  * **用户信息/余额**：建议不要缓存，或仅缓存几秒钟。

### B. 缓存键（Key）的设计

Key 必须具有唯一性。如果接口依赖 `Header`（如语言切换）或 `User ID`（个性化内容），必须将这些因素加入 Key 中：

  * `cache:/api/products?category=electronics`
  * `cache:user:101:/api/profile`

### C. 序列化开销

Redis 存储的是字符串。对于大型 JSON 对象，`JSON.stringify()` 和 `JSON.parse()` 会消耗 CPU。如果数据量巨大，可以考虑使用 **MessagePack** 等更高效的序列化格式。

### D. 处理缓存击穿与雪崩

  * **缓存击穿**：热点 Key 过期瞬间，大量请求涌入。可以配合 `Semaphore`（信号量）或设置“逻辑过期”来缓解。
  * **缓存雪崩**：大量 Key 同时过期。**解决方法**：在设置 `duration` 时增加一个随机的偏移量（Jitter），如 `duration + Math.random() * 60`。

### E. 缓存主动更新

当数据库数据发生 **Update/Delete** 时，必须同步删除 Redis 中对应的 Key，否则客户端会看到过期的数据。

-----

## 4\. 示例用法

在路由中应用中间件：

```javascript
const express = require('express');
const app = express();

// 对此接口开启 60 秒缓存
app.get('/api/news', cacheMiddleware(60), async (req, res) => {
  const news = await db.fetchNews(); // 模拟数据库查询
  res.json(news);
});
```

 
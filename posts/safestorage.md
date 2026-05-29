---
id: FU0aVz
title: SafeStorage
createdAt: "2026-05-29 14:53:03"
updated: "2026-05-29 14:54:48"
tags:
    - utils
    - storage
tag_ids:
    - CRyCqP
    - 4uK6uG
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---
<!-- https://juejin.cn/post/7644861167895789618 -->
## 实现
```js
class SafeStorage {
  #storage;
  #prefix;
  #available;

  constructor({ storage = localStorage, prefix = 'app' } = {}) {
    this.#prefix = prefix;
    this.#storage = storage;
    this.#available = this.#probe();
  }

  #probe() {
    try {
      const k = '__probe__';
      this.#storage.setItem(k, '1');
      this.#storage.removeItem(k);
      return true;
    } catch { return false; }
  }

  #key(key) { return `${this.#prefix}:${key}`; }

  set(key, value, ttlMs = null) {
    if (!this.#available) return false;
    const payload = JSON.stringify({
      v: value,
      exp: ttlMs ? Date.now() + ttlMs : null
    });
    try {
      this.#storage.setItem(this.#key(key), payload);
      return true;
    } catch (e) {
      if (e.name === 'QuotaExceededError') {
        console.warn('[SafeStorage] 超出存储配额，写入失败');
        return false;
      }
      throw e;
    }
  }

  get(key, defaultValue = null) {
    if (!this.#available) return defaultValue;
    try {
      const raw = this.#storage.getItem(this.#key(key));
      if (raw === null) return defaultValue;
      const { v, exp } = JSON.parse(raw);
      if (exp && Date.now() > exp) {
        this.remove(key);
        return defaultValue;
      }
      return v;
    } catch { return defaultValue; }
  }

  remove(key) {
    if (!this.#available) return;
    this.#storage.removeItem(this.#key(key));
  }

  clear(onlyOwn = true) {
    if (!this.#available) return;
    if (!onlyOwn) { this.#storage.clear(); return; }
    const toRemove = [];
    for (let i = 0; i < this.#storage.length; i++) {
      const k = this.#storage.key(i);
      if (k?.startsWith(`${this.#prefix}:`)) toRemove.push(k);
    }
    toRemove.forEach(k => this.#storage.removeItem(k));
  }
}

```

## 示例
```js
// 使用
const store = new SafeStorage({ prefix: 'myapp' });
store.set('user', { name: 'Alice' });                // 永久
store.set('token', 'abc123', 60 * 60 * 1000);        // 1 小时 TTL
store.get('user');                                   // { name: 'Alice' }
store.get('missing', 'fallback');                    // 'fallback'
```
---
id: YSzBHm
title: 如何避免 $router.back() 返回到外部网站
createdAt: "2026-05-20 10:45:46"
updated: "2026-05-20 10:45:46"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

# 如何避免 $router.back() 返回到外部网站

> 当用户从外部网站跳入你的页面时，`router.back()` 会把用户带回到那个外部网站，而不是你的首页。本文介绍 Vue 和 React 项目的解决方案。

## 问题场景

- 用户通过搜索引擎、广告链接、微信分享等从外部网站进入你的页面
- 页面上有返回按钮，调用 `$router.back()` 或 `window.history.back()`
- 点击返回 → 用户被送回外部网站，脱离了你的站点

这不是预期行为，我们希望：**有站内历史就返回上一页，没有就跳首页**。

---

## Vue 项目方案：history.state.position

### vue-router@4 的内部机制

Vue Router 4 在每次导航时，会通过 `history.pushState()` 写入一个 state 对象：

```ts
// vue-router 内部行为（简化）
history.pushState({ position: 0, back: null, current: '/home', ... }, '', '/home')
history.pushState({ position: 1, back: '/home', current: '/list', ... }, '', '/list')
history.pushState({ position: 2, back: '/list', current: '/detail', ... }, '', '/detail')
```

其中 `position` 是从 0 开始递增的索引，表示当前页面在 Vue Router 历史栈中的位置。

### 关键字段说明

| 字段 | 含义 |
|------|------|
| `window.history.length` | 浏览器历史栈总长度（包含外部网站记录），直接打开页面时为 1 |
| `window.history.state.position` | vue-router 写入的当前页面位置索引，0 表示起点 |

### 为什么需要双重判断

单独使用任一条件都不够：

- **只看 `length > 1`**：外部网站跳入时 length 也 ≥ 2，无法区分站内外
- **只看 `position > 0`**：position 是 vue-router 写入的，但需要 length 辅助确认栈中确实有其他记录

组合判断才能精确区分：

| 场景 | length | position | 结果 |
|------|--------|----------|------|
| 直接输入 URL 进入本站 | 1 | 0 | 跳首页 |
| 从本站其他页面跳过来 | 3 | 1 | `back()` ✅ |
| 从外部网站直接跳进来 | 2 | 0 | 跳首页 ✅ |
| 外部网站进入后，站内再跳了几页 | 4 | 2 | `back()` ✅ |

### 实现代码

```vue
<template>
  <KKCIcon name="icon-com_return" @click="handleBack" />
</template>

<script setup lang="ts">
// [AI-GEN] 安全返回：避免返回到外部网站
const handleBack = () => {
  const isBrowserHistoryValid =
    window.history?.length > 2 && window.history?.state?.position > 0

  if (isBrowserHistoryValid) {
    $router.back()
  } else {
    navigateTo('/') // 回首页
  }
}
</script>
```

### 注意事项

- `state.position` 是 vue-router@4 的**实现细节**，不是 Web 标准，其他框架不会写入此字段
- 浏览器原生 `history.state` 只是一个任意对象，内容由 `pushState/replaceState` 的调用者决定
- vue-router@4 写入的完整结构为 `{ position, back, current, forward, replaced }`

---

## React 项目方案

React Router v6 不会往 `history.state` 写入 `position`，无法直接复用 Vue 的方案。以下是三种替代思路：

### 方案一：document.referrer（最简单）

`document.referrer` 是浏览器原生属性，记录上一个页面的完整 URL。

```ts
// [AI-GEN] 安全返回：检查来源是否为本站
const handleBack = () => {
  const referrer = document.referrer
  const isSameOrigin = referrer && new URL(referrer).origin === window.location.origin

  if (isSameOrigin) {
    window.history.back()
  } else {
    navigate('/') // React Router useNavigate()
  }
}
```

局限：部分场景 `referrer` 会丢失（HTTPS → HTTP、`noopener` 链接、浏览器隐私策略）。直接输入 URL 进入时 referrer 为空，走首页 — 这恰好是期望行为。

### 方案二：sessionStorage 标记来源（最可靠）

在全局 Layout 或路由守卫中记录站内来源：

```ts
// [AI-GEN] 全局记录站内来源路径
// 在 Layout 或路由守卫中，每次进入新页面时记录
useEffect(() => {
  sessionStorage.setItem('from_path', window.location.pathname)
}, [location.pathname])

// 返回时使用记录的来源
const handleBack = () => {
  const fromPath = sessionStorage.getItem('from_path')

  if (fromPath) {
    sessionStorage.removeItem('from_path')
    navigate(fromPath)
  } else {
    navigate('/')
  }
}
```

优势：最可靠，完全不依赖浏览器 API 的不确定性。需要全局配合设置标记。

### 方案三：组合判断（推荐）

兼顾简单性和准确性：

```ts
// [AI-GEN] 安全返回：组合判断
const handleBack = () => {
  const referrer = document.referrer
  const isFromSameSite = referrer && new URL(referrer).origin === window.location.origin
  const hasHistory = window.history.length > 1

  if (isFromSameSite && hasHistory) {
    window.history.back()
  } else {
    navigate('/')
  }
}
```

### 方案对比

| 方案 | 可靠性 | 复杂度 | 说明 |
|------|--------|--------|------|
| `document.referrer` | 中 | 低 | 最简单，但 referrer 有丢失场景 |
| `sessionStorage` 标记 | 高 | 中 | 最可靠，需要全局配合 |
| `referrer + length` 组合 | 中高 | 低 | 兼顾简单和准确性 |

---

## 核心思路总结

无论是 Vue 还是 React，核心逻辑都是同一句话：

**判断上一页是否是站内页面，是就 back()，不是就走首页。**

- Vue 有 vue-router@4 的 `state.position` 这个便利字段可以直接判断
- React 没有类似字段，需要用 `document.referrer` 或手动标记来替代

原理相同，实现方式不同。
---
id: urvyZq
title: nuxt response-wrapper
createdAt: "2026-06-05 16:12:57"
updated: "2026-06-05 16:12:57"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

// server/plugins/response-wrapper.ts
export default defineNitroPlugin((_nitroApp) => {
  _nitroApp.hooks.hook('beforeResponse', async (event, { body }) => {
    // 1. 只处理 /api 路径
    if (!event.path.startsWith('/api')) { return }

    // 2. 排除特殊接口（如 JSONP）
    const excludePaths = ['/api/image-msg']
    if (excludePaths.some(p => event.path.startsWith(p))) { return }

    // 3. 已经是包装格式的（有 code 字段），直接透传
    if (body && typeof body === 'object' && 'code' in body) {
      setResponseHeader(event, 'Content-Type', 'application/json')
      return send(event, JSON.stringify(body))
    }

    // 4. 普通业务数据统一包装
    const responseFormat = {
      code: '10000',
      data: body,
      msg: '成功',
      success: true,
      time: Math.floor(Date.now() / 1000).toString(),
    }
    setResponseHeader(event, 'Content-Type', 'application/json')
    return send(event, JSON.stringify(responseFormat))
  })
})

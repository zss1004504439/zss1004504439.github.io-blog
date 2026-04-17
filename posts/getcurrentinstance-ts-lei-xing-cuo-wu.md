---
id: rtcXJB
title: getCurrentInstance ts类型错误
createdAt: "2021-05-31 10:34:00"
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
import {
  ComponentInternalInstance,
  defineComponent,
  getCurrentInstance,
} from '@vue/composition-api';
export default defineComponent({
  setup(){
    // 类型断言
    const { proxy: _this } = getCurrentInstance() as ComponentInternalInstance
  }
})
```
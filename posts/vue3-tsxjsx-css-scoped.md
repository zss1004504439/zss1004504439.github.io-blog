---
id: FZuX4D
title: vue3 tsx/jsx css scoped
createdAt: "2021-05-31 10:41:07"
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
<script lang="tsx">
import { defineComponent, withScopeId, getCurrentInstance } from 'vue'

export default defineComponent({
  setup(props, ctx) {
    const instance = getCurrentInstance()
    const scopeId = instance.type.__scopeId
    const withId = withScopeId(scopeId)

    return withId(() => <div class="ceshi">fdsafas</div>)
  },
})
</script>

```
https://github.com/vuejs/jsx-next/issues/51
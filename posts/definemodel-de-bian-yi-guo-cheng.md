---
id: CvtMXn
title: defineModel 的编译过程
createdAt: "2025-12-22 14:47:00"
updated: "2025-12-22 14:47:00"
tags:
    - vue3
tag_ids:
    - 6sA0AI
categories: []
published: true
hideInList: false
feature: ""
isTop: false
_source_: https://juejin.cn/post/7631050859780636682
---

```
// defineModel 编译前（我们写的代码）
const model = defineModel({ type: Number, required: true })

// 编译后（编译器自动生成的代码）
const props = defineProps({ modelValue: { type: Number, required: true } })
const emit = defineEmits(['update:modelValue'])

// 生成一个 ref 对象，关联 props.modelValue 和 emit
const model = computed({
  get: () => props.modelValue, // 读取父组件传递的 props（数据父→子）
  set: (newVal) => emit('update:modelValue', newVal) // 修改时触发 emit，通知父组件更新
})

```

## 双向绑定
```
<!-- 父组件 -->
<template>
  <div>父组件 count: {{ count }}</div>
  <Child v-model="count" />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Child from './Child.vue'

const count = ref(0)
// 父组件可主动修改数据，子组件仅能通过 emit 通知修改
const resetCount = () => {
  count.value = 0
}
</script>

<!-- 子组件 -->
<script setup lang="ts">
// 显式指定类型，TS 自动校验 props 规则
const model = defineModel<number>({
  required: true,
  validator: (val) => val >= 0 // 子组件可对 props 进行校验，无法修改
})

// 子组件只能通过修改 model.value 触发 emit，无法直接修改父组件 count
const increment = () => {
  model.value++ // 触发 emit('update:modelValue', model.value + 1)
}
</script>

```
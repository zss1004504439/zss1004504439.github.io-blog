---
id: MSxVQU
title: Vue 3 动态路由 Pinia 状态同步踩坑
createdAt: "2025-04-23 18:41:23"
updated: "2025-04-23 18:51:57"
tags:
    - pinia
tag_ids:
    - JHyI1j
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

在 Vue 3 中结合动态路由（`/course/:id`）和 Pinia 设计 Store 时，核心挑战在于**确保 Store 的状态与当前路由参数同步**，尤其是在同一个组件实例被复用（即从 `/course/1` 跳转到 `/course/2`）时。

以下是三种推荐的设计方案：

---

### 1. 组合式设计（推荐）：以 ID 为核心的响应式 Store
这种方式最符合 Vue 3 的心智模型。不再是在组件里手动调用 `fetch`，而是让 Store 监听 ID 的变化。

```javascript
import { defineStore } from 'pinia';
import { ref, watch } from 'vue';

export const useCourseStore = defineStore('course-detail', () => {
  const courseId = ref(null);
  const details = ref(null);
  const loading = ref(false);

  // 设置 ID 并自动触发更新
  const setCourseId = async (id) => {
    if (courseId.value === id) return;
    courseId.value = id;
    await fetchCourseDetail();
  };

  const fetchCourseDetail = async () => {
    loading.value = true;
    try {
      // 模拟 API 请求
      // const res = await api.getCourse(courseId.value);
      // details.value = res.data;
    } finally {
      loading.value = false;
    }
  };

  // 重置状态（离开页面时调用）
  const reset = () => {
    courseId.value = null;
    details.value = null;
  };

  return { courseId, details, loading, setCourseId, reset };
});
```

---

### 2. 路由守卫集成：确保“数据先于页面”
为了避免页面渲染后出现瞬间的空白或旧数据，可以在组件的 `onBeforeRouteUpdate` 或全局守卫中触发 Store 更新。

**在组件中使用：**
```vue
<script setup>
import { useCourseStore } from '@/stores/course';
import { onMounted } from 'vue';
import { useRoute, onBeforeRouteUpdate } from 'vue-router';

const store = useCourseStore();
const route = useRoute();

// 初始进入
onMounted(() => {
  store.setCourseId(route.params.id);
});

// 路由参数变化（如同一个页面切换不同课程）
onBeforeRouteUpdate((to) => {
  store.setCourseId(to.params.id);
});
</script>
```

---

### 3. 设计要点总结

| 关注点 | 建议方案 |
| :--- | :--- |
| **状态清理** | 在 `onUnmounted` 时调用 `store.$reset()` 或自定义 `reset()`。防止用户进入新课程时先看到上一个课程的残余数据。 |
| **数据缓存** | 如果不需要实时性，可以在 `fetchCourseDetail` 中根据 ID 判断，如果缓存中已有数据则跳过请求。 |
| **唯一性** | 如果你的应用支持多开 Tab（如某些后台系统），考虑使用 `Map<id, data>` 结构存储，而不是单一的 `details` 对象。 |

### 进阶：多实例管理（针对复杂场景）
如果你的系统允许同时打开多个课程页面（例如在 Monorepo 或标签页架构中），建议在 Store 内部使用对象存储：

```javascript
const courses = ref({}); // { '101': { name: 'Vue' }, '102': { name: 'Nuxt' } }

const currentCourse = computed(() => courses.value[courseId.value]);
```

**建议技巧：** 由于你正在处理 Vue 项目，可以利用 **Pinia 的 `$onAction`** 订阅功能。如果需要对所有课程详情页进行埋点或统一错误处理，这比在每个组件写 `try...catch` 更优雅。
---
id: nyBbNL
title: 解决 window.addEventListener('storage') 监听不生效问题
createdAt: "2022-02-17 10:11:03"
updated: "2026-04-17 13:48:26"
tags:
    - 通信
    - storage
tag_ids:
    - HOOERd
    - 4uK6uG
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

# 解决 window.addEventListener('storage') 监听不生效问题
很多前端同学在使用 `window.addEventListener('storage')` 监听本地存储变化时，都会遇到**监听不触发、完全没反应**的问题，这篇文章用最简单的话讲清原因和解决方案。

## 一、核心真相（必看）
`storage` 事件**天生就不会在当前窗口触发**！
只有**同源的另一个窗口/标签页**修改了 `localStorage`，当前窗口才会收到监听通知。

简单说：**自己改存储，自己收不到通知；别人改，你才能收到**。

## 二、监听不生效的 4 个常见原因
1. **同一窗口测试**：当前页面改存储，自己的监听不会执行
2. **值没变化**：重复设置相同的值，不会触发事件
3. **用错存储**：`sessionStorage` 不跨标签页，监听无效
4. **跨域**：不同网址/端口，无法触发 storage 事件

## 三、正确使用代码（复制即用）
```javascript
// 监听存储变化（放在需要接收通知的页面）
window.addEventListener('storage', function (e) {
  console.log('存储发生变化！');
  console.log('修改的key：', e.key);
  console.log('旧值：', e.oldValue);
  console.log('新值：', e.newValue);
  console.log('修改来源页面：', e.url);
});
```

## 四、正确测试步骤（一步不落）
1. 打开**标签页A**，运行上面的监听代码
2. 打开**标签页B**（同源页面），执行修改存储：
```javascript
localStorage.setItem('userInfo', '张三');
// localStorage.removeItem('key'); 删除也能触发
```
3. 回到标签页A，就能看到监听生效了

## 五、同一页面想监听？用这个方案
如果必须在**当前页面修改存储、当前页面监听**，用自定义事件替代：
```javascript
// 1. 封装统一修改方法
function setMyStorage(key, value) {
  localStorage.setItem(key, value);
  // 触发自定义事件
  const event = new CustomEvent('myStorage', {
    detail: { key, value }
  });
  window.dispatchEvent(event);
}

// 2. 监听自定义事件（当前页面可用）
window.addEventListener('myStorage', (e) => {
  console.log('当前页面存储修改：', e.detail);
});

// 3. 使用
setMyStorage('name', '测试');
```

## 六、避坑总结
1. ❌ 同一窗口改存储 → 不触发
2. ✅ 另一同源窗口改 → 触发
3. ❌ 值不变重复设置 → 不触发
4. ✅ 自定义事件 → 当前窗口可用
5. ❌ sessionStorage → 不跨标签页

看完这篇，再也不会被 `storage` 监听坑到啦！

---

### 总结
1. 博客极简清晰，只讲核心问题+解决方案，新手也能看懂
2. 代码可直接复制使用，测试步骤明确
3. 区分**原生storage用法**和**同一页面监听方案**，覆盖所有场景
4. 无多余术语，适合做笔记/快速查阅
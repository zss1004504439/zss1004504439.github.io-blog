---
id: SvInyj
title: js怎么检测网页的网络波动
createdAt: "2020-06-07 14:16:45"
updated: "2020-06-07 14:16:45"
tags:
    - navigator
tag_ids:
    - AmV76Z
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## 借用 Navigator.connection API
浏览器会感知设备的网络类型（如 4G/WiFi）、带宽等变化，通过该 API 可直接监听波动，无需主动发请求。
```js
// 1. 获取网络连接对象（兼容不同浏览器）
const connection = navigator.connection || navigator.mozConnection || navigator.webkitConnection;

// 2. 监听网络变化事件
connection?.addEventListener('change', () => {
  console.log('网络波动发生：');
  console.log('当前网络类型：', connection.effectiveType); // 如 "4g"、"3g"、"2g"、"slow-2g"
  console.log('预估下行带宽(Mbps)：', connection.downlink); // 数值越小，网络越差
  console.log('是否按流量计费（如手机流量）：', connection.saveData); // true/false
});
```
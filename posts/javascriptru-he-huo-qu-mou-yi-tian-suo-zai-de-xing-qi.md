---
id: Cg2FyE
title: JavaScript：如何获取某一天所在的星期
createdAt: "2019-09-17 08:28:34"
updated: "2026-04-17 11:19:42"
tags:
    - ECMAScript
    - Date
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

[如何获取某一天所在的星期](https://segmentfault.com/a/1190000020342937)
```js
function getWeekStartAndEnd(timestamp) {
    const oneDayTime = 1000 * 60 * 60 * 24; // 一天里一共的毫秒数
    const today = timestamp ? new Date(timestamp) : new Date();
    const todayDay = today.getDay() || 7; // 若那一天是周末时，则强制赋值为7
    const startDate = new Date(
        today.getTime() - oneDayTime * (todayDay - 1)
    );
    const endDate = new Date(today.getTime() + oneDayTime * (7 - todayDay));

    return { startDate, endDate };
}
```
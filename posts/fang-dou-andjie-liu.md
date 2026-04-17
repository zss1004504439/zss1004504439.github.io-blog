---
id: m1bIM0
title: "\U0001F618防抖&节流"
createdAt: "2019-09-11 14:21:28"
updated: "2026-04-17 11:19:43"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## 防抖

``` html
<body>
  <button id="debounce">点我防抖！</button>
  <script>
    window.onload = function() {
      var myDebounce = document.getElementById("debounce");
      myDebounce.addEventListener("click", debounce(sayDebounce));
    }
    function debounce(fn) {
      let timeout = null;
      return function() {
        clearTimeout(timeout);
        timeout = setTimeout(() => {
          fn.call(this, arguments);
        }, 1000);
      };
    }
    function sayDebounce() {
      console.log("防抖成功！");
    }
  </script>
</body>
```
## 节流
```html
<body>
  <button id="throttle">点我节流！</button>
  <script>
    window.onload = function() {
      var myThrottle = document.getElementById("throttle");
      myThrottle.addEventListener("click", throttle(sayThrottle));
    }
    function throttle(fn) {
      let flag = true;
      return function() {
        if(!flag) {
          return;
        }
        flag = false;
        setTimeout( () => {
          fn.call(this, arguments);
          flag = true;
        }, 1000);
      };
    }
    function sayThrottle() {
      console.log("节流成功！");
    }
  </script>
</body>
```
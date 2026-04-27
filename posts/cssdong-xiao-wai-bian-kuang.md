---
id: nHBIC7
title: css动效外边框
createdAt: "2026-04-27 17:44:56"
updated: "2026-04-27 17:44:56"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---


```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>紫色发光边框</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background:
        radial-gradient(circle at center, rgba(68, 55, 145, 0.18), transparent 34%),
        linear-gradient(180deg, #030712 0%, #05091c 100%);
      font-family: Arial, Helvetica, sans-serif;
    }

    .card-wrap {
      position: relative;
      width: 460px;
      height: 285px;
      border-radius: 28px;
      padding: 2px;
      background: linear-gradient(
        135deg,
        rgba(196, 158, 255, 0.95) 0%,
        rgba(105, 80, 210, 0.75) 18%,
        rgba(64, 55, 145, 0.28) 55%,
        rgba(113, 83, 210, 0.6) 78%,
        rgba(184, 139, 255, 0.9) 100%
      );
      /* box-shadow:
        0 0 4px rgba(210, 180, 255, 0.8),
        0 0 14px rgba(132, 88, 255, 0.65),
        0 0 30px rgba(77, 56, 202, 0.35); */
      overflow: hidden;
    }

    /* 某一段边框高亮扫光 */
    .card-wrap::before {
      content: "";
      position: absolute;
      inset: -45%;
      background: conic-gradient(
        from 228deg,
        transparent 0deg,
        transparent 245deg,
        rgba(148, 92, 255, 0.1) 260deg,
        rgba(217, 193, 255, 0.95) 280deg,
        rgba(255, 255, 255, 1) 292deg,
        rgba(174, 115, 255, 0.9) 308deg,
        transparent 330deg,
        transparent 360deg
      );
      filter: blur(3px);
      animation: borderFlow 5s linear infinite;
      opacity: 0.9;
    }

    .card {
      position: relative;
      width: 100%;
      height: 100%;
      border-radius: 26px;
      overflow: hidden;
      background:
        radial-gradient(circle at 18% 0%, rgba(123, 78, 255, 0.5), transparent 34%),
        radial-gradient(circle at 100% 14%, rgba(52, 63, 145, 0.38), transparent 35%),
        linear-gradient(135deg, #342174 0%, #15174d 43%, #080d28 100%);
      padding: 34px 32px;
      color: #fff;
      z-index: 1;
    }

    @keyframes borderFlow {
      to {
        transform: rotate(360deg);
      }
    }
  </style>
</head>
<body>
  <div class="card-wrap">
    <div class="card">
    </div>
  </div>
</body>
</html>

```
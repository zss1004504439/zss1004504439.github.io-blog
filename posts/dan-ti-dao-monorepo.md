---
id: XHvunz
title: 单体到Monorepo
createdAt: "2026-05-09 11:04:03"
updated: "2026-05-09 11:04:03"
tags:
    - Monorepo
tag_ids:
    - HWq3VY
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## https://juejin.cn/post/7549930693388927027
```
sichuanji-cc/
├── apps/                    # 应用层：隔离但统一
│   ├── web/                # 前台官网 (3000端口)
│   └── admin/              # 管理后台 (3001端口)
├── packages/               # 共享层：核心价值所在
│   ├── shared/             # 工具函数和类型定义
│   ├── ui/                 # 可复用UI组件库
│   ├── auth/               # 认证授权模块
│   ├── database/           # 数据库访问层
│   └── dev-tools/          # 开发工具配置

```
---
id: L6GaJl
title: v-if 与 v-for 优先级
createdAt: "2023-04-27 17:50:44"
updated: "2023-04-27 17:50:44"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---



## **`v-if` 与 `v-for` 优先级变了**

- Vue2： `v-for` 优先级 **高于** `v-if`
- Vue3： `v-if` 优先级 **高于** `v-for` （所以不建议同时用，用 `<template>` 隔开）
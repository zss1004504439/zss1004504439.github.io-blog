---
id: ghRGle
title: Git 小技巧：一个本地仓库关联两个远程仓库
createdAt: "2020-01-17 13:53:10"
updated: "2020-01-17 13:53:10"
tags:
    - Git
tag_ids:
    - chD1ia
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

# Git 小技巧：一个本地仓库关联两个远程仓库
日常开发经常需要把代码同时同步到 GitHub、Gitee 或公司内网 Git，一个本地仓库绑定两个远程仓库，一次提交就能两边同步，非常省事。

### 一、先看现有关联
```bash
git remote -v
```

### 二、添加第二个远程仓库
给第二个仓库起个名字，比如 gitee：
```bash
git remote add gitee 仓库地址
```

### 三、分别推送
```bash
git push origin main
git push gitee main
```

### 四、一键推两个（更省事）
给 origin 追加推送地址，之后 git push 就会同时推两个：
```bash
git remote set-url --add --push origin 第一个地址
git remote set-url --add --push origin 第二个地址
```

### 五、常用命令
- 重命名远程：`git remote rename old new`
- 修改地址：`git remote set-url 名称 新地址`
- 删除关联：`git remote remove 名称`

---
一句话总结：用 `git remote add` 加第二个仓库，想一键同步就用 `set-url --add --push` 追加推送地址，复制即用，不用记复杂配置。

要不要我再给你写一段**一键双推的 Git 别名配置**，直接贴进配置文件就能用？
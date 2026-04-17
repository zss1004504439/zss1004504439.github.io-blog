---
id: ukZtBj
title: MoneyFormat
createdAt: "2020-07-28 09:55:13"
updated: "2026-04-17 11:19:42"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

## 金额格式化
```js
      var money = 1200;
      var MoneyFormat = (money) => {
        const moneyStr = money.toLocaleString("en-IN", {
          style: "currency",
          currency: "CNY",
          currencyDisplay: "symbol",
          minimumFractionDigits: 2,
        })
        console.log("MoneyFormat -> moneyStr", moneyStr)
        return moneyStr.replace('CN','')
      };
      console.log(MoneyFormat(money)); ￥1,200.00
```
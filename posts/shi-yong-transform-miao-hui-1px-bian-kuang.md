---
id: f4CTf4
title: 使用transform描绘1px边框
createdAt: "2019-10-10 17:11:13"
updated: "2026-04-17 11:19:42"
tags:
    - scss
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

分辨率比较低的屏幕下显示1px的边框会显得模糊，通过::before或::after和transform模拟细腻的1px边框
```scss
.onepx-border {
	margin-top: 10px;
	width: 200px;
	height: 80px;
	cursor: pointer;
	line-height: 80px;
	text-align: center;
	font-weight: bold;
	font-size: 50px;
	color: $red;
	&:first-child {
		margin-top: 0;
	}
}
.normal {
	border: 1px solid $red;
}
.thin {
	position: relative;
	&::after {
		position: absolute;
		left: 0;
		top: 0;
		width:200%;
		height:200%;
		border: 1px solid $red;
		content: "";
		transform: scale(.5);
		transform-origin: left top;
	}
}
```
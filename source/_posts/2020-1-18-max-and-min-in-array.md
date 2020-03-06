---
layout: post
title: 数组比最大值和最小值
date: 2020-01-18 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
---

```javascript
Array.prototype.max = function() {
	return Math.max.apply({},this)

}
Array.prototype.min = function() {
	return Math.min.apply({},this)
}

array.min()

array.max()
```

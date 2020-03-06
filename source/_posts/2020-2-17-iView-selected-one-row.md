---
layout: post
title: iView的表格如何单击这行就选中
date: 2020-02-17 12:00:00
categories:
    - JavaScript
    - Vue.js
tags:
    - JavaScript
    - 前端
    - iView
    - Vue.js
---

```HTML
<Table 
    border 
    ref="table" 
    :columns="columns" 
    :data="list" 
    @on-row-click="rowClick" 
    @on-selection-change="SelectGoods"
>
</Table>
```
```javascript
rowClick(data,index){
    this.$refs.table.toggleSelect(index);
}
```

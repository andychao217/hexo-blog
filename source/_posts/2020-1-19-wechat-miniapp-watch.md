---
layout: post
title: 微信小程序，实现 watch 属性，监听数据变化
date: 2020-01-19 12:00:00
categories:
    - JavaScript
    - 小程序
tags:
    - JavaScript
    - 前端
    - 小程序
    - 微信
---

# 微信小程序，实现 watch 属性，监听数据变化

## 目标
在微信小程序实现 watch 属性，监听 data 中的属性，当被监听属性的值改变时，执行我们指定的方法。

## 思路
Vue 的 computed 和 watch 可以很方便的检测数据的变化，从而做出相应的改变，所以，模仿 vue 肯定是一个不错的选择。

与 Vue 一样，我们使用 ES5 的Object.defineProperty()方法，劫持对象的 **getter/setter**，从而实现给对象赋值时(调用 setter)，执行 watch 对象中相对应的函数，达到监听效果。

## 代码
不啰嗦，上代码，真实可用。

```javascript
function observe(obj, key, watchFun, deep, page) {
  let val = obj[key];

  if (val != null && typeof val === "object" && deep) {
    Object.keys(val).forEach((item) => {
      observe(val, item, watchFun, deep, page);
    });
  }

  Object.defineProperty(obj, key, {
    configurable: true,
    enumerable: true,
    set: function(value) {
      watchFun.call(page, value, val);
      val = value;

      if (deep) {
        observe(obj, key, watchFun, deep, page);
      }
    },
    get: function() {
      return val;
    }
  });
}

export function setWatcher(page) {
  let data = page.data;
  let watch = page.watch;

  Object.keys(watch).forEach((item) => {
    let targetData = data;
    let keys = item.split(".");

    for (let i = 0; i < keys.length - 1; i++) {
      targetData = targetData[keys[i]];
    }

    let targetKey = keys[keys.length - 1];

    let watchFun = watch[item].handler || watch[item];

    let deep = watch[item].deep;
    observe(targetData, targetKey, watchFun, deep, page);
  });
}
```

#### 注意事项：
- watch 只能监听已存在的属性，数组的 push()，pop()等方法并不会触发监听函数。

## 使用

```javascript
import * as watch from "./watch.js";

Page({
  data: {
    name: "二狗子"
  },

  onLoad() {
    watch.setWatcher(this);
  },

  watch: {
    name: function(newVal, oldVal) {
      console.log(newVal, oldVal);
    }
  }
});
```

#### 首先在需要的页面引入
- 在 Page 的onLoad钩子设置监听器
- 然后就可以愉快的使用了。

## 总结
watch 会使代码更简洁，逻辑更清晰，在响应式数据处理上很方便。
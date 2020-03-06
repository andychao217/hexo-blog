---
layout: post
title: 前端如何检测系统黑暗模式(Dark Mode)
date: 2020-01-24 12:00:00
categories:
    - JavaScript
    - CSS
tags:
    - JavaScript
    - 前端
    - CSS
    - 黑暗模式
    - Dark Mode
---

## 白天不知夜的黑

iOS 13 发布至今，除了带来无以计数的 bug 以外，还带来了在 Andorid Q 中就先一步实现的暗黑模式（dark mode）。基于 OLED 屏幕的特性，暗黑模式相对来说更为省电，良好的设计也能在黑夜中为用户提供更好的视觉体验。因此，在过去的一段时间里，各大 app 纷纷推出了针对暗黑模式的更新。从此暗黑模式的时代来临了。

那么问题来了，前端工程师如何为 web 应用实现暗黑模式呢？

在没有白天和暗黑模式都没有的黑夜里，我们通常可以通过 JavaScript 判断本地时间或者获取服务器时间来判断是否改为当前 web 应用添加暗黑模式的层叠样式表。这样做直观且粗暴，简单的是无论平台是否是支持暗黑模式的 iOS 13+ 还是不支持暗黑模式的 Ubuntu 12.04，用户都能获得“暗黑模式”的体验；粗暴的是，这个体验也就真的仅仅是体验而已了。且不说是否符合日出日落的规律，如果是 app 内嵌的 html 页面，大多数时候都会获得一个无法和 app 同昼同夜的糟糕体验。

那么为了和 OS 的暗黑模式同时同分同秒同生死，我们还有什么解决方案？幸运的是，媒体查询总是能救开发者于水火。
## prefers-color-scheme

media query 为开发者提供了系统主题检测的属性：prefers-color-scheme，有三个可用的值：no-preference、light 和 dark。

从值上很容易理解这个属性的作用，我们只要考虑如何使用即可：

### CSS
```CSS
/* light mode */
@media (prefers-color-scheme: light) {
  body {
    background: #f0f0f0;
  }
}

/* dark mode */
@media (prefers-color-scheme: dark) {
  body{
    color: #fff;
    background: #3e3e3e;
  }
}
```

### JavaScript
```javascript
if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
  // dark mode
}
```

---
layout: post
title: The typeof typeof
date: 2020-04-03 15:00:00
categories:
    - JavaScript
tags: 
    - JavaScript
    - 前端
    - 基础知识
---

### What does the following code evaluate to?

```js
typeof typeof 0
```

#### Answer

It evaluates to `"string"`.

`typeof 0` evaluates to the string `"number"` and therefore `typeof "number"` evaluates to `"string"`.

#### Good to hear

##### Additional links

* [MDN docs for typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

<!-- tags: (javascript) -->

<!-- expertise: (1) -->

---
layout: post
title: Browser or Node?
date: 2020-03-30 15:00:00
categories:
    - JavaScript
tags: 
    - JavaScript
    - 前端
---

## isBrowser

Determines if the current runtime environment is a browser so that front-end modules can run on the server (Node) without throwing errors.

Use `Array.prototype.includes()` on the typeof values of both window and document (globals usually only available in a browser environment unless they were explicitly defined), which will return true if one of them is `undefined. typeof` allows globals to be checked for existence without throwing a `ReferenceError`. If both of them are not undefined, then the current environment is assumed to be a browser.

```js
const isBrowser = () => ![typeof window, typeof document].includes('undefined');
isBrowser(); // true (browser)
isBrowser(); // false (Node)
```
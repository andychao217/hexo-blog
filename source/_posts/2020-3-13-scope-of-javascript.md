---
layout: post
title: Javascript作用域
date: 2020-03-13 13:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 基础知识
---

> JavaScript 是一门编译语言, 编译就发生在执行前几微秒(甚至更短!)

## 编译步骤: 
1. 分词/词法分析
2. 解析/语法分析
3. 代码生成

## 变量的赋值:

```
graph LR
LHS:在当前作用域声明变量-->RHS:在作用域中查找该变量并赋值
```
在当前作用域无法找到某个变量时, 引擎就会在外层嵌套的作用域中继续查找, 直到找到该变量, 或者抵达最外层的作用域(即全局作用域)为止 (从内到外查找)

```javascript
function foo(a){
    console.log(a + b);
}
var b = 2;
foo(2); //4
```

- RHS查询在所有作用域中都找不到变量时, 就会抛出ReferenceError

- LHS在*非严格模式*下查找不到变量时,会在全局作用域自动创建该名称的变量

- LHS在*严格模式*下查找不到变量时, 也会抛出ReferenceError

- 作用域查找会在找到*第一个*匹配的标识符时停止, 多层嵌套的作用域中, 同名标识符会产生"*遮蔽效应*", 内层会遮蔽外层

- 全局变量被遮蔽时, 可用*window.a*这种方法访问

- 非全局变量被遮蔽后, 则无论如何都没法访问到

- eval(...)可以将字符串转换为代码, 很危险!! 禁止使用!!!

- with为重复引用同一对象中多个属性的快捷方式, 也禁止使用!!!

## 函数中的作用域

### 函数内容私有化

```javascript
function foo(a) {
    function bar(a) {
        return a - 1;
    }
    var b;
    b = a + bar(a*2);
    console.log(b*3);
}
foo(2); //15

//b和bar都无法从外部访问
```
### 规避冲突

```javascript
var MyReallyCoolLib = {
    awesome: "stuff",
    foo: function() {
        //...
    },
    bar: function() {
        //...
    },
    ...
}
```
> 如果function是声明中的第一个词, 那么就是一个函数声明, 否则就是函数表达式

```javascript
(function foo(){
    //函数表达式
})();
```
> 始终给函数表达式命名是一个最佳实践!!

```javascript
setTimeout(function timeoutHandler(){ //<--有名字
    //...
}, 1000);
```
> 立即执行表达式IIFE (function foo(){...})(), 第一个()将函数变为表达式, 第二个()立即执行这个函数

### let
- 只要声明有效, 在声明的人员位置都可以使用{...}来为let创建用于绑定的块
- 使用let进行的声明不会在块作用域中提升, 声明的代码运行前, 声明并不存在

```javascript
{
    console.log(bar); //ReferenceError!
    let bar = 2;
}
```
- 回收垃圾

```javascript
function process(data) {
    //...
}
//这个块中的内容完事可以自动销毁!
{
    let someReallyBigData = {...};
    process(someReallyBigData);
}
...
```
### const
- 不能修改的块级常量

```javascript
var foo = true;
if (foo) {
    var a = 2;
    const b = 3;
    
    a = 3; //正常!
    b = 4; //错误!
}
console.log(a); //3
console.log(b); //ReferenceError!
```
## 提升:
- 包括变量和函数在内的所有声明都会在任何代码执行之前首先处理
- *先有蛋(声明)后有鸡(赋值)*
- 只有声明本身会提升, 而赋值或其他运行逻辑会留在原地
- 函数声明会提升, 但是函数表达式不会被提升
- 函数首先被提升, 然后才是变量
- 避免重复声明
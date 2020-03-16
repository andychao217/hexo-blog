---
layout: post
title: Closure in JavaScript
date: 2020-03-14 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 基础知识
---

## 闭包

> 当函数可以记住并访问所在的词法作用域, 即使函数是在当前词法作用域之外执行, 这时就产生了*闭包*

> JavaScript中闭包无处不在, 只需要能够识别并拥抱它.

```javascript
function foo() {
    var a = 2;
     
    function bar() {
        console.log(a);
    }
    
    return bar; //将bar引用的函数对象本身当作返回值
}

var baz = foo();

baz(); //2 --这就是闭包
```

拜bar()所声明的位置所赐, 它拥有涵盖foo()内部作用域的闭包, 使得该作用域能够一直存活, 一共bar()在之后任何时间进行引用. 

> 闭包使得函数可以继续访问定义时的词法作用域

> 只要使用了*回调函数*, 实际上就是使用闭包!

- 错误循环

```javascript
for (var i=1; i<=5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, 1000);
}
//以每秒一次的频率输出1~5
//循环终止时,i==6;
//延迟函数的回调会在循环全部结束时才执行, 因此每次都会输出一个6
```

- 正确示范

```javascript
//方法一:
for (var i=1; i<=5; i++) {
    (function(j) {
        setTimeout(function timer() {
            console.log(j);
        }, 1000);
    })(i);
}
//以每秒一次的频率分别输出1~5
//IIFE为每个迭代都生成新的作用域, 使得延迟函数的回调可以将新的作用域封闭在每个迭代内部, 访问正确的变量

//方法二:
for (let i=1; i<=5; i++) {
    setTimeout(function timer() {
        console.log(i);
    }, 1000);
}
//块作用域和闭包,天下无敌!
```
## 模块

```javascript
function CoolModule() {
    var somthing = "cool";
    var another = [1, 2, 3];
    
    function doSomething() {
        console.log(something);
    }
    
    function doAnother() {
        console.llog(another.join("!"));
    }
    
    return {
        doSomething: doSomething,
        doAnother: doAnother
    }
}

var foo = CoolModule();

foo.doSomething(); //cool
foo.doAnother(); //1!2!3
```
### 模块模式必备条件: 
1. 必须有外部的封闭函数, 该函数必须至少调用一次(每次调用都会创建一个新的模块实例)
2. 封闭函数必须返回至少一个内部函数, 这样内部函数才能在私有作用域中形成闭包, 并且可以访问或者修改私有的状态

### 现代模块

```javascript
var MyModules = (function Manager() {
    var modules = {};
    
    function define(name, deps, impl) {
        for (var i=0; i<deps.length; i++) {
            deps[i] = modules[deps[i]];
        }
        modules[name] = impl.apply(impl, deps);
    }
    
    function get(name) {
        return modules[name];
    }
    
    return {
        define: define,
        get: get
    }
})();

MyModules.define("bar", [], function(){
    function hello(who) {
        return "Let me introduce: " + who;
    }
    
    return {
        hello: hello
    }
});

MyModules.define("foo", ["bar"], function(){
    var hungry = "hippo";
    
    function awesome() {
        console.log(bar.hello(hungry).toUpperCase());
    }
    
    return {
        awesome: awesome
    }
});

var bar = MyModules.get("bar");
var foo = MyModules.get("foo");

console.log(bar.hello("hippo")); //Let me introduce: hippo
foo.awesome(); //LET ME INTRODUCE: HIPPO
```

### 未来模块

- bar.js


```javascript
function hello(who) {
    return "Let me introduce: " + who;
}

export hello;
```

- foo.js

```javascript
import hello from "bar";

var hungry = "hippo";
    
function awesome() {
    console.log(hello(hungry).toUpperCase());
}

export awesome;
```

- baz.js

```javascript
module foo from "foo";
module bar from "bar";

console.log(bar.hello("rhino")); //Let me introduce: rhino
foo.awesome(); //LET ME INTRODUCE: HIPPO
```

1. import可以将一个模块中的一个或多个API导入到当前作用域中, 并分别绑定在一个变量上, 如hello
2. module会将整个模块的API导入并绑定在一个变量上, 如foo和bar
3. export会将当前模块的一个标识符(变量, 函数)导出为公共函数
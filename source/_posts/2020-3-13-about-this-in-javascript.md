---
layout: post
title: About this in Javascript
date: 2020-03-13 12:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 基础知识
---

> #### this提供了一种优雅的方式来隐式"传递"对象引用, 有助于将API设计的更加简洁且易于复用

#### 1. What is *"this"* ?
- 当函数被调用时,会创建一个活动记录(执行上下文), 这个记录包含函数在哪里被调用(调用栈),调用方式, 传入的参数等信息. this就是这个记录的一个属性.
- this实际上就是在函数*调用时发生的绑定*, 它指向什么完全取决于函数*在哪里被调用*

#### 2. 绑定规则
1. 默认绑定

- foo()直接调用时, 只能使用默认绑定, this指向全局对象;


```javascript
function foo(){
    console.log(this.a);
}
var a=2;
foo(); //2
```
- foo()本身使用strict mode时, 不能使用默认绑定, this为undefined;


```javascript
function foo(){
    "use strict";
    console.log(this.a);
}
var a=2;
foo(); //TypeError: this is undefined
```
- 在strict mode下调用foo(), 不影响默认绑定


```javascript
function foo(){
    "use strict";
    console.log(this.a);
}
var a=2;
(function(){
    "use strict";
    foo(); //2
})();
```

2. 隐式绑定

- 函数引用有上下文对象时, 隐式绑定规则会将this绑定到上下文对象
- 对象属性引用链只在最后一层调用位置中起作用

```javascript
function foo(){
    console.log(this.a);
}
var obj2 = {
    a: 42,
    foo: foo
};
var obj1 = {
    a: 2,
    obj2: obj2
};
obj1.obj2.foo(); //42
```
- 隐式丢失


```javascript
function foo() {
    console.log(this.a);
}
var obj = {
    a:2,
    foo: foo
};
var bar = obj.foo; //bar引用的是foo函数本身
var a = "oops, global"; //全局属性
bar(); //"oops, global"

//bar()此时就是一个不带任何修饰的函数调用,因此使用默认绑定

setTimeout(obj.foo, 100); //"oops, global"
//setTimeout相当于下述代码, 会把回调函数的this强制绑定到触发事件的DOM元素上
/*
    function setTimeout(fn, delay){
        //等待delay毫秒
        fn(); //调用函数
    } 
*/
```

3. 显示绑定

- 使用call(...)和apply(...)强制绑定


```javascript
function foo() {
    console.log(this.a);
}
var obj = {
    a:2
};
var bar = function() {
    foo.call(obj); //this被硬绑定至obj
};
bar(); //2
setTimeout(bar, 100); //2
bar.call(window); //2 硬绑定的bar不能再修改它的this
```

- 可复用的辅助绑定函数


```javascript
function bind(fn, obj) {
    return function() {
        return fn.apply(obj, arguments);
    };
}

//调用方法
function foo(sth) {
    console.log(this.a, sth);
    return this.a + sth;
}
var obj = {
    a:2
};
var bar = bind(foo, obj);
var b = bar(3); //2, 3
console.log(b); //5
```

- API调用的上下文


```javascript
function foo(el) {
    console.log(el, this.id);
}
var obj = {
    id: "awesome"
};
[1,2,3].forEach(foo, obj); //1 awesome 2 awesome 3 awesome
```

4. new绑定


```javascript
function foo(a) {
    this.a = a;
}
var bar = new foo(2);
console.log(bar.a); //2
```

#### 3. 绑定优先级

1. 是否在new中调用?　        是的话, this绑定至新创建的对象
2. 是否通过call, apply调用?  是的话, this绑定至指定的对象
3. 是否通过上下文隐式绑定?   是的话, this绑定至新创建的对象
4. 上述都不是的话, 严格模式下, this绑定至undefined, 否则绑定至全局对象

#### 4. 绑定例外
- 被忽略的this

如果把null或undefined作为this的绑定对象传入call, apply, bind, 在调用的时候会被忽略
```javascript
function foo(){
    console.log(this.a);
}
var a=2;
foo.call(null); //2
```

安全的做法


```javascript
function foo(a,b) {
    console.log("a:" + a + ", b:" + b);
}
//DMZ空对象
var ф = Object.create(null);
foo.apply(ф, [2, 3]); //a:2, b:3
var bar = foo.bind(ф, 2);
bar(3); //a:2, b:3
```
使用变量名ф不仅让函数更安全, 而且能提高代码可读性, 因为ф表示"我希望this是空", 这比null含义更清晰

- 间接引用

```javascript
function foo() {
    console.log(this.a);
}
var a = 2;
var o = { a:3, foo:foo };
var p = { a:4 };
o.foo(); //3
(p.foo = o.foo)(); //2

//p.foo = o.foo的返回值是目标函数的引用,因此调用位置是foo()而不是p.foo()或o.foo(), 因此为默认绑定
```

- 软绑定

如果给默认绑定指定一个全局对象和undefined以外的值, 可以实现和硬绑定相同的效果, 同时保留隐式绑定或者显示绑定修改this的能力
```javascript
if (!Function.prototype.softBind) {
    Function.prototype.softBind = function(obj) {
        var fn = this;
        //捕获所有curried参数
        var curried = [].slice.call(arguments, 1);
        var bound = function() {
            return fn.apply(
                (!this || this === (window || global)) ?
                    obj: this,
                curried.concat.apply(curried, arguments)
            );
        };
        bound.prototype = Object.create(fn.prototype);
        return bound;
    }
}
```

软绑定使用
```javascript
function foo() {
    console.log("name: " + this.name);
}
var obj = {name: "obj"},
    obj2 = {name: "obj2"},
    obj3 = {name: "obj3"};
var fooOBJ = foo.softBind(obj);

fooOBJ(); //name: obj

obj2.foo = foo.softBind(obj);
obj2.foo(); //name: obj2

fooOBJ.call(obj3); //name: obj3

setTimeout(obj2.foo, 10); //name: obj <-- 应用软绑定
```

#### 5. 箭头函数

++箭头函数不使用上述this的四种标准规则, 而是根据外层(函数或全局)作用域来决定this(无论this绑定到什么)++

```javascript
function foo() {
    //返回箭头函数
    return (a) => {
        //this继承自foo()
        console.log(this.a);
    };
}
var obj1 = {a:2};
var obj2 = {a:3};
var bar = foo.cal(obj1);
bar.call(obj2); //2, 不是3!

//foo()的this已经绑定至obj1, bar的this也绑定至obj1, 箭头函数的绑定无法修改
```

箭头函数常用于回调函数中, 可以像bind(...)一样确保this绑定至指定对象上

```javascript
function foo() {
    setTimeout(()=>{
        //这里的this继承自foo()
        console.log(this.a);
    }, 100);
}
var obj = {a:2};
foo.call(obj); //2
```
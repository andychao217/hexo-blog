---
layout: post
title: 混合对象-类
date: 2020-03-13 17:00:00
categories:
    - JavaScript
tags:
    - JavaScript
    - 前端
    - 基础知识
---

## 类
- 类是一种设计模式, JavaScript中可以用一些方法实现类的功能
- 类意味着复制
- 传统的类被实例化时, 它的行为会被复制到实例中.
- 类被继承时, 行为也会被复制到子类中
- 多态看起来似乎是从子类引用父类, 但本质上引用的其实是复制的结果
- JavaScript的对象机制不会自动执行复制行为, 即JavaScript中只有对象, 不存在可以被实例化的"类", 一个对象不会被复制到其他对象, 它们会被关联起来
- 混入(mixin): 模拟类的复制行为, 但会让代码难以读懂且难以维护

### 显式混入
- 伪多态会导致代码更复杂, 难以阅读且难以维护, 应尽量避免使用显式伪多态

```javascript
function mixin(sourceObj, targetObj) {
    for (var key in sourceObj) {
        //不存在则复制
        if (!(key in targetObj)) {
            targetObj[key] = sourceObj[key];
        }
    }
    return targetObj;
}

var Vehicle = {
    engines: 1,
    ignition: function() {
        console.log("Start engine.");
    },
    drive: function() {
        this.ignition();
        console.log("Go go go!");
    }
};

var Car = mixin(Vehicle, {
    wheels: 4,
    drive: function() {
        Vehicle.drive.call(this); //显式多态, call(this)确保this绑定至Car而不是Vehicle
        console.log("Rolling on all" + this.wheels + " wheels!");
    }
});
```
- 寄生继承

```javascript
//传统的JavaScript类Vehicle
function Vehicle() {
    this.engines = 1;    
}
Vehicle.prototype.ignition = function() {
    console.log("Start engine");
};
Vehicle.prototype.drive = function() {
    this.ignition();
    console.log("Go go go!");
};

//寄生类
function Car() {
    var car = new Vehicle();
    car.wheels = 4;
    var vehDrive = car.drive();
    car.drive = function() {
        vehDrive.call(this);
        console.log("Rolling on all" + this.wheels + " wheels!");
    }
    return car;
}

var myCar = new Car();
myCar.drive();
//"Start engine"
//"Go go go!"
//"Rolling on all 4 wheels!"
```

### 隐式混入

```javascript
var Something = {
    cool: function() {
        this.greeting = "Hello";
        this.count = this.count? this.count + 1 : 1;
    }
}

Something.cool();
Something.greeting; //"Hello"
Something.count;    //1

var Another = {
    cool: function() {
        Something.cool.call(this);
    }
};

Another.cool();
Another.greeting; //"Hello"
Another.count;    //1

//利用了this的重绑定功能, 应尽量避免使用这样的结构, 以保持代码的整洁和可维护性
```
- 总之, 在JavaScript中模拟类是得不偿失的
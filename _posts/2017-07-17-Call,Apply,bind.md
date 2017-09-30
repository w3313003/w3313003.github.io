---
layout: post
title: "call,Apply,bind"
date: 2017-07-17
categories: [JavaScript]
datetime: 2017-07-17
tags: [JavaScript]

---

* content
{:toc}

call,Apply,bind的含义与使用
<!-- more -->

``JavaScript的this总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，那么如何控制住这个善变的小家伙呢。``

## 1. call&apply
    
- Function.prototype.call和Function.prototype.apply都是非常常用的方法。它们的作用一模样，区别仅在于传入参数形式的不同。

 apply 接受两个参数，第一个参数指定了函数体内 this 对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply 方法把这个集合中的元素作为参数传递给被调用的函数： 
```js
var func = function( a, b, c ){ 
alert ( [ a, b, c ] ); // 输出 [ 1, 2, 3 ] 
}; 
func.apply( null, [ 1, 2, 3 ] ); 
```
在这段代码中，参数 1、2、3 被放在数组中一起传入 func 函数，它们分别对应 func 参数列
中的 a,b,c

 call 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是代表函数体内的 this 指向，从第二个参数开始往后，每个参数被依次传入函数：
```js
var func = function( a, b, c ){ 
alert ( [ a, b, c ] ); // 输出 [ 1, 2, 3 ] 
}; 
func.call( null, 1, 2, 3 ); 
```
当使用 call 或者 apply 的时候，如果我们传入的第一个参数为 null，函数体内的 this 会指
向默认的宿主对象，在浏览器中则是 window; 但如果是在严格模式下，函数体内的 this 还是为 null.


## 2. bind
    
bind方法接收只一个参数，用来修改函数中的this。此外与call&bind的区别在于修改完this之后需要手动调用函数，函数才可以执行。

```js
//在不支持bind方法的环境中用如下函数部署bind
Function.prototype.bind = function(context){
    var _this = this;
    return function() {
        return _this.apply(context,arguments)
    }
};
```

## 3. 总结
- call 是包装在 apply 上面的一颗语法糖，如果我们明确地知道函数接受多少个参数，
而且想
一目了然地表达形参和实参的对应关系，那么也可以用 call 来传送参数。

- 作用
    他们的作用都是改变函数内部的this。
    这三个函数都是函数对象的方法，也就是说只有函数才可以直接调用这些方法。


- 区别
    - 参数： 三个函数的第一个参数都是需要绑定的 this。
        - call： 可以有n个参数，从第二个参数开始的所有参数都是原函数的参数。
        - apply：只有两个参数，并且第二个参数必须为数组，数组中的所有元素一一对应原函数的参数。
        - bind： 只有一个参数，即要绑定的this。
    - 调用： 
        - call,apply:调用后立即执行原函数。
        - bind:调用后返回已经绑定好this的函数


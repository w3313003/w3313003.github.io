---
layout: post
title: "JavaScript数据类型判断"
date: 2017-09-30
categories: [JavaScript]
datetime: 2017-09-30
tags: [JavaScript]

---

* content
{:toc}

JavaScript中有5种基本数据类型：`undefined`,`String`,`Boolean`,`Number`,`Null`,以及一种复杂数据类型`Object`。日常工作中经常会有判断数据类型的需求，这里简单介绍下我平时判断数据类型的几种方式。
<!-- more -->



>  JavaScript中有5种基本数据类型：`undefined`,`String`,`Boolean`,`Number`,`Null`,以及一种复杂数据类型`Object`。日常工作中经常会有判断数据类型的需求，这里简单介绍下我平时判断数据类型的几种方式。

## 1. typeof操作符

**语法**
```js
    typeof variable
```
    

对一个值使用typeof会返回如下字符串
- 'undefined' 这个值未定义
- 'string' 这个值是字符串
- 'object' 这个值是对象或者null
- 'function' 这个值是函数
- 'number' 这个值是数值
- 'boolean' 这个值是布尔值

example：
```js
var a;
console.log(typeof a) // 'undefined'
var b = 123;
console.log(typeof b) // 'number'
```
> typeof是一个操作符,并不是作为全局对象的一个方法存在的，所以尽管可以像`typeof(123)`这样调用，但圆括号并不是必须的。

## 2. instanceof操作符

> 当我们检测的数据类型是基本类型的时候，typeof能很好的满足我们的需求，但在检测引用类型的时候就显得有些力不从心了，通常我们并不是想知道某个值是对象，而是想知道它是什么类型的对象。因此，JavaScript提供了instanceof操作符。


**语法**
```js
    result = variable instanceof constructor
```
如果这个值是给定引用类型的实例，那么instanceof操作符就会返回true，否则会返回false。

example:
```js
var obj = {};
console.log(obj instanceof Object); //true
console.log(obj instanceof RegExp); //false
```
*当使用**instanceof**检测基本类型时，会始终返回**false***

## 3. Object.prototype.toString.call(data)
> instanceof操作符确实解决了类型判断的问题，但还是有一些不足之处。由于instanceof是根据数值原型链来识别数据类型的，但是JS中所有引用类型都是Object的实例，因此在检测一个引用类型的值与Object时，会始终返回true。此外，instanceof只能返回true/false，并不能直接返回数据的类型。因此，这里给大家介绍一种更直观的检测方式。

调用`Object.prototypeto.String.call(data)`会返回一个字符串,如
```js
Object.prototype.toString.call(123) // "[object Number]"
Object.prototype.toString.call('123') // "[object String]"
Object.prototype.toString.call([123]) // "[object Array]"
Object.prototype.toString.call(/123/) // "[object RegExp]"
```
是不是很直观呢，为了方便使用，还可以做一些简单的处理，进一步的封装为一个函数，来供我们在开发中使用。
```js
    function checkType(data) {
        return {}.toString.call(data).match(/[A-Z]\w+/)[0]
    };
    console.log(checkType(123))   // 'Number'
    console.log(checkType([123])) // 'Array'
    console.log(checkType(/123/)) // 'RegExp'
    console.log(checkType(null))  // 'Null'
```

## 4.小结
关于我在js中判断数据类型的方式，就到这里了。如果有什么其它的建议，欢迎指点，让大家互相交流，互相学习，一起进步！最后，祝大家节日快乐！
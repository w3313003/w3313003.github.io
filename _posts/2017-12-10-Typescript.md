---
layout: post
title: "TypeScript-声明，解构，函数"
date: 2017-12-10
categories: [TypeScript]
datetime: 2017-12-10
tags: [TypeScript]

---

* content
{:toc}
函数是JavaScript应用程序的基础。 它帮助你实现抽象层，模拟类，信息隐藏和模块。 在TypeScript里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义 行为的地方。 TypeScript为JavaScript函数添加了额外的功能，让我们可以更容易地使用。
<!-- more -->

## 声明和结构
和es6中类似，TypeScript 中，支持var、let 和 const 这样的声明方式。
> let

let 与 var 声明变量的写法类似，示例代码如下：
```js
let hello = "Hello Angular";
```
不同于var， let 声明的变量只在块级作用域内有效：
```js
function f(input: boolean) {
    let a = 100;
    if(input) {
        let b = a + 1; // 运行正确
        return b;
    }
    return b; // 错误， b没有被定义
}

```
这里定义了两个变量 a 和 b ，a 的作用域是在f()函数体内，而b是在if这个语句块中。块级作用域还有几点需要注意的：

- 不能在它的声明之前被读取或赋值
- 在同样的作用域中，let 不允许变量被重复声明

还需要注意 let 声明在下面两种函数入参的对比：
```js
function funA(x) {
    let x = 100;    // 报错，x已经在函数入参声明
}

// 增加了判断条件组成的新的块级作用域
function funB(condition, x) {
    if(condition) {
        let x = 100;     // 运行正常
        return x;
    }
    return x;    
}

funB(false, 0);    // 返回 0
funB(true, 0);    // 返回 100

```
> const

const 声明与 let 声明相似，它与 let 拥有相同的作用域规则，但 const 声明的是常量，常量不能被重新赋值。但是如果定义的常量是对象，对象里的属性值是可以被重新赋值的。示例代码如下：
```js
const CAT_AGE = 9;

const cat = {
    name: 'kitty',
    age: CAT_AGE
}
// 错误
CAT_AGE = 10;
// 错误
cat = {
    name: 'Danie',
    age: CAT_AGE
}

cat.name = "Jason";    // 正确

```

## 解构

解构是ES6中一个重要特性。解构就是将声明的一组变量与相同结构的数组或者对象的元素数值一一对应，并将变量相对应元素进行赋值。

在 TypeScript 中支持数组解构和对象解构。

#### 数组解构

数据结构是最简单的解构类型，示例代码如下：
```js
let input = [1, 2];
let [first, second] = input;
console.log(first); // 相当于 input[0]: 1;
console.log(second); // 相当于 input[1]: 2

// 也可作用于已声明的变量：
[first, second] = [second, first];    // 变量变换

// 或作用于函数参数
function f([first, second] = [number, number]) {
    console.log(first + second);
}
f([1, 2]);    // 输出 3

```
有时，我们会看到如 let[first, ...rest] 使用 “...” 这样的语句，“...” 三个连续小数点表示的是展开操作符，用于创建可变长的参数列表，使用起来非常方便，示例代码如下：

```js
let [first, ...rest] = [1, 2, 3, 4];
console.log(first);    // 输出 1
console.log(rest);    // 输出 [2, 3, 4];

```

#### 对象解构

对象解构让我们的代码更加简洁，可读性强，示例代码如下：
```js
    let userInfo = {
        userName: 'react',
        passWord: 123
    }
    const { userName, passWord } = userInfo;
    console.log(userName, passWord) //'react' 123
```

## 函数

> 函数定义

在TypeScript中支持函数声明和函数表达式的写法，示例代码如下：
```ts
// 函数声明写法
function maxA(x: number, y: number): number {
    return x > y? x : y;
}

// 函数表达式写法
let maxB = function(x: number, y: number): number {
    return x > y? x : y;
}
// 书写完整函数类型
let maxC: (x: number, y: number) => number = 
function(x: number, y: number): number {
     return x > y? x : y;
}
```
函数类型包含两部分：参数类型和返回值类型。 当写出完整函数类型的时候，这两部分都是需要的。 我们以参数列表的形式写出参数类型，为每个参数指定一个名字和类型。 这个名字只是为了增加可读性。 只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。

第二部分是返回值类型。 对于返回值，我们在函数和返回值类型之前使用( =>)符号，使之清晰明了。 如之前提到的，返回值类型是函数类型的必要部分，如果函数没有返回任何值，你也必须指定返回值类型为 void而不能留空。

> 可选参数

在 TypeScript 中，被调函数的每个参数都是 必传 的，也就是说传递给函数的参数个数必须和函数定义时的参数一致，示例代码如下：
```ts
function max(x: number, y: number): number {
    return x > y? x: y;
}

let result1 = max(2);    // 报错
let result2 = max(2, 4);    // 正常

```
但是，在现实开发中，我们需要根据实际情况来决定是否传入某个参数的情况，TypeScript 也提供了可选参数语法，即在参数名旁边加上 ？ 来使其变成可变参数，需要注意的是，可选参数必须位于必选参数的 后面 ，示例代码如下：
```ts
function max(x: number, y?: number): number {
    if(y) {
        return x > y? x: y;
    } else {
        return x;
    }
}
let result1 = max(2);    // 正常
let result2 = max(2, 4);    // 正常
let result3 = max(2, 4, 7);    // 报错

```
>默认参数

TypeScript 还支持初始化默认参数，如果函数的某个参数设置了默认值，当该函数被调用时，如果没有给这个参数传值或者传的值为 undefined 时，这个参数的值就是设置的默认值，示例代码如下：

```js
function max(x: number, y = 4): number {
    return x > y? x: y;
}

let result1 = max(2);    // 正常
let result2 = max(2, 4);    // 正常
let result3 = max(2, undefined);    // 正常

```

带默认值的参数不必放在必选参数后面，但如果默认值参数放到了必选参数前面，用户必选显式地传入 undefined, 示例代码如下：

```js
function sum(x:number, ...restOfNumber:number[]): number {
    // 是不是觉得...特别熟悉，就是上面说的展开操作符
    let result = x;
    for(let i = 0; i < restOfNumber.length; i++) {
        result += restOfNumber[i];    
    }
    return result;
}

let result = sum(1, 2, 3, 4, 5);
console.log(result);    // 输出 15

```

>函数重载

函数重载是通过为同一个函数提供多个函数类型定义来达到实现多种功能的目的。TypeScript 可以支持函数的重载，示例代码如下：
```js
function css(config: {});
function css(config: string, value: string);

```
在上面的例子种，这个函数有2种重载方法，编译器会根据参数类型来判断该调用哪个函数。TypeScript 的函数重载通过查找重载列表来实现匹配，因此，在实现重载方法时，建议将最精确的定义放在最前面。






---

layout: post

title: 详解函数与变量的声明与提升

date: 2017-07-16

categories: [JavaScript]

tags: [JavaScript]

---

* content
{:toc}

详解函数声明月变量声明及其提升原理。
<!-- more -->
## 函数声明

定义函数有两种方式: 一种是函数声明，另一种是函数表达式。
函数声明语法如下:

```js
function functionName(arg0, arg1, arg2) {
	// 函数体
}

```



### 函数声明提升

所谓的函数声明提升，是指在一个作用域中，如果声明了一个函数，那么在代码执行的时候，会将这个声明提升到整个作用域的最前面来执行。看下面的例子。

```js

printString('hello world')

function printString(string) {
	console.log(string)
}

```



将代码整体复制到控制台，会打印出``hello world``。

再看函数内部作用域:

```js
function outer() {
	inner('hello world')
    function inner(string) {
    	console.log(string)
    }
}
outer()  // hello world
```

这样也得到了正确的输出。

为什么在函数声明之前就可以调用呢？就是因为函数声明提升。
所以，无论函数声明被写在所处作用域的哪里，都会被提升到这段代码的最前面。

### 重复声明函数

如果在已经声明了一个函数的情况下，声明另一个同名函数，将会覆盖前面一个。

```js
function b() {console.log(1)}
function b() {console.log(2)}
b()  // 2

```

上述代码中，真正执行的是后面声明的函数。



### 函数声明的实质

函数声明实质上是定义了一个**变量**，该变量是``Function``的实例, 它指向了一个函数。

```js
function newF() {
	console.log('hello')
}

console.log(typeof newF) //  "function"
console.log(newF instanceof Function) // true
```

这样的话，就不难理解为什么声明重名函数，会覆盖前面一个了，这相当于重新给这个变量赋了一个值。

## 变量定义

定义一个变量的语法如下:

```js
var a
var b
```

实际上是用``var``声明了一个新的变量,变量被声明时默认值为``undefined``。

### 变量声明并初始化的实质

这里说的***变量声明并初始化***是指下面这种方式:

```js
var a = 1;
```

实际上这行代码是分两步执行的:

1. 先声明变量

```js
var a = undefined;
```

2. 给变量赋值

```js
a = 1;
```

这也是为什么可以用一个变量给另一个变量做初始化的原因。看下面的代码:

```js
var b = 3;
var a = b
console.log(a) // 3
```

实际上分了四步:

1. 声明b: ``var b = undefined;``
2. 生命a: ``var a = undefined;``
3. 给b赋值: ``b = 3;``
4. 给a赋值: ``a = b;``

### 变量声明提升

变量的声明也可以提升，也就是说JS在执行时，会把变量声明的代码提升到这个作用域代码段的前面。**注意:**是前面，而不是最前面，因为变量声明提升的优先级别要低于函数声明提升。

看下面代码:

```js
console.log(foo)
var foo = 3
```

执行结果是``undefined``。实际执行顺序为:

```js
var foo = undefined
console.log(foo)
foo = 3
```

### 重复声明变量

如果声明一个已经声明过的变量，JS会忽略第二次声明。但是如果在声明时给变量赋值，那么会覆盖之前的值。

```js
var foo1 = 3;
var foo1
console.log(foo1) // 3
```

拆开来看，是如下代码:

```js
var foo1 = undefined;
foo1 = 3;
var foo1 = undefined; // 这行代码被忽略。不会执行
console.log(foo1) // 3
```



### 声明一个与函数声明的名字相同的变量

如果声明了一个变量，这个变量和某个函数声明具有同样的名字，那么也会忽略这个变量声明。

```js
function foo3() {}
var foo3
console.log(foo3)  // function foo3() {}
```

但是如果声明变量时进行了初始化就不一样了，相当于给变量重新赋值:

```js
function foo3() {}
var foo3 = 3
console.log(foo3)  // 3
```



## 函数表达式

另一种定义函数的方式是函数表达式,语法如下:

```js
var func = function(){
	console.log('hello')
}
```

实际上，这就是一个变量声明并初始化的代码。分解后是这样:

```js
var func = undefined;
func = function(){
	console.log('hello')
}

```

从以下代码可以看出:

```js
console.log(func) // undefined
var func = function(){}
console.log(func)  // function (){}
```

这也印证了*变量声明提升*与*变量定义分两步走*的结论。
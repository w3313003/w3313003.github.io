---
layout: post
title: "新手开发中常用ES6基础知识总结"
date: 2017-08-07
categories: [Vue]

---

* content
{:toc}

很早之前就学过TypeScript和ES6，后来做项目的时候零零散散用了些。这几天又系统地把ES6的知识看了一遍。感觉对我这种没实习没工作的新手，虽然一些高级的功能暂时用不上，但是一些基础的知识还是为平时的开发提供了巨大的便利。ES6学习告一段落，现在结合平时的开发，总结一些常用的知识。
<!-- more -->

{% raw %}

## let与const及其相关
### 块级作用域
ES6新增了块级作用域，总结一句话大致就是：大括号``{}``包起来的代码块基本山都可以当做块级作用域。

常见的有
1. 直接使用``{}``包起来：
 ```js
 {
  var a = 4
 }
 ```

2. 函数体大括号，``if-else``大括号，``for``循环大括号，``switch``大括号，``try-catch``大括号等。
	需要注意的是，``for``循环，每一次循环时的``{}``都是一个独立的块级作用域。
    ```js
     for(let i=0; a < 5; i++){
       let j = i
     }
    ```
    上面代码循环了五次，实际上有五个独立的``j``。

日常开发中，我们就可以利用这个特性，来创建块级作用域了。
### 块级作用域变量``let``与``const``
使用``let``或``const``声明的变量只在当前块级作用域生效，出了这个代码块，就无法访问。
```js
 {
   let a = 5
 }
console.log(a) 
//  Uncaught ReferenceError: a is not defined
```
日常开发中，块级作用域中使用的变量，尽量使用``let``或者``const``声明。

需要注意的问题:
1. ``let``和``const``变量一定要先声明，再使用，避免出错。不要试图利用变量提升的特性。
2. ``const``声明变量时，一定要初始化，否则会报错。``let``建议也在声明时初始化。
3. ``const``声明的变量一旦初始化，以后就不可以在进行赋值操作，但可以对其引用的对象进行更改。
	```js
	 const noChangeMe;	
     // Uncaught SyntaxError: Missing initializer in const declaration
	 const noChangeMe = [1,2,4]
	 noChangeMe = [2, 3]	
     // Uncaught TypeError: Assignment to constant variable
	 noChangeMe[100] = 100		// everything is OK
	```
4. ``let``和``const``声明的变量不能再重复声明。
	虽然``var``可以无限次重复声明，但是并不适用于这两个新的声明方式。
	```js
	 let a = 1
	 let a = 2		
     // Identifier 'a' has already been declared
	```
5. 不要用``window.xxx``去调用``let``与``const``声明的变量
	ES6规定，``let``、``const``、``class``声明的全局变量，不属于顶层对象的属性。

## String
### 使用反引号"`"
使用<code>``</code>将字符串包起来，可以解决下面的问题:
 - 字符串不能换行，如果换行只能使用``+``号
 - 字符串中的引号如果和最外层相同，需要进行转义
 - 字符串中插入变量，需要用``+``号
以前这样的写法:
```js
 var name = 'world'
 var str = "小明说:\"hello, " + name + "\""	   // 小明说:"hello, world"
```
现在可以方便的写作：
```js
 var name = 'world'
 str str = `小明说："hello, ${name}"`
```
总的来说，有三个好处:
1. 不怕字符串中出现的引号；
2. 不怕换行，反引号包起来的字符串可以随便换行；
3. 使用``${}``将变量或表达式包起来直接放在字符串中，不用写很多``+``

### 遍历字符串
使用``for...of``代替``for``循环:
```js
 var string = 'string'
 for(var i of string) {
 	console.log(i)
 }
```
### ``includes()``,``startsWidth()``,``endsWidth()``
不用使用``indexOf()``判断字符串中是否包含某个值了，使用``includes()``:
```js
 var string = 'string'
 string.includes('i'， 0)			// true
```
``includes``第二个参数表示查找的起始索引。
还可以使用``startsWidth()``和``endsWidth()``判断字符串是否以某些字符开始或结尾。

## 函数
### 参数的默认值
方便地设置函数参数的默认值
```js
 function print( a = 2 ){
 	console.log(a)
 }
 print()	//2
```

### 扩展运算符...获取其余参数
可以使用``...``加上变量名保存其他参数全部数量。
当只知道函数的前几个具体参数，不确定之后会有多少个参数传入时，可以使用``...``把其他传入的参数保存到一个**数组**中。
```js
 function print(value1, value2, ...values){
 	console.log(values.join('--'))
 }
 print(1, 2, '参数3')		// 参数3
 print(1, 2, '参数3', '参数4', '参数5')	// print(1, 2, '参数3', '参数4', '参数5')
```
### 使用箭头函数
使用箭头函数有两个好处:
1. 代码更加简洁
2. 静态绑定``this``
	箭头函数中，``this``指向的是定义箭头函数的对象中的``this``。
	```js
	 var name = 'outer'
	 var obj = {
	 	name: 'inner',
	 	func: () => {
	 		console.log(this.name)
	 	}
	 }
     var obj2 = {
         name: 'inner',
         func: function() {
             console.log(this.name)
         }
     }
     obj.func() // "outer"
     obj2.func()	// "inner"
	```
	第一个使用了箭头函数，由于箭头函数的``this``与其所在环境中的``this``相同，也就是与obj的``this``相同，而obj处于全局环境的活动对象中，``this``指向全局对象，这里指``window``，所以输出``outer``。
	**注意:**obj对象的``this``与它的属性中的``this``不一样。
	第二个使用了寻常函数，作为obj2的一个属性，``func``方法中的``this``指向了所在的对象。输出``inner``。

## 数组
### 使用``Array.from()``把类数组对象转为数组
一般来说，含有``length``属性的对象就可以当作类数组对象。平时获取多个DOM对象后，不能使用数组中的很多方法。我们可以使用``Array.from``方便的转换为数组,。
```js
 var divs = Array.from(document.querySelectorAll('div'))
 divs.forEach((value, index) => {})
```

### 使用``fill()``初始化数组
想要以某个值初始化数组，需要遍历。而使用``fill()``方法，就方便了很多。
```js
 var arr = new Array(100)  // 建立一个100元素的数组
 arr.fill('初始值', 0, arr.length)
```
第一个参数是要填充的值，后面两个与一般的数组方法一样，起始索引和结束索引(不包括)。

### 使用``includes``方法
和字符串的``includes``方法一样，看数组中是否有给定值。

## 对象
### 使用简单的属性表示和方法表示
```js
 var name = 'John'
 var a = {
 	name: name,
 	sayName: function(){ console.log(this.name) }
 }
```
改写为:
```js
 var name = 'John'
 var a = {
 	name,
 	sayName () { console.log(this.name) }
 }
```

### 记得``Object.is()``这个方法
其与``===``的差别:
```js
 NaN === NaN // false
 Object.is(NaN, NaN) // true

 -0 === +0   //true
 Object.is(+0, -0)		// false
 Object.is(+0, 0)		/ true
 Object.is(-0, 0)		/ false
```
可以这样理解,遵循的原则: 是同一个东西就要相等。
``NaN``与``NaN``就是一个东西，而``-0``带了个负号，和``0``与``+0``不一样。``0``和``+0``相同就像``1``和``+1``相同一样。

### 使用``Object.assign()``合并多个对象
``Object.assign()``是用来合并对象的。``assign``，译作``分配,指派``。这个方法本意是将某些对象自己的属性拷贝到目标对象上。它不回去复制继承来的属性。
比如可以在定义某个配置的时候，定义一个基础配置，在定义两个不同情况下的配置。使用时，进行合并。
```js
 var pathConfig = {
 	path: 'style/images'
 }
 var devConfig = {
 	baseUrl: 'http://localhost:8080/'
 }
 var buildConfig = {
 	baseUrl: 'https://192.128.0.2'
 }
 // 使用时，合并
 var mode = 'dev'
 var config = Object.assign({}, pathConfig, mode === 'dev' ? devConfig : buildConfig)
```
只是举个例子。

### ``Object.keys()``，``Object.values()``，``Object.entries``
这三个方法返回对象**自身**的，**可枚举**的，属性名为**字符串**的属性相关的东西，分别为：
- ``Object.keys()``: 属性名组成的数组
- ``Object.values()``： 属性值组成的数组
- ``Object.entries``: ``["key", "value"]``组成的数组。
```js
 var john = {
 	name: 'John',
 	age: 12
 }
 Object.keys(john) //  ["name", "age"]
 Object.values(john) //  ["John", 12]
 Object.entries(john) // [["name", "John"], ["age", 12]]
```

## 常用``...``运算符
前面在函数的剩余参数处理中提到了``...``扩展运算符。总结了一下，感觉有两个用法:
1. 用来读取数组或者对象的时候，是把数组或对象给扩展开；
2. 用来给对象或者数组赋值的时候，自动给对象或数组添加属性或元素。

### 用来读取
读取的时候就是把数组或者对象给扩展开来。

```js
 var a = [...[1,2,3], 4]		// 把数组的每一项都展出来
 a  // [1, 2, 3, 4]
 var obj = {
 	name: 'John',
 	age: 12
 }
 var newObj  = {...obj, job: 'teacher' }		// 把某个属性展出来
 newObj // {name: "John", age: 12, job: "teacher"}
```
所以可以很方便的用来扩展，合并数组或对象。

### 用作赋值
用作赋值的时候，是用作解构赋值，含义就是把等号右边表达式的剩余属性或数组项都放到``...``后面的变量里。
```js
 var a, restB
 [a, ...restB] = [1, 3, 5]
 a // 1
 restB //	[3, 5]

 var c, restD
 { name, ...restD } = {name: 'John', age: 12, job: 'teacher'}
 name  //  "John"  是一个属性的值
 restD	// 	{ c, ...restD } = {name: 'John', age: 12, job: 'teacher'} 是一个对象
```
对于对象的解构赋值，会把对应不到的属性全部放进``...``后面的变量对象中。

**注意**：因为是把剩下没人要的属性或者数组项都收下，所以``...``应该放在数组或者对象中的最后，且只能有一个。


如有错误，恳请指出！


{% endraw %}
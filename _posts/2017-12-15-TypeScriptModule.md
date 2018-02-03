---
layout: post
title: "TypeScript-Modules"
date: 2017-12-15
categories: [TypeScript]
datetime: 2017-12-15
tags: [TypeScript]

---
* content
{:toc}
模块是自声明的，两个模块之间的关系是通过在文件级别上使用 import 和 export 来建立的。TypeScript 和 ES6 一样，任何包含顶级 import 或者 export 的文件都会被当成一个模块。

<!-- more -->



## 模块

模块是在其自身的作用域里执行，并不是在全局作用域，这意味着定义在模块里面的变量、函数和类等在模块外部是不可见的，除非明确地使用 export 导出它们。类似地，我们必须通过 import 导入其他模块导出的变量、函数、类等。

模块使用模块加载器去导入它的依赖，模块加载器在代码运行时会查找并加载模块间的所有依赖。常用的模块加载器有 SystemJs 和 Webpack。

### 模块导出方式

模块可以通过导出方式来提供变量、函数、类、类型别名、接口等给外部模块调用，导出的方式分为以下三种：

1. 导出声明

任何模块都能够通过 export 关键字来导出，示例代码如下：


```js
export const COMPANY = 'EG';    // 导出变量

export interface IdentiryValidate {    // 导出接口
    isValidate(s: string): boolean;    
}

export class ErpIdentityValidate implements IdentiryValidate {    // 导出类
     isValidate(s: string) {
         return true;
     }
}

```
2. 导出语句

有时我们需要对导出的模块进行重命名，这个时候就用到了导出语句，示例代码如下：

```js
export class ErpIdentityValidate implements IdentiryValidate {    // 导出类
     isValidate(s: string) {
         return true;
     }
}

export { ErpIdentityValidate }
export { ErpIdentityValidate as EGIdentityValidate };    // 重命名

```
3. 模块包装

 
有时候我们需要修改和扩展已有的模块，并导出供其他模块调用，这时，可以时候模块包装来再次导出，我看到别的文章将其称为 “重新导出” 。示例代码如下：

```js
export { ErpIdentityValidate as EGIdentityValidate } from './ErpIdentityValidate';

```
一个模块可以包裹多个模块，并把新的内容以一个新的模块导出，示例代码如下：

```js
export * from './IdentiryValidate';
export * from './ErpIdentityValidate';

```

### 模块导入方式

模块导入与模块导出相对应，可以使用 import 关键字来导入当前模块依赖的外部模块。导入方式有如下几种：

1. 导入一个模块

```js
import { ErpIdentityValidate } from "./ErpIdentityValidate";
let erpValide = new ErpIdentityValidate();

```
2. 别名导入

```js
import { ErpIdentityValidate as ER} from "./ErpIdentityValidate";
let erpValide = new ERP ();

```
另外，我们也可以对整个模块进行别名导入，将整个模块导入到一个变量，并通过这个变量来访问模块的导出部分，实例代码如下：

```js
import * as validator from "./ErpIdentityValidate";
let myValidate = new validator.ErpIdentityValidate();

```

### 模块的默认导出

模块可以用 default 关键字实现默认导出的功能，每个模块可以有一个默认导出。类 和 函数声明 可以直接省略导出名来实现默认导出。默认导出有利于减少调用方调用模块的层数，省去一些冗余的模块前缀书写，示例代码如下：

1. 默认导出类

```js
// ErpIdentityValidate.ts
export default class ErpIdentityValidate implements IdentiryValidate {
     isValidate(s: string) {
         return true;
     }
}

// test.ts
import validator from "./ErpIdentityValidate";
let erp = new validator();

```
2. 默认导出函数

```js
// nameServiceValidate.ts
export default function(s: string){
    return true;    // 具体代码略
}

// test.ts
import validate from './nameServiceValidate';
let name = 'React';

// 使用导出函数
console.log(`"${name}" ${validate(name)? " matches": " does not matches"}`);

```

3. 默认导出值
```js
// constantService.ts
export default "React";

// test.ts
import name form "./constantService";
console.log(name);

```

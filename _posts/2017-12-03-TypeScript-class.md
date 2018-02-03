---
layout: post
title: "TypeScript -- 类"
date: 2017-12-03
categories: [TypeScript]
datetime: 2017-12-03
tags: [TypeScript]

---

* content
{:toc}
在ES6之前，JavaScript是使用构造函数和基于原型（Prototype）继承来创建可重用的“类”，。TypeScript 中可以支持使用基于类的面向对象编程。
<!-- more -->


**TypeScript-类**
> 和es6语法类似，TypeScript 中可以支持使用class语法创建一个类。

---
### 类的例子
example:
```ts
class Car {
    engine: string;
    constructor(engine: string) {
        this.engine = engine;
    }
    drive(distanceInMeters: number = 0) {
        console.log(`A car runs ${ distanceInMeters }m powered by` + this.engine);
    }
}
```
上面声明了一个汽车类，这个类有三个类成员：

- 类属性 engine，可通过 this.engine 访问

- 构造函数 constructor

- 方法 drive

下面实例化一个Car的对象，通过执行构造函数对其初始化。
```js
let car = new Car('petrol');
car.drive(100); // 输出：A car runs 100m powered by petrol

```
### 继承与多态

封装、继承、多态是面向对象的三个特性。上面的例子中把汽车的行为写到一个类中，即所谓的封装。在 TypeScript 中，使用 extends 关键字即可方便地实现继承。示例代码如下：

```js
// 继承自前文的 Car 类
class MotoCar extends Car {
    constructor(engine: string) { super(engine); }
}

class Jeep extends Car {
    constructor(engine: string) { super(engine); }
    drive(distanceInMeters: number = 100) {
        console.log("Jeep...")
        retrun super.drive(distanceInMeters);
    }
}

let tesla = new MotoCar('electricity');
let landRover: Car = new Jeep('petrol');

tesla.drive();    // 调用父类的 drive() 方法
landRover.drive(200);    // 调用子类的 drive() 方法

```
从上面的例子可以看到，MotoCar 和 Jeep 是基类Car的子类，通过 extends 来继承父类，子类可以访问父类的属性和方法，也可以重写父类的方法。Jeep 类重写了父类的 drive() 方法，这样 drive() 方法在不同的类中就具有不同的功能，这就是多态。

### 修饰符

在类中的修饰器可以分为公共（public）、私有（private）和受保护（protected）三种类型。

- public 修饰符

在 TypeScript 里，每个成员默认为 public，可以被自由访问，我们也可以显式地给 Car 类里定义的成员加上 public 修饰符，示例代码如下：

```js
class Car {
    public engine: string;
    public constructor(engine: string) {
        this.engine = engine;
    }
    public drive(distanceInMeters: number = 0) {
        console.log(`A car runs ${ distanceInMeters }m powered by` + this.engine);
    }
}

```
- private 修饰符

当类成员被标记为 private 时，就不能在类的外部访问它，示例代码如下：

```js
class Car {
    private engine: string;
    constructor(engine: string) {
        this.engine = engine;
    }
}

new Car('petrol').engine;    // 报错：engine 属性是私有的，只能在类内部访问

```

- protected 修饰符

protected 修饰符与 private 修饰符的行为很相似，但有一点不同，protected 成员在派生类中仍然可以访问，示例代码如下：
```js
class Car {
    private engine: string;
    constructor(engine: string) {
        this.engine = engine;
    }
    drive(distanceInMeters: number = 0) {
        console.log(`A car runs ${ distanceInMeters }m powered by` + this.engine);
    }
}
class MotoCar extends Car {
    constructor(engine: string) { super(engine); }
    drive(distanceInMeters: number = 100) {
        super.drive(distanceInMeters);
    }
}
let tesla = new MotoCar('electricity');
console.log(tesla.drive());    // 正常
console.log(tesla.engine);    // 报错

```
### 静态属性
类的静态成员存在于类本身而不是类的实例上，我们使用 类名. 来访问静态属性。可以使用 static 关键字来定义类的静态属性，示例代码如下：

```js
class Grid {
    static origin = {x: 0, y: 0};
    constructor(public scale: number) {}
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
}
let grid1  = new Grid(1.0);

let grid2  = new Grid(5.0);

// 输出：14.142...
console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
// 输出：2.828...
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));

```

### 抽象类

TypeScript 有抽象类的概念，他是供其他类继承的基类，不能直接实例化。抽象类必须包含一些抽象方法，同时也可以包含非抽象的成员。abstract 关键字用于定义抽象类和抽象方法。抽象类中的抽象方法不包含具体实现，但是必须在派生类中实现，示例代码如下:

```js
abstract class Person {
    abstract speak(): void;    // 必须在派生类中实现
    walking(): void {
        console.log('Walking on the road');
    }
}

class Male extends Person {
    speak(): void {
        console.log('How are you?');
    }
}

let person: Person;    // 创建一个抽象类的引用
person = new Person();    // 报错，不能创建抽象类实例
person = new Male();    // 创建一个 Male 实例
person.speak();
person.walking();

```
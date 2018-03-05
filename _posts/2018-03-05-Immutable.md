---
layout: post
title: "Immutable"
date: 2018-03-05
categories: [JavaScript]
datetime: 2018-03-05
tags: [JavaScript]

---
* content
{:toc}
JavaScript 中的对象一般是可变的（Mutable），因为使用了引用赋值，新的对象简单的引用了原始对象，改变新的对象将影响到原始对象。

<!-- more -->

# immutable
> JavaScript 中的对象一般是可变的（Mutable），因为使用了引用赋值，新的对象简单的引用了原始对象，改变新的对象将影响到原始对象。如 `foo={a: 1}; bar=foo; bar.a=2` 你会发现此时 `foo.a` 也被改成了 `2`。虽然这样做可以节约内存，但当应用复杂后，这就造成了非常大的隐患，Mutable 带来的优点变得得不偿失。为了解决这个问题，一般的做法是使用 shallowCopy（浅拷贝）或 deepCopy（深拷贝）来避免被修改，但这样做造成了 CPU 和内存的浪费。
Immutable 可以很好地解决这些问题。

## List.API
List 数据类型，对应原生 Array 数组。和原生数组，最大区别不存在'空位'。[, , , , ]

1. List.isList();
- 判断传入的参数是否为List
```ts
List.isList(maybeList: any): boolean
```

2. List.of()

- 创建新的List
```ts
List.of<T>(...values: Array<T>): List<T>
```
3. List.size 
- 返回List的长度(含empty
)
4. List.set
- 返回包含索引值的新列表。
如果索引已经存在于这个列表中，它将被替换。

```ts
set(index: number, value: T): List<T>
// example
const list = List([1,2,3]);
list.set(1, 4).toJS(); // return [1, 4, 3]
```

5. List.delete(alias: remove)
- 删除对应Index的值(类似arr.splice(i, 1))

```js
delete(index: number): List<T>

```
6. List.insert
- 在传入索引之前插入传入值
```ts
insert(index: number, value: T): List<T>

// example
    const list = List([1 ,3, 4]);
    const newList = list.insert(1, 5);
    console.log(newList.toJS()); return[1, 5, 3, 4]

```
7. List.clear
- 返回一个空的List
```ts
clear(): List
```
8. List.push
- 同arr.push，返回push后的新List
```ts
push(...values: Array<T>): List<T>
// example
List([ 1, 2, 3, 4 ]).push(5).toJS()// return [1, 2, 3, 4, 5]

```
9. List.pop
- 同arr.pop 返回删除当前List末尾值的新List
```ts
pop(): List<T>
// example
List([1, 2, 3, 4]).pop().toJS(); //return [1, 2, 3]
```
10. List.shift
- 同arr.shift 返回删除当前List首位值的新List
```ts
pop(): List<T>
// example
List([1, 2, 3, 4]).shift().toJS(); //return [2, 3, 4]
```
11. List.unshift
- 同arr.push，返回push后的新List

12. update
- 返回一个新的列表，其索引中有一个更新值，返回值是使用现有值调用updater。如果传入的索引没有匹配值，则把notSetValue作为updater的参数传入，将updater的返回值作为传入的Index的值，并入新列表。
如果使用单个参数调用，则使用列表本身调用updater。
```ts
update(index: number, notSetValue: T, updater: (value: T) => T): this
update(index: number, updater: (value: T) => T): this
update<R>(updater: (value: this) => R): R;
// example
const list = List([1,2,3]);
list.update(0, val => val + 2).toJS;// return [3, 2, 3];
list.update(3, 5, val => val + 2).toJS()// return [1, 2, 3, 7];
list.update(val => val.push('x')).toJS()//
return [1, 2, 3, 'x'];

```

13. List.setSize
- 返回长度为传入值的新List，若参数大于当前长度，则用empty填充;若参数小于当前长度,则排除大于参数的索引的值;
```ts
setSize(size: number): List<T>
```

14. List.setIn
- 返回在keyPath处具有设置值的新List。 如果keyPath中的任何键不存在，那么将在该键上创建一个新的不可变Map
```ts
setIn(keyPath: Iterable<any>, value: any): this
// keyPath([index, key])首项为index 其余成员为key如{a:b:c:{age:123}} -> [index,'a', 'b', 'c','age']
const list = List([ 0, 1, 2, List([ 3, 4 ])])
list.setIn([3, 0], 999);
// List [ 0, 1, 2, List [ 999, 4 ] ];
const list = Immuatable.fromJS([1, 2, 3, {name: 'zj'}]);
list.setIn([3,'name'], 'ZJ').toJS();
//[1, 2, 3, {name: 'ZJ'}];
```

15. List.deleteIn
- 返回一个新的列表，该列表删除了keypath的值。
如果keypath中的任何键不存在，则不会发生更改。
```ts
deleteIn(keyPath: Iterable<any>): this

```

16. List.updateIn   
- 同update In版
```ts
updateIn(keyPath: Iterable<any>, notSetValue: T, updater: (value: T) => T): this

```
17. List.mergeIn
- 返回一个修改了keyPath对应值的新List
```ts

```

更新中。。。。。。
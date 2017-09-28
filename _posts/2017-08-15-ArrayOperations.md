---
layout: post
title: "常见的数组处理操作"
date: 2017-08-15
categories: [JavaScript]

---

* content
{:toc}

这里总结一些日常开发中对数组的操作，包括数组去重。
<!-- more -->


## 数组去重
1. 使用ES6的``Set``对象。
 ``Set``数据结构不含有重复的值，可以利用这个特性对数组去重。
 ```js
  let arr = [1,2,4,5,3,4,5,'hello', 'hello']
  function unDuplicateArray(arr) {
    let set = new Set(arr)
    return [...set]
  }
  let newarr = unDuplicateArray(arr) // [1, 2, 4, 5, 3, "hello"]
 ```
2. 创建新数组，以不重复的方式加入到新数组中，然后返回该数组。
 ```js
  let arr = [1,2,4,5,3,4,5,'hello', 'hello']
  function unDuplicateArray(arr) {
    let tmp = []
    for(let ele of arr) {
    	if(!tmp.includes(ele)) {
      	tmp.push(ele)
      }
    }
    return tmp
  }
  let newArr = unDuplicateArray(arr)	 // [1, 2, 4, 5, 3, "hello"]
 ```
3. 看到别人说的还有一种利用键值反转再反转的方式去重。看代码。
 ```js
  let arr = [1,2,4,5,3,4,5,'hello', 'hello']
  function unDuplicateArray(arr) {
  	let tmp = []
    for(let ele of arr) {
    	tmp[ele] = 1
    }
    let newTmp = []
    for(let n of tmp.values()) {
    	newTmp.push(n)
    }
    return newTmp
  }
  let newArr2 = unDuplicateArray(arr) //  ["1", "2", "3", "4", "5", "hello"]
 ```
 可以看出，这种方法不可行，因为传给对象的键名会自动转换成字符串，所以原来的数值类型的元素都成了字符串。
 **但是**,自从有了ES6的``Map``，情况就不一样了。``Map``也是存储"键值对"的数据结构，但是它的键名可以是任意类型的。
 ```js
  let arr = [1,2,4,5,3,4,5,'hello', 'hello']
  function unDuplicateArray(arr) {
  	let tmp = new Map()
    for(let ele of arr) {
    	tmp.set(ele, 1)
    }
    return [...tmp.keys()]
  }
  let newArr2 = unDuplicateArray(arr) //  [1, 2, 4, 5, 3, "hello"]
 ```
 代码也简洁了许多。能用``Map``的地方，肯定可以用``Set``，所以还是``Set``更简单。
4. 以上方法都是需要改变原数组的引用地址的。也就是说，如果之前将这个数组共享给了其他变量，那么这样对数组的更改无法反映到之前的变量中。想要对数组本身做修改，可以利用``indexOf()``方法。
 ```js
  let arr = [1,2,4,5,3,4,5,'hello', 'hello']
  function unDuplicateArray(arr) {
    let i = 0;
    for(i = 0;i<arr.length;i++){
      let fromIndex = i+1
      let findIndex = arr.indexOf(arr[i], fromIndex)
        console.log("1:"+findIndex)
      while(findIndex != -1){
        console.log("2:"+findIndex)
        arr.splice(findIndex, 1)
        fromIndex = findIndex
         findIndex = arr.indexOf(arr[i], fromIndex)
      }
    }
  }
  newArr2 = unDuplicateArray(arr) //  [1, 2, 4, 5, 3, "hello"]
 ```

**注意**:以上方法试用类型为基本类型，也就是``number``,``string``和``boolean``，对于引用类型，不建议使用。除非重复出现的元素都是引用的同一个对象。


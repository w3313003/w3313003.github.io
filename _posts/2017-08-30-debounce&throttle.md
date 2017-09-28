---
layout: post
title: "很简单的函数去抖与函数节流实现(debounce and throttle)"
date: 2017-08-30
categories: [JavaScript]

---

* content
{:toc}

函数去抖和函数节流经常用来防止函数高频率地调用。最常用的场景就是事件处理器，比如鼠标的``mousemove``事件，元素的``scroll``事件。
<!-- more -->


## 函数去抖
### 概念
**去抖与节流**只是一种形象的说法，看图片更加简单。
函数去抖(throttle)就是把间隔少于一定时间的，对函数的调用全都合并为一次调用，在最后一次调用之后的一段时间后，才去真正地执行函数。像下图所示：
<div style="text-align: center;"><img src="/assets/images/JavaScript/debounce&throttle/debounce.png"></div>
<br>
对函数特别密集的调用都会当作一次调用，但是由于无法确定哪一次才是真正的最后一次，所以需要延迟一段时间才能确定可以执行该函数。
这样做，就相当于强行对某个函数使用了延迟执行的策略，比如``scroll``事件的处理函数。我们希望事件触发不要太密集，就可以设置100ms触发一次。但是按照上面的策略，当鼠标开始滑动100ms后，处理函数才开始执行，这就显得反应迟钝。所以我们可以让某段时间内，事件第一次触发的时候就调用处理函数。在此后的一段时间，如果再次触发，就应用上面的策略，在最后一次触发后，才开始执行处理函数。如下图所示：
<div style="text-align: center;"><img src="/assets/images/JavaScript/debounce&throttle/debounce-leading.png"></div><br>

### 代码实现
代码实现的思路很简单。比较流行的方法就是参数要去抖的函数，然后对这个函数进行加工，最后返回一个满足条件的函数。在这里，我们增加两个选项，``leading``和``trailing``参数，用来控制是否在一段函数被密集调用的间隔内，是否在函数首次调用时执行函数，以及是否在最后一次调用函数后的一段时间后执行函数。

```js
function debounce(func, wait = 0, leading = true) {
  var timeout = null

  return function(){

    if (timeout === null) {
      if(leading) {
      	func(...arguments)
      }
      timeout = setTimeout(() => {
        timeout = null
      }, wait)
    } else {
      clearTimeout(timeout)
      timeout = setTimeout(() => {
        timeout = null
        if (trailing) {
          func(...arguments)
        }
      }, wait)
    }
  }
}

```

## 函数节流
函数节流就是控制某段时间内函数只能执行一次。
```js
function throttle(func, wait) {
  var timeout = null

  return function() {
    if(timeout === null) {
      func(...arguments)
      timeout = setTimeout(() => {
        timeout = null
      }, wait)
    }
  }
}
```

本文首发于[我的博客](https://zhuqingguang.github.io)

参考自[这里](https://css-tricks.com/debouncing-throttling-explained-examples/)




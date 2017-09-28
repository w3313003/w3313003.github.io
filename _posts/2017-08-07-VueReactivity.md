---
layout: post
title: "Vue深入响应式原理讲了啥"
date: 2017-08-07
categories: [Vue]

---

* content
{:toc}

看了Vue响应式原理的教程，知识点有点散，总结一下。[原文](https://cn.vuejs.org/v2/guide/reactivity.html)
<!-- more -->

{% raw %}

## 如何跟踪变化

1. Vue把创建实例时的配置中的数据都用``Object.defineProperty``重新改成了*存取器属性(getter/setter)*，所以在设置属性的值的时候，就可以在``set``方法中，通知Vue数据被重新赋值

2. 每个Vue实例都有一个``watcher``，其实就是监听各个属性的改变，当有了改变之后，就通知相应的组件进行变化

![Vue数据监测](/assets/images/data.png)



## 变化检测问题

1. 使用一般的``vm.newObj = 5``给Vue实例添加的属性不是具有响应式的属性。这里的不**具有响应式**是指，当该属性变化时，实例的``watcher``不能检测到，DOM也就无法重新渲染。这是因为Vue实例没有把这个属性变为*存取器属性(getter/setter)*。
  但是，当有别的响应式属性改变，导致**重新渲染DOM**时，这个非响应式属性的改变也能体现出来。
  ```html
    <div id="app">
     <button type="" @click="change">点击</button>{{hello}}
    </div>
    <script>
      var vm = new Vue({
        el: '#app',
        data: {
          message: 1
        },
        methods: {
          change () {
            this.hello++
            this.message++		// 通过改变另一个响应式属性，引起DOM重新渲染
          }
        }
      })
      vm.hello = 1
    </script>
  ```
  如上代码，使用``vm.hello``直接向Vue实例添加了一个属性，但是这个属性由于没有在创建实例的时候，被转换为*存取器属性(getter/setter)*，所以，他的变化不能引起DOM重新渲染。
  但是我们在改变非响应式属性的时候，还改变了一个响应式 属性，那么非响应式属性的变化也能体现出来了。
  这样做完全是没有必要的，最好就是在创建实例的时候通过``data``对象把属性都设置好。

2. 使用``Vue.set(object, key, value)``可以向Vue中的某个对象属性添加一个响应式属性
  ```html
    <div id="app10">
    <button type="" @click="change">点击</button>{{message.text}}</div>
    <script>
      var vm = new Vue({
        el: '#app10',
        data: {
          message: {
          }
        },
        methods: {
          change () {
            this.message.text++
          }
        }
      })
      Vue.set(vm.message, 'text', 1)
    </script>
  ```
  这样，``message``对象的``text``属性就变成响应式的了。
  也可以使用Vue实例的``$set``方法。
  ```html
 	  <script>
      var vm = new Vue({
        el: '#app10',
        data: {
          message: {
          }
        },
        created () {
          this.$set(this.message, 'text', 1)
        },
        methods: {
          change () {
            this.message.text++
          }
        }
      })
    </script>
  ```
3. Vue实例无法检测对象内部的改变
 直接向Vue实例中的对象属性添加新的属性时，Vue实例的``watcher``是检测不到的。因为对于这种改变，watcher无法检测到。因为并没有给该对象属性重新赋值，也就没有调用它的``setter``方法，而Vue正是依靠这个方法来检测变化的。
  如果想对某个对象做更改而引起变化，需要给这个对象重新赋值，也就是改变其引用。

## 异步更新队列

Vue中数据的改变并不会马上引起DOM的变化，而是把这个改变放到任务队列中，然后下次事件循环时，在更新DOM。

这样的话，当修改了某个数据，并希望在DOM更改完成后做一些事，就无法实现了。因为并没有那样的回调。

但是，我们在更改了数据之后，可以使用``Vue.nextTick(callback)``或者``vm.$nextTick(callback)``，这样可以在数据更改后的第一次DOM更新完成后，立刻调用传进来的回调函数。

{% endraw %}
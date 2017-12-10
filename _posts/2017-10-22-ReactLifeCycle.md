---

layout: post
title: "React生命周期"
date: 2017-10-22
categories: [JavaScript]
datetime: 2017-10-22
tags: [JavaScript]

---

* content
{:toc}

最近学习了react。 React生命周期分为三个阶段，每一个阶段都有不同的函数执行顺序，值得注意的是实例化和销毁阶段执行一次，存在期是可以反复执行的：

<!-- more -->

> 最近学习了react。 React生命周期分为三个阶段，每一个阶段都有不同的函数执行顺序，值得注意的是实例化和销毁阶段执行一次，存在期是可以反复执行的：

## 1.组件实例化阶段 
**（在实例化阶段只会执行一次）**
- constructor 
- componentWillMount
- render
- componentDidMount

## 2.组件存在阶段 
**（在组件存在的阶段，每重新渲染一次，都会执行一遍） **
- componentWillReceiveProps（props改变时触发）
- shouldComponentUpdate（特别注意，如果返回false，则不会执行渲染，也会跳过render前后的钩子函数） 
- componnetWillUpdate 
- render
- componentDidUpdate（渲染之后，可以通过ref或者findDOMNode，来操作DOM，当React运行在服务端是，不会被调用） 
## 3.组件销毁阶段 
- componentWillUnmount 

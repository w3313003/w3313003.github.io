---
layout: post
title: "七种实现左侧固定，右侧自适应两栏布局的方法"
date: 2017-08-16
categories: [CSS]

---

* content
{:toc}

总结一下左边固定，右边自适应的两栏布局的七种方法。其中有老生常谈的``float``方法,BFC方法，也有CSS3的``flex``布局与``grid``布局。并非所有的布局都会在开发中使用，但是其中也会涉及一些知识点。关于最终的效果，可以查看[这里](https://zhuqingguang.github.io/vac-works/cssLayout/index1.html)
<!-- more -->

常用的宽度自适应的方法通常是利用了``block``水平的元素宽度能随父容器调节的流动特性。另外一种思路是利用CSS中的``calc()``方法来动态设定宽度。还有一种思路是，利用CSS中的新型布局``flex layout``与``grid layout``。

首先创建基本的HTML布局和最基本的样式。
```html
<div class="wrapper" id="wrapper">
  <div class="left">
    左边固定宽度，高度不固定 </br> </br></br></br>高度有可能会很小，也可能很大。
  </div>
  <div class="right">
    这里的内容可能比左侧高，也可能比左侧低。宽度需要自适应。</br>
    基本的样式是，两个div相距20px, 左侧div宽 120px
  </div>
</div>
```
基本的样式是，两个盒子相距``20px``, 左侧盒子宽 ``120px``，右侧盒子宽度自适应。基本的CSS样式如下:
```css
.wrapper {
    padding: 15px 20px;
    border: 1px dashed #ff6c60;
}
.left {
    width: 120px;
    border: 5px solid #ddd;
}
.right {
    margin-left: 20px;
    border: 5px solid #ddd;
}
```
下面的代码都是基于这套基本代码做覆盖，通过给容器添加不同的类来实现效果。

## 双``inline-block``方案
```css
.wrapper-inline-block {
    box-sizing: content-box;
    font-size: 0;	// 消除空格的影响
}

.wrapper-inline-block .left,
.wrapper-inline-block .right {
    display: inline-block;
    vertical-align: top;	// 顶端对齐
    font-size: 14px;
    box-sizing: border-box;
}

.wrapper-inline-block .right {
    width: calc(100% - 140px);
}
```
这种方法是通过``width: calc(100% - 140px)``来动态计算右侧盒子的宽度。需要知道右侧盒子距离左边的距离，以及左侧盒子具体的宽度(content+padding+border)，以此计算父容器宽度的``100%``需要减去的数值。同时，还需要知道右侧盒子的宽度是否包含``border``的宽度。
在这里，为了简单的计算右侧盒子准确的宽度，设置了子元素的``box-sizing:border-box;``以及父元素的``box-sizing: content-box;``。
同时，作为两个``inline-block``的盒子，必须设置``vertical-align``来使其顶端对齐。
另外，为了**准确地应用**计算出来的宽度，需要消除``div``之间的空格，需要通过设置父容器的``font-size: 0;``,或者用注释消除``html``中的空格等方法。
**缺点:**
- 需要知道左侧盒子的宽度，两个盒子的距离，还要设置各个元素的``box-sizing``
- 需要消除空格字符的影响
- 需要设置``vertical-align: top``满足顶端对齐。

## 双``float``方案
```css
.wrapper-double-float {
    overflow: auto;		// 清除浮动
    box-sizing: content-box;
}

.wrapper-double-float .left,
.wrapper-double-float .right {
    float: left;
    box-sizing: border-box;
}

.wrapper-double-float .right {
    width: calc(100% - 140px);
}
```
本方案和双``float``方案原理相同，都是通过动态计算宽度来实现自适应。但是，由于浮动的``block``元素在有空间的情况下会[依次紧贴，排列在一行](https://www.w3.org/TR/CSS21/visuren.html#bfc-next-to-float)，所以无需设置``display: inline-block;``，自然也就少了顶端对齐，空格字符占空间等问题。
> A floated box is shifted to the left or right until its outer edge touches the containing block edge or the outer edge of another float.

不过由于应用了浮动，父元素需要清除浮动。
**缺点:**
- 需要知道左侧盒子的宽度，两个盒子的距离，还要设置各个元素的``box-sizing``。
- 父元素需要清除浮动。

## ``float+margin-left``方案
```css
.wrapper-float {
    overflow: hidden;		// 清除浮动
}

.wrapper-float .left {
    float: left;
}

.wrapper-float .right {
    margin-left: 150px;
}
```
上面两种方案都是利用了CSS的``calc()``函数来计算宽度值。下面两种方案则是利用了``block``级别的元素盒子的宽度具有**填满父容器，并随着父容器的宽度自适应**的**流动特性**。
但是``block``级别的元素都是独占一行的，所以要想办法让两个``block``排列到一起。
我们知道，``block``级别的元素会认为浮动的元素不存在，但是``inline``级别的元素能识别到浮动的元素。这样，``block``级别的元素就可以和浮动的元素同处一行了。
为了让右侧盒子和左侧盒子保持距离，需要为左侧盒子留出足够的距离。这个距离的大小为左侧盒子的宽度以及两个盒子之间的距离之和。然后将该值设置为右侧盒子的``margin-left``。
**缺点：**
- 需要清除浮动
- 需要计算右侧盒子的``margin-left``

## 使用``absolute+margin-left``方法
另外一种让两个``block``排列到一起的方法是对左侧盒子使用``position: absolute``的绝对定位。这样，右侧盒子也能**无视**掉它。
```css
.wrapper-absolute .left {
    position: absolute;
}

.wrapper-absolute .right {
    margin-left: 150px;
}
```
**缺点:**
- 使用了绝对定位，若是用在某个div中，需要更改父容器的``position``。
- 没有清除浮动的方法，若左侧盒子高于右侧盒子，就会超出父容器的高度。因此只能通过设置父容器的``min-height``来放置这种情况。

## 使用``float+BFC``方法
上面的方法都需要通过左侧盒子的宽度，计算某个值，下面三种方法都是不需要计算的。只需要设置两个盒子之间的间隔。
```css
.wrapper-float-bfc {
    overflow: auto;
}

.wrapper-float-bfc .left {
    float: left;
    margin-right: 20px;
}

.wrapper-float-bfc .right {
    margin-left: 0;
    overflow: auto;
}
```
这个方案同样是利用了左侧浮动，但是右侧盒子通过``overflow: auto;``形成了BFC，因此右侧盒子不会与浮动的元素重叠。
> an element in the normal flow that establishes a new block formatting context (such as an element with 'overflow' other than 'visible') must not overlap the margin box of any floats in the same block formatting context as the element itself。

这种情况下，只需要为左侧的浮动盒子设置``margin-right``，就可以实现两个盒子的距离了。而右侧盒子是``block``级别的，所以宽度能实现自适应。
**缺点:**
- 父元素需要清除浮动

## ``flex``方案
```css
.wrapper-flex {
    display: flex;
    align-items: flex-start;
}

.wrapper-flex .left {
    flex: 0 0 auto;
}

.wrapper-flex .right {
    flex: 1 1 auto;
}
```
``flex``可以说是最好的方案了，代码少，使用简单。有朝一日，大家都改用现代浏览器，就可以使用了。
**需要注意**的是，``flex``容器的一个默认属性值:``align-items: stretch;``。这个属性导致了列等高的效果。
为了让两个盒子高度自动，需要设置: ``align-items: flex-start;``

## ``grid``方案
又一个新型的布局方式。可以满足需求，但这并不是它发挥用处的真正地方。
```css
.wrapper-grid {
    display: grid;
    grid-template-columns: 120px 1fr;
    align-items: start;
}

.wrapper-grid .left,
.wrapper-grid .right {
    box-sizing: border-box;
}

.wrapper-grid .left {
    grid-column: 1;
}

.wrapper-grid .right {
    grid-column: 2;
}
```
**注意:**
- ``grid``布局也有列等高的默认效果。需要设置: `` align-items: start;``。
- ``grid``布局还有一个值得注意的小地方和``flex``不同:在使用``margin-left``的时候，``grid``布局默认是``box-sizing``设置的盒宽度之间的位置。而``flex``则是使用两个div的``border``或者``padding``外侧之间的距离。

## 极限情况
最后可以再看一下在父容器极限小的情况下，不同方案的表现。主要分成四种情况：

- 动态计算宽度的情况
  	
    两种方案: 双inline-block方案和双float方案。宽度极限小时，右侧的div宽度会非常小，由于遵循流动布局，所以右侧div会移动到下一行。

- 动态计算右侧margin-left的情况
  	
    两种方案: float+margin-left方案和absolute+margin-left方案。宽度极限小时，由于右侧的div忽略了文档流中左侧div的存在，所以其依旧会存在于这一行，并被隐藏。

- ``float+BFC``方案的情况
  	
    这种情况下，由于BFC与float的特殊关系，右侧div在宽度减小到最小后，也会掉落到下一行。

- ``flex``和``grid``的情况
  	
    这种情况下，默认两种布局方式都不会放不下的div移动到下一行。不过 flex布局可以通过 flex-flow: wrap;来设置多余的div移动到下一行。 grid布局暂不支持。
 
如果感觉写的有问题，恳请指出。

本文首发于[我的博客](https://zhuqingguang.github.io)




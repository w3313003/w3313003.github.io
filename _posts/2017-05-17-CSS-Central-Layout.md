---
layout: post
title: "CSS居中总结"
date: 2017-05-17
categories: [CSS]
tags: [CSS]
---

* content
{:toc}


[原文](https://codepen.io/shshaw/full/gEiDt)
详细讲解了各种水平和垂直居中的方法，并给出了浏览器兼容性和优缺点。
<!-- more -->
### 各种方式对比总结
<div class="overflow-x">
<table class="table">
        <thead>
          <tr>
            <th scope="col" class="Technique-Title">Technique</th>
            <th scope="col" class="Browser-Support">Browser Support</th>
            <th scope="col" class="Responsive-Support">Responsive</th>
            <th scope="col" class="Overflow-Support">Overflow</th>
            <th scope="col" class="Resize-Support"><code>resize:both</code></th>
            <th scope="col" class="Height-Support">Variable Height</th>
            <th scope="col" class="Major-Caveats">Major Caveats</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th scope="row" class="Technique-Title"><a rel="nofollow" href="#Overview">Absolute Centering</a></th>
            <td class="Browser-Support">Modern &amp; IE8+</td>
            <td class="Responsive-Support">Yes</td>
            <td class="Overflow-Support">Scroll, can overflow container</td>
            <td class="Resize-Support">Yes</td>
            <td class="Height-Support">Yes*</td>
            <td class="Major-Caveats"><a rel="nofollow" href="#Height">Variable Height</a> not perfect cross-browser</td>
          </tr>
          <tr>
            <th scope="row" class="Technique-Title"><a rel="nofollow" href="#Negative-Margins">Negative Margins</a></th>
            <td class="Browser-Support">All</td>
            <td class="Responsive-Support">No</td>
            <td class="Overflow-Support">Scroll</td>
            <td class="Resize-Support">Resizes but doesn't stay centered</td>
            <td class="Height-Support">No</td>
            <td class="Major-Caveats">Not responsive, margins must be calculated manually</td>
          </tr>
          <tr>
            <th scope="row" class="Technique-Title"><a rel="nofollow" href="#Transforms">Transforms</a></th>
            <td class="Browser-Support">Modern &amp; IE9+</td>
            <td class="Responsive-Support">Yes</td>
            <td class="Overflow-Support">Scroll, can overflow container</td>
            <td class="Resize-Support">Yes</td>
            <td class="Height-Support">Yes</td>
            <td class="Major-Caveats">Blurry rendering</td>
          </tr>
          <tr>
            <th scope="row" class="Technique-Title"><a rel="nofollow" href="#Table-Cell">Table-Cell</a></th>
            <td class="Browser-Support">Modern &amp; IE8+</td>
            <td class="Responsive-Support">Yes</td>
            <td class="Overflow-Support">Expands container</td>
            <td class="Resize-Support">No</td>
            <td class="Height-Support">Yes</td>
            <td class="Major-Caveats">Extra markup</td>
          </tr>
          <tr>
            <th scope="row" class="Technique-Title"><a rel="nofollow" href="#Inline-Block">Inline-Block</a></th>
            <td class="Browser-Support">Modern, IE8+ &amp; IE7*</td>
            <td class="Responsive-Support">Yes</td>
            <td class="Overflow-Support">Expands container</td>
            <td class="Resize-Support">No</td>
            <td class="Height-Support">Yes</td>
            <td class="Major-Caveats">Requires container, hacky styles</td>
          </tr>
          <tr>
            <th scope="row" class="Technique-Title"><a rel="nofollow" href="#Flexbox">Flexbox</a></th>
            <td class="Browser-Support">Modern &amp; IE10+</td>
            <td class="Responsive-Support">Yes</td>
            <td class="Overflow-Support">Scroll, can overflow container</td>
            <td class="Resize-Support">Yes</td>
            <td class="Height-Support">Yes</td>
            <td class="Major-Caveats">Requires container, vendor prefixes</td>
          </tr>
        </tbody>
      </table>      
</div>

## absolute centering

- html
```
  <div class="container">
    <div class="content">
      // Content
    </div>
  </div>
```

- css
```
  .container {
      position: relative;
  }
  .content {
      position: absolute;
      margin: auto;
      top: 0; left: 0; bottom: 0; right: 0;
  }
```


 **注意**: 当没有指定内容块的具体的高度和宽度时，内容块会填满剩余空间。可以通过使用``max-height``来限制高度，也可以通过 display:table 来使高度由内容来决定，但是浏览器支持不是很好。
 - Not compatible with the Resizing technique.
 - Firefox/IE8: Using display: table aligns the content block to the top, but is still centered horizontally.
 - IE9/10: Using display: table aligns the content block to the top left.
 - Mobile Safari: The content block is centered vertically, but becomes slightly off-center horizontally when using percentage based widths.

## Negative margins
- html
```
  <div class="isNegative">
    //Content
  </div>
```

- css
```
  .isNegative {
      position: relative;
      width: 200px;
      height: 300px;
      left: 50%;
      top: 50%;
      margin-left: -100px;
      margin-top: -150px;
  }
```

 **缺点**:需要知道具体的高度和宽度

## Transform
```
  .content {
      position: relative;
      left: 50%;
      top: 50%;
      -webkit-transform: translate(-50%,-50%);
        -ms-transform: translate(-50%,-50%);
            transform: translate(-50%,-50%);
  }
```
 这里translate的百分比是相对于自身的，所以高度是可变的


## Table-Cell
- html
```
  <div class="Center-Container is-Table">
    <div class="Table-Cell">
      <div class="Center-Block">
      <!-- CONTENT -->
      </div>
    </div>
  </div>
```

- css
```
  .Center-Container.is-Table { display: table; }
  .is-Table .Table-Cell {
    display: table-cell;
    vertical-align: middle;
  }
  .is-Table .Center-Block {
    width: 50%;
    margin: 0 auto;
  }
```

 **注意**: 需要添加最内层的div，并且给div指定宽度和margin:0 auto;来使div居中。

## Inline-Block
- html
```
  <div class="Center-Container is-Inline">
    <div class="Center-Block">
      <!-- CONTENT -->
    </div>
  </div>
```

- css
```
  .Center-Container.is-Inline {
    text-align: center;
    overflow: auto;
  }

  .Center-Container.is-Inline:after,
  .is-Inline .Center-Block {
    display: inline-block;
    vertical-align: middle;
  }

  .Center-Container.is-Inline:after {
    content: '';
    height: 100%;
    margin-left: -0.25em; /* To offset spacing. May vary by font */
  }

  .is-Inline .Center-Block {
    max-width: 99%; /* Prevents issues with long content causes the content block to be pushed to the top */
    /* max-width: calc(100% - 0.25em) /* Only for IE9+ */
  }
```
 - 空内容也会占据一定空间，需要margin-left:-0.25em;来抵消偏移
 - 内容块的最大宽度不能是100%，否则会把::after的内容挤到下一行

## Flex
- html
 ```
   <div class="contain">
     <div class="content">
        // content
     </div>
   </div>
 ```

- css
 ```
  .container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
 ```
 最方便最简单的方式，但是要注意浏览器的支持

---

layout: post
title: "CSS背景总结"
date: 2017-05-15
categories: [ CSS]
datetime: 2017-05-15
tags: [CSS]
---

* content
{:toc}

详细记录了CSS中与背景设置有关的属性和取值。
<!-- more -->
### 设置背景内容
- **background-color**: ``grba() | red | #ddd``;设置颜色
- **background-image**: ``url()``;

### 设置背景位置
- **background-origin**: border-box | padding-box | content-box;
  用于设置显示背景的区域
- **background-position**: 100% 90% | left top | 2px 4px;
  用于设置背景图片的位置
- **background-attachment**: fixed | scroll | local ;
  fixed: 设置背景为相对于浏览器窗口固定
  scroll: 设置背景相对于容器窗口固定，内容滚动时，背景不变，但是当容器滚动时，背景会随着容器一起滚动
  local: 设置背景相对于容器的内容区域固定。内容滚动时，背景也会变化

### 设置背景大小
- **background-size**: cover | contain | auto | 30% 30% | 1px 2px;
  cover: 背景会尽可能大的缩放图片，在保持比例不变的情况下，使图片完全覆盖容器
  contain: 背景会尽可能大的缩放图片，在保持比例不变的情况下，使图片在容器范围内
  auto: 会自动的确定图片大小
  **值得注意的是**，取值为cover时，切换背景图片会产生拉伸或者缩放的动画效果
- **background-clip**: border-box | padding-box | content-box;
  设置背景裁剪的范围，取值是与盒模型相对应的。

### 设置背景重复
- **background-repeat**: repeat|no-repeat|round|space|repeat-x|repeat-y;
  设置背景的重复及排列方式

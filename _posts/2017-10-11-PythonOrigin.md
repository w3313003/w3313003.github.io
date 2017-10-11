---
layout: post
title: "windows下pip源替换"
date: 2017-10-11
categories: [Python]
datetime: 2017-10-11
tags: [Python]

---

* content
{:toc}

经常在使用Python的时候需要安装各种模块，而pip是很强大的模块安装工具，但是由于国外官方pypi经常被墙，导致不可用，所以我们最好是将自己使用的pip源更换一下，这样就能解决被墙导致的装不上库的烦恼。

<!-- more -->

windows下pip更新源替换
=

> 今天初次安装Django，由于默认安装时的源大都是外国的更新源，速度相对国内会慢很多，经常出现timeout，开始的时候安装了7、8次都没成功经过一番折腾终于替换成功，这里给大家介绍一下本人使用的方法。


1.打开 'C:\Users\username\AppData\Roaming'文件夹`(或者直接在地址栏输入'%APPDATA%')，回车即可`

2.新建一个名为pip的文件夹，并在其中新建pip.ini的配置文件

3.在新建的pip.ini文件中输入以下内容，搞定
```
[global]  
index-url = https://pypi.doubanio.com/simple/  
[install]  
trusted-host=pypi.doubanio.com  
disable-pip-version-check = true  
timeout = 6000  

```

**搞定，再次使用pip install体验一下不一样的速度吧！**
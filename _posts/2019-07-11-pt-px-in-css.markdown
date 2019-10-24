---
layout:     post
title:      "CSS 中的几种度量单位"
subtitle:   ""
date:       2019-07-11 18:07:12
author:     "ywlvs"
header-img: "img/post-bg-docker-logo-whale.png"
catalog: true
tags:
    - CSS
---

## 物理像素

即设备像素，指设备屏幕上的物理像素，一个指定设备屏幕的物理像素是不会改变的。物理像素的单位是 pt。比如 iPhone 5 的设备物理分辨率是 1136pt x 640pt。


## 物理像素密度

单位长度内像素点的数量，单位是 dpi（dot per inch），即每英尺内像素点的数量。比如 iPhone 5 拥有 4 inch 的屏幕，即对角线长度为 4 inch，计算方式为 (1136^2 + 640^2)^0.5/4，结果取整之后就是 326，这就是该设备的物理像素密度。

## CSS 像素

是 CSS、JS 中的一个抽象的概念，用于控制页面元素的大小，单位是 px。

> CSS 像素也被称为设备独立像素（device-dependent-pixels），简称 dips，单位是 dp。

一个 CSS 像素对应多少个物理像素，取决于两个条件：

+ 页面是否缩放
+ 屏幕是否是高密度


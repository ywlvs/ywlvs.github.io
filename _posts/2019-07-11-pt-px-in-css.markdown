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

即设备像素，指设备屏幕上的物理像素，一个指定设备屏幕的物理像素是不会改变的。物理像素的单位是 pt，即 point 的缩写。比如 iPhone 5 的设备物理分辨率是 1136pt x 640pt。在操作系统的调度下，每个设备像素都有自己的颜色值和亮度值。

## 物理像素密度

单位长度内像素点的数量，单位是 dpi（dot per inch）或者 ppi（pixels per inch），两个单位所针对的领域不一样。dpi 一般用于打印机，描述“每英寸墨量”；ppi 用于显示器，一个像素一个格子，就是每英寸显示的物理像素数量。

比如 iPhone 5 拥有 4 inch 的屏幕，即对角线长度为 4 inch，计算方式为 (1136^2 + 640^2)^0.5/4，结果取整之后就是 326，这就是该设备的物理像素密度。

## CSS 像素

是 CSS、JS 中的一个抽象的概念，用于控制页面元素的大小，单位是 px。

## 设备独立像素

density-independent pixel，从英文名字定义就可以看出来，是与密度无关的像素，可以认为是计算机坐标系统中的一个点，这个点表示一个可以由计算机程序使用的一个虚拟像素（比如 css 像素）。

一个 CSS 像素对应多少个物理像素，取决于两个条件：

+ 页面是否缩放
+ 屏幕是否是高密度

在普通屏幕下，一个 css 像素对应一个物理像素。在 retina 屏幕下，一个 css 像素对应 4 个物理像素。

---

## 参考文献

+ [pt和px区别 pt是逻辑像素，px是物理像素](https://www.bbsmax.com/A/o75Nn9MP5W/)

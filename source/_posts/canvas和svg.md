---
title: canvas和svg
date: 2020-09-17 21:04:18
tags:
---

### Canvas
- 通过Javascript来绘制2D图形。
- 是逐像素进行渲染的。
- 其位置发生改变，会重新进行绘制。

### svg
- 一种使用XML描述的2D图形的语言
- SVG基于XML意味着，SVG DOM中的每个元素都是可用的，可以为某个元素附加Javascript事件处理器。
- 在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

### 两者比较
#### canvas
- 依赖分辨率
- 不支持事件处理器
- 弱的文本渲染能力
- 能够以 .png 或 .jpg 格式保存结果图像
- 最适合图像密集型的游戏，其中的许多对象会被频繁重绘

#### svg
- 不依赖分辨率
- 支持事件处理器
- 最适合带有大型渲染区域的应用程序（比如谷歌地图）
- 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
- 不适合游戏应用


### 如何在同一个canvas 下画不同的形状和形状和颜色
 ```javascript
   // 解决方法， 给画图形的地方加入 开始和关闭的方法
   
   var canvas = document.getElementById('canvas')
   var ctx = canvas.getContext('2d')
   // 圆形
   ctx.beginPath()
   ...
   ctx.closePath()
   
   // 正方形
   ctx.beginPath()
   ...
   ctx.closePath()
   
   
   // 长方形
   ctx.beginPath()
   ...
   ctx.closePath()
   ```

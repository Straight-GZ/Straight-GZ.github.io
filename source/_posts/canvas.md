---
title: canvas
tags:
  - HTML
---

## canvas

`<canvas>`元素可被用来通过 JS 绘制图形动画

 <!-- more -->

## 使用

1. canvas 标签 （自带宽高属性）

   ```
   <canvas id='canvas' width:"100" height:"100"></canvas>
   ```

2. JS

   创建 canvas 画布

   ```
   let canvas = document.getElementById('canvas');
   let ctx = canvas.getContext('2d');
   ```

3. 常用属性

- 绘制矩形:

  - 填充：fillRect(x,y,width,height)
  - 边框：strokeRect
  - 清除指定矩形区域 `clearRect(x, y, width, height)`

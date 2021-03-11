---
title: BFC是什么？
tags:
  - CSS
---

### 1、BFC 概念

- 块格式化上下文（Block Formatting Context，BFC），是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。
- 具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。
<!-- more -->

### 2、触发 BFC

常见触发 BFC 的方式

- 根元素（html）
- 浮动元素（元素的 float 不是 none）
- 绝对定位元素（元素的 position 为 absolute 或 fixed）
- 行内块元素（元素的 display 为 inline-block）
- overflow 计算值(Computed)不为 visible 的块元素
- 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
- 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）

### 4、BFC 的特性

1. 在 BFC 中，盒子从顶端开始垂直地一个接一个地排列.
2. 盒子垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻盒子的 margin 会发生重叠

BFC 是一个独立的渲染区域，它规定了内部如何布局，与外部互不干涉

### 5、BFC 的应用

1. 避免外边距重叠

   如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。

2. 清楚浮动
   浮动的元素会脱离普通文档流

   ```
   <style>
    .test1{
        border:1px solid red;
    }
    .test2{
        float:left;
        width:100px;
        height:100px;
        border:1px solid black;
    }
   <style>
   <div class=test1>
        <div class=test2></div>
   </div>
   ```

   ![image-20210309124909376](https://i.loli.net/2021/03/09/ILwB5DpFQ8RzmCr.png)

   这时候只要触发容器`test1`的 BFC 就可以清除浮动
   ![image-20210309125307615](https://i.loli.net/2021/03/09/v3rmNdEZKQfALyi.png)

3. BFC 可以阻止元素被浮动元素覆盖

   ```
   <style>
   .test1{
        width:100px;
        height:100px;
        border:1px solid red;
        float:left;
    }
    .test2{
    border:1px solid black;
    }
   </style>
   <div class=test1></div>
   <div class=test2>文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字</div>
   ```

   ![image-20210309125831884](https://i.loli.net/2021/03/09/C7VMyNGY8c5eRhp.png)
   此时，只需要触发`test2`的 BFC 就可以解决

   这种方法也可以实现一侧固定宽度，一侧自适应的两栏布局。

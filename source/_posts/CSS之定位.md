---
title: CSS定位
tags:
  - CSS
  - 布局
---

布局是屏幕平面上的，**定位是垂直于屏幕的**

## 1、盒模型：

- 背景的范围：border**外边沿**围城的区域（包括 border）
- 从左边看 div：background 在文字后面
<!-- more -->

## 2、div 的分层：![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ce8ce3604b684f15a9c8dd9638198a4b~tplv-k3u1fbpfcp-watermark.webp)

浮动元素脱离文档流：浮起来一点点

## 3、position 属性

- static：默认值，待在文档流里

- relative：相对定位，升起来，但不脱离文档流

  - 使用场景：

    1.用于做位移（很少用） 2.用于给 absolute 做爸爸

  - 配合 z-index

    z-index： auto 默认值，不创建新层叠上下文 可以取 0 正负整数

- absolute：绝对定位，定位基准是祖先里的非 static

  - 使用场景

    1、脱离原来的位置，另起一层（对话框里的关闭按钮）

    2、鼠标提示

  - 注意：

    1、某些浏览器如果不写 top/left 位置会错乱

    2、善用`left:100%` 善用`left:50%` 加负 margin

- fixed： 固定定位，定位基准是 viewPort （父元素有 transform）

  - 使用场景：广告、回到顶部按钮
  - 注意：手机上尽量不要使用，bug 很多。

- sticky： 粘滞定位

经验：

- 如果写了 absolute，一般都得补一个 relative
- 如果写了 absolute 或 fixed，一定要补 top 和 left；
- sticky 兼容性差

**例子**：http://js.jirengu.com/lumad/17/edit?html,css,output

## 4、z-index

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/038b80cb9d3d4f8a86fa8b07d2d81f18~tplv-k3u1fbpfcp-watermark.webp)

取值：

- auto：不会创建一个新的本地堆叠上下文。在当前堆叠上下文中生成的盒子的堆叠层级和父级盒子相同。
- 整型数字：生成的盒子在当前堆叠上下文中的堆叠层级。此盒子也会创建一个堆叠层级为 0 的本地堆叠上下文。这意味着后代（元素）的 z-indexes 不与此元素的外部元素的 z-indexes 进行对比。

层叠上下文：

- 每个层叠上下文就是一个新的小世界

- 每个小世界里的 z-index 跟外界无关

- 处在同一小世界的 z-index 才能比较

- 哪些属性可以创建它

  z-index/flex/opacity/transform [层叠上下文 MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)

**负 margin 与层叠上下文：负 z-index 逃不出小世界**

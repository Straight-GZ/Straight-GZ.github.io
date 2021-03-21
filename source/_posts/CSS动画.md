---
title: CSS动画
tags:
  - CSS
  - 动画
---

## 一、浏览器渲染原理及过程：

浏览器通过解析生成 DOM、CSSDOM，合成 render tree，然后布局（Layout）、绘制（Paint）、合成（Compose）页面。

 <!-- more -->

步骤：

- 根据 HTML 构建 HTML 树（DOM）
- 根据 CSS 构建 CSS 树（CSSOM）
- 将两棵树合并成一棵渲染树（render tree）
- Layout 布局（文档流、盒模型、计算大小和位置）
- Paint 绘制（边框颜色、文字颜色、阴影等）
- Compose 合成（根据层叠关系展示画面）

**三棵树：**
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87fa48987593403fa2840d7fc4a394af~tplv-k3u1fbpfcp-watermark.webp)

三种更新方式：

JS / CSS > 样式 > 布局 > 绘制 > 合成
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1db893095cee40779dd3cb941d30ce05~tplv-k3u1fbpfcp-watermark.webp)
如果您修改元素的“layout”属性，也就是改变了元素的几何属性（例如宽度、高度、左侧或顶部位置等），那么浏览器将必须检查所有其他元素，然后“自动重排”页面。任何受影响的部分都需要重新绘制，而且最终绘制的元素需进行合成。
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59b28c5d64ad4a5696e2dbd22ff3ad00~tplv-k3u1fbpfcp-watermark.webp)
如果您修改“paint only”属性（例如背景图片、文字颜色或阴影等），即不会影响页面布局的属性，则浏览器会跳过布局，但仍将执行绘制。
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8cbccf8baf0444ab99d8d461cbb05654~tplv-k3u1fbpfcp-watermark.webp)
如果您更改一个既不要布局也不要绘制的属性，则浏览器将跳到只执行合成。这个**最后的版本开销最小**，最适合于应用生命周期中的高压力点，例如**动画或滚动**。

一般使用 js 更新样式：

- div.style.background='red' 改变背景颜色，跳过 Layout，直接 repaint+Compose
- div.remove()删掉节点 触发当前消失，其他元素 Layout
- div.classList.add('red') 改变 transform，只需 Compose

  **不同属性触发的过程 祥见：https://csstriggers.com/**

  <br>

## 二、transform 改变形态

**旋转、旋转，缩放，倾斜或平移。**

- translate：位移 参数：长度、百分数（translate()不可用）

  `translateX（tx）` 水平移动 `translateY(ty)` 垂直移动

  `translateZ(tz)` 3D 空间的 z 轴方向移动 配合父元素 perspective() （到 z=0 平面的距离）

  `translate(tx,ty?)` `translate(tx,ty,tz)`

  **`translate(50%,50%)`可以做绝对定位元素居中**

- scale：缩放 参数：number

  `scaleX(sx)` 横坐标缩放 `scaleY(sy)` 纵坐标缩放 `scaleX(sx,sy?)`

  **容易出现模糊**

- rotate：旋转

  `rotate(a)` 旋转角度 同`rotateZ(a)` `rotateX(a)` x 轴旋转 `rotateY(a)` y 轴转动

  **用 360° 旋转 制作 loading**

- skew：倾斜

  `skewX()`沿横倾斜 `skewY()`沿纵轴倾斜 `skew(ax, ay?)`

**transform 组合使用：**

`transform: scale(0.5) translate(-100%, -100%);`

`transform: none` 取消所有 <br>
**注**：

- 一般配合**transition**过渡

- inline 元素不支持 transform，需要先变成 block

<br>

## 三、制作动画

1. transition 过渡

- 作用：补充中间帧

- 语法：

  1. transition：属性名 时长 过渡方式 延迟 `transition: left 200ms linear`

  2. 可以用逗号分隔两个属性 `transition: left 200ms,right 200ms`

  3. 可以用 all 代表所有属性 `transition all 200ms`

  4. 过渡方式：linear | ease | ease-in-out | step-start [详细点击](https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function)

- 注意：并不是所有的属性都可以过渡

  1. display：none => block 无法过渡 一般改成 visibility:hidden => visible(隐藏或显示，但不改变布局)

  2. 颜色、透明度，都可以过渡。**过渡必须有起始**

- 如果有中间点

  1. 使用两次 transform .a=transform=>.b .b=transform=>.c
  2. 使用 setTimeout 或者监听 transitioned 事件
  3. **animation**

##### **例子：[跳动的心](http://js.jirengu.com/dunuz/1/edit?html,css,output)**

<br>

2. animation 动画制作

- 声明关键帧

  **`@keyframes`**语法 两种写法

  ```css
  @keyframes slidein {
    from {
      transform: translateX(0%);
    }

    to {
      transform: translateX(100%);
    }
  }
  ```

  ```css
  @keyframes identifier {
    0% {
      top: 0;
    }
    50% {
      top: 30px;
      left: 20px;
    }
    50% {
      top: 10px;
    }
    100% {
      top: 0;
    }
  }
  ```

- 添加动画

  animation 缩写语法：

  `animation: 时长|过渡方式|延迟|次数|方向|填充模式|是否暂停|动画名`

  1. 时长：1s 或者 100ms
  2. 过渡方式：和 transition 的取值一样
  3. 次数：1 或者 2.4 infinite 无限次
  4. 方向 reverse|alternate|alternate-reverse
  5. 填充模式：none|forwards|backwards|both
  6. 是否暂停：paused|running
  7. 以上属性均有单独属性

  **我们可以在任意一个点指定关键帧，所以 animation 可以用来做更复杂的动画**

##### 例子：[跳动的心](http://js.jirengu.com/fuzib/7/edit?html,css,output)

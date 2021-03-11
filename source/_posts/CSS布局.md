---
title: 简述CSS三种布局
tags:
  - CSS
  - 布局
---

### 一、布局概述

布局就是把页面分成一块一块，按左中右、上中下等排列。

1、布局分类：

- **固定宽度布局**，一般为 960/1000/1024px
- **不固定布局**，主要靠文档流的原理来布局（文档流是自适应，不需要加额外样式）
- **响应式布局**，PC 上固定宽度，手机不固定宽度，混合布局
<!-- more -->

2、布局思路

从大到小：先定下大局，然后完善每个部分的小布局

从小到大：先完成小布局，然后组合成大布局

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d5cd5a0716644d2285941d447eb0c92d~tplv-k3u1fbpfcp-watermark.webp)

### 二、float 布局

步骤：

- 在子元素上加 float:left 和 width

- 在父元素上加.clearfix（不要忘了加）

  ```css
  .clearfix:after {
    content: "";
    display: block;
    clear: both;
  }
  ```

经验：

- 留一些空间或者最后一个不设 width，或者设置一个最大宽度
- IE6/7 存在双倍 margin bug

实践：

- float 两栏布局（顶部条）：`float:left; ` `clearfix`
- float 三栏布局（内容区）
- float 四栏布局（导航栏）
- float 平均布局（产品展示）需要在内容外再加一层，使用负 margin

### 三、flex 布局

Flexible Box 模型，通常被称为 flexbox，是一种一维的布局模型。

#### flex 的两个轴

1. 主轴

- 由`flex-direction`定义
- 取值：row(水平方向)/row-reserve/column(垂直方向)/column-reserve

2. 交叉轴

- 交叉轴垂直于主轴
- `flex-direction` 为 `row` 或者 `row-reverse` ，交叉轴的方向就是垂直方向。
- 相反亦然

#### flex 容器

1. 实现 flex

   需要将指定一个容器，将容器的`display`属性值改为 `flex` 或者 `inline-flex`，容器中的直系子元素就会变为 **flex 元素**。

```
.container {
    display: flex | inline-flex;
}
```

**当时设置 flex 布局之后，子元素的 float、clear、vertical-align 的属性将会失效。**

2. 容器的属性

- flex-direction:**决定主轴的方向(项目的排列方向)**

  ```
  .container {
      flex-direction: row | row-reverse | column | column-reverse;
  }
  ```

  默认值：row，主轴为水平方向，起点在左端，row-reverse：主轴为水平方向，起点在右端；

  ​ column：主轴为垂直方向，起点在上沿，column-reverse：主轴为垂直方向，起点在下沿

- **flex-wrap: 决定容器内项目是否可换行**

  ```
  .container {
      flex-wrap: nowrap | wrap | wrap-reverse;
  }
  ```

  默认值：**nowrap** 不换行,当空间不足时，子元素会缩小。

  ​ wrap：项目太大而无法全部显示在一行中，则会换行显示，第一行在上方

  ​ wrap-reverse：换行，第一行在下方

- **justify-content：定义了项目在主轴的对齐方式。**

  ```
  .container {
      justify-content: flex-start | flex-end | center | space-between | space-around;
  }
  ```

  默认值: flex-start 左对齐,flex-end：右对齐,center：居中

  ​ space-between：两端对齐，剩余空间等分成间隙。

  ​ space-around：每个项目两侧的间隔相等，所以项目之间的间隔比项目与边缘的间隔大一倍。

- align-items: 可以使元素在交叉轴方向对齐。

  ```
  .container {
      align-items: flex-start | flex-end | center | baseline | stretch;
  }
  ```

  默认值为 `stretch` 即如果项目未设置高度或者设为 auto，将占满整个容器的高度。

  `flex-start`，使 flex 元素按 flex 容器的顶部对齐, `flex-end` 使它们按 flex 容器的下部对齐, 或者`center`使它们居中对齐.

三、Flex 项目属性：

1. order

   ```css
   .item {
     order: <integer>;
   }
   ```

   **定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0**

2. flex-grow: **定义项目的放大比例**

   ```css
   .item {
     flex-grow: <number>;
   }
   ```

   默认值为 0，即如果存在剩余空间，也不放大

   如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间。(如果有的话)

   如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍。

3. flex-shrink:**定义了项目的缩小比例**

   ```css
   .item {
     flex-shrink: <number>;
   }
   ```

4. flex: **flex-grow, flex-shrink 和 flex-basis 的简写**

   当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%

   ```
   .item {flex: 1;}   //等价的
   .item {
       flex-grow: 1;
       flex-shrink: 1;
       flex-basis: 0%;
   }
   ```

   - 当 flex 取值为 0 时，对应的三个值分别为 0 1 0%
   - 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，有如下等同情况（注意 0% 是一个百分比而不是一个非负数字）
   - 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%
   - 当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1

5. align-self：**允许单个项目有与其他项目不一样的对齐方式**

   单个项目覆盖 align-items 定义的属性

   ```css
   .item {
     align-self: auto | flex-start | flex-end | center | baseline | stretch;
   }
   ```

### 四、Grid 布局

1. 成为 container

```css
.container {
  display: grid;
}
```

2.  行和列

```css
.container {
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f8b97e7942049fea47f4bb92abc0f8b~tplv-k3u1fbpfcp-watermark.webp)

3. 给每条线取名字：

```css
.container {
  grid-template-columns:
    [first] 40px [line2] 50px [line3] auto
    [col4-start] 50px [five] 40px [end];
  grid-template-rows:
    [row1-start] 25% [row1-end] 100px [third-line]
    auto [last-line];
}
```

4. items 可以设置范围

```css
.item-a {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start;
  grid-row-end: 3;
}
```

5. fr（free space） 份 可以和像素混用

```css
.container {
  grid-template-columns: 1fr 50px 1fr 1fr;
}
```

- 分区 grid-template-areas

```css
grid-template-areas:
  "head head"
  "nav  main"
  "nav  foot"; /* 区域划分 当前为 三行 两列 */

#page > header {
  grid-area: head; /*  指定当前元素所在的区域位置, 从grid-template-areas选取值 */
  background-color: #8ca0ff;
}
```

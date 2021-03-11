---
title: jQuery
tags:
  - DOM
  - 库
---

jQuery 是一个轻量级的 JavaScript 函数库。
它的基本设计思想和主要用法，就是"**选择某个网页元素，然后对其进行某种操作**"。
<br>

### jQuery 如何获取元素

使用 jQuery 的第一步，往往就是将一个选择表达式，放进构造函数 jQuery()（简写为$），然后得到被选中的元素
<br>
**选择表达式**：

```
$(document) //选择整个文档对象
$('#myId') //选择ID为myId的网页元素
$('div.myClass') // 选择class为myClass的div元素
$('a:first') //选择网页中第一个a元素
$('tr:odd') //选择表格的奇数行
$('#myForm :input') // 选择表单中的input元素
```

对结果进行筛选

```
$('div').has('p'); // 选择包含p元素的div元素
$('div').not('.myClass'); //选择class不等于myClass的div元素
$('div').filter('.myClass'); //选择class等于myClass的div元素
```

### jQuery 的链式操作

选中网页元素以后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来

```
$('div').find('h3').eq(2).html('Hello')
```

原理就是，每一步的 jQuery 操作，返回的都是一个 jQuery 对象
<br>
jQuery 提供 end()方法,返回上一次操作的结果

### jQuery 如何创建元素

创建新元素只需要把新元素直接传入 jQuery 的构造函数

```
$('<p>Hello</p>');
$('<li class="new">new list item</li>');
$('ul').append('<li>list item</li>');
```

### jQuery 移动元素

一个方法是直接移动该元素，另一个方法是移动其他元素，使得目标元素达到我们想要的位置。

```
$('div').insertAfter($('p'));  //把div元素移动p元素后面,返回div元素
$('p').after($('div'));  //把p元素加到div元素前面，返回p元素
```

### jQuery 修改元素属性

jQuery attr() 方法用于获取或修改元素属性值

```
$("#id").attr("href","http://www.xxx.com");
$("#runoob").attr({
     "href" : "www.xxx.com",
     "title" : "jQuery"
}); //同时设置多个属性
```

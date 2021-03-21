---
title: MVC
tags:
  - MVC
---

#### 一、MVC

MVC 包括三类对象:

1. 模型 model 用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法，会有一个或多个视图监听此模型。一旦模型的数据发生变化，模型将通知有关的视图。
<!-- more -->

```
const m = {
  data: {存储数据},
  create() {添加数据},
  delete() {删除数据},
  update(data) {     	更新数据
    eventBus.trigger()	触发事件
  },
  get() {读取数据}
}
```

1. 视图 view 是它在屏幕上的表示，描绘的是 model 的当前状态。当模型的数据发生变化，视图相应地得到刷新自己的机会。

```
const v = {
  el: 渲染标记,
  html: `页面内容`,
  init(){初始化页面},
  render(){渲染页面}
}
```

3. 控制器 controller 定义用户界面对用户输入的响应方式，起到不同层面间的组织作用，用于控制应用程序的流程，它处理用户的行为和数据 model 上的改变。

```
const c = {
  init() {
    v.init(container)   		初始化view
    v.render() 				第一次渲染
    c.autoBindEvents()			自动绑定事件
    eventBus.on(){}			监听事件
    },
  events: {哈希表记录事件},
  autoBindEvents() {自动绑定事件}
}
```

#### 二、EventBus

EventBus 事件总线,可以用来进行组件之间的监听和通信。

- 初始化

```
import $ from 'jquery'
const eventBus = $(window)
```

- api:`eventBus.trigger()`:触发事件；`eventBus.on(){}`：监听事件;`eventBus.off(){}`:解除事件

```
const m = {
	...
   update(data) {
   ...
    eventBus.trigger('m:updated')		//在更新数据时触发事件
  },
const c = {
  	...
    eventBus.on('m:updated', () => {v.render(m.data.n)})
  },		//接收事件，事件触发就执行后面的函数
  const

  eventBus.off('m:updated')		//移除事件
```

#### 三、表驱动编程

简单来说，就是以查表的方式获取数据而不使用逻辑语句。事实上，凡是能通过逻辑语句来选择的事物，都可以通过查表来选择。
从表中查询数据主要有直接访问、索引访问、阶梯访问三种方式。

例：今天星期几

```
const day = new Date().getDay()
let day_zh;
if(day === 0){
    day_zh = '星期日'
}else if(day === 1) {
    day_zh = '星期一'
}
...
else{
    day_zh = '星期六'
}

// 或者 用 switch case
switch(day) {
    case 0:
        day_zh = '星期日'
        break;
    case 1:
        day_zh = '星期一'
        break;
        ...
}
```

使用表驱动法，将数据放在一个表里

```
const week = ['星期日', '星期一',..., '星期六']
const day = new Date().getDay(
const day_zh = week[day]
```

#### 四、如何理解模块化的

模块化就是将一个复杂的程序，通过一定规则封装成不同的模块，可以是对象、文件等。降低代码耦合度，各个模块有更好的独立性。这样做可以解决项目中的全局变量污染的问题，使开发效率更高，方便代码复用和维护 。

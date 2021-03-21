---
title: computed和watch的区别
tags:
  - 框架
  - Vue
---

### 1、computed

- computed 虽然看上去是方法，但实际上是计算属性，因此，使用方法和属性的使用方法相同，不需要加括号。
- computed 会根据依赖的数据动态显示，并且计算结果会被缓存，只有在依赖的数据发生变化的之后再次获取 computed 的值才会重新计算。

 <!-- more -->

**示例**

```javascript
new Vue({
  data: {
    users: {
      id: 1,
      name: "张三",
      phone: "123456",
    },
  },
  computed: {
    _user: {
      get() {
        return this.users.name + ":" + this.users.phone;
      },
      set(value) {
        this.users.name = value;
      },
    },
  },
  template: `
  <div>
    {{_user}}
    <button @click="name='李四'">setName</button>
  </div>
  `,
}).$mount("#app");
```

**注意：**

> 上述例子可以直接在模板字符串中写{{this.users.name + ":" + this.users.phone}} ，模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。

### 2、watch

- watch 是对数据 data 的监听回调，当所依赖的 data 变化时，执行回调，是异步的,会传入`newVal`和`oldVal`.
- 提供两个选项，`immediate`用来设置是否在第一次渲染的时候执行，`deep`用来设置要监听对象内部属性的变化
  **示例**

```javascript
new Vue({
  data() {
    return {
      n: 0,
      history: [],
      isUndo: false,
    };
  },
  methods: {
    add() {
      this.n += 1;
    },
    undo() {
      const last = this.history.pop();
      this.isUndo = true;
      this.n = last.from; //触发watch异步更新
      this.$nextTick(() => {
        //需要在上一步更新之后 再设置    setTimeout
        this.isUndo = false;
      }, 0);
    },
  },
  watch: {
    n(newN, oldN) {
      //当 this.isUndo为false 才会执行
      if (!this.isUndo) this.history.push({ from: oldN, to: newN });
    },
  },
  template: `<div>
  {{n}}
  <hr>
 <button @click='add'>+1</button>
 <button @click='undo'>撤销</button>
 <hr>
{{history}}
</div>`,
}).$mount("#app");
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b43acb00de3a401cb8714c3c978ef48b~tplv-k3u1fbpfcp-watermark.image)

**计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。**

---
title: Vuex的基本用法
tags:
  - 框架
  - Vue
---

##### 1、Vuex 是什么？

一个专为 Vue.js 应用程序开发的状态管理工具

##### 2、作用

- 解耦：将所有数据相关的逻辑放入 store（也就是 MVC 中的 Model，换了个名字而已）
- 数据读写更方便：任何组件不管在哪里，都可以直接读写数据
- 控制力更强：组件对数据的读写只能使用 store 提供的 API 进行
<!-- more -->

##### 3、核心概念

Vuex 的核心就是仓库 store，这个 store 实例会被注入到所有子组件里面,子组件通过`this.$store`获取

主要架构

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {

  },
  mutations: {

  },
  actions: {

  }
})
```

- state

  state 属性保存着全局的状态

  ```
  state: {
    count: 10   //定义状态
  },
  ```

  ```
  //子组件通过计算属性获取
  computed: {
    count() {
      return this.$store.state.count;
    }
  }
  ```

  mapState：辅助函数

  ```
  import { mapState } from 'vuex'
  export default {
  // ...
  computed: mapState({
      count: state => state.count,
      // 传字符串参数 'count' 等同于 `state => state.count`
      countAlias: 'count',
      // 为了能够使用 `this` 获取局部状态，必须使用常规函数
      countPlusLocalState (state) {
        return state.count + this.localCount
      }
    })
  }
  ```

  对象三点操作符

  ```
    computed: {
      msg() {
        return this.$store.state.msg;
      },
    ...mapState(['count', 'firstName', 'lastName'])
  },
  ```

- Getter

  对状态进行处理的提取出来的公共部分,可以理解为 state 的计算属性，结果会缓存，且只有当它的依赖值发生了改变才会被重新计算

  ```
  export default new Vuex.Store({
  state: {
    list: [1, 2, 3, 4, 5]
  },
  getters: {
    modifyArr(state) {
      return state.list.filter((item, index, arr) => {
        return item % 2 == 0;
      })
    },
    getLength(state, getter) { // 方法里面传getter，调用modifyArr来计算长度
      return getter.modifyArr.length;
    }
  });
  ```

  mapGetters:辅助函数,将 store 中的 getter 映射到局部计算属性

  ```
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'modifyArr',
      'getLength',
    ])
  }
  //改名
    computed: {
    ...mapGetters([
      x:'modifyArr',
      y:'getLength',
    ])
  }
  ```

- Mutation

  - 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

    ```
    mutations: {
      increment (state) {
        // 变更状态
        state.count++
      }
    }
    store.commit('increment')
    ```

    ```
    //提交额外参数
    mutations: {
      increment (state,n) {
        // 变更状态
        state.count+= n
      }
    }
    store.commit('increment',1)
    ```

  * 对象风格的提交

    ```
    store.commit({
      type: 'increment',
      n: 10
      })
    ```

    ```
      mutations: {
        increment (state, obj) {
          state.count += obj.n
        }
      }
    ```

    mapMutations:辅助函数

    官方代码

    ```
    import {mapMutations} from 'vuex';
    export default {
    // ...
    methods: {
      ...mapMutations([
        'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

        // `mapMutations` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
      ]),
      ...mapMutations({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
      })
    }
    }
    ```

- Action

  Action 类似于 mutation，不同在于：

  Action 提交的是 mutation，而不是直接变更状态；Action 可以包含任意异步操作。

  ```
     actions: {
    increment (context) { //context是和store 实例具有相同方法和属性的对象
      context.commit('increment')
    }
  }
  ```

  你在组件中使用 this.$store.dispatch('xxx') 分发 action，或者使用 mapActions 辅助函数将组件的 methods 映射为 store.dispatch 调用（需要先在根节点注入 store）

  [文档](https://vuex.vuejs.org/zh/guide/actions.html)

- Module

  ```
    const moduleA = {
      state: () => ({ ... }),
      mutations: { ... },
      actions: { ... },
      getters: { ... }
    }
    const moduleB = {
      state: () => ({ ... }),
      mutations: { ... },
      actions: { ... }
    }
    const store = new Vuex.Store({
      modules: {
        a: moduleA,
        b: moduleB
      }
    })
    store.state.a // -> moduleA 的状态
    store.state.b // -> moduleB 的状态
  ```

[官方文档](https://vuex.vuejs.org/zh/guide/modules.html)

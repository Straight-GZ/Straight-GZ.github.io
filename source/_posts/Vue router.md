---
title: Vue Router
tags:
  - 框架
  - Vue
---

##### 1、Vue Router

Vue.js 官方的路由管理器。路由就是 SPA（single page application 单页应用）的路径管理器，单页面应用的思路是一个 index.html 里面有一个容器，在用户进行交互的时候，并不会重新加载某个界面，而是只选择把某个视图更新到容器中去显示。

##### 2、路由模式

- hash 模式

  vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，当 URL 改变时，页面不会重新加载。# 是 url 的一个锚点，记载了网页中的位置，Hash 模式通过锚点值的改变，根据不同的值，渲染指定 DOM 位置的不同数据。
  hash 模式的原理是 onhashchange 事件(监测 hash 值变化)，可以在 window 对象上监听这个事件。

* history 模式

  当你使用 history 模式时，URL 就像正常的 url，需要后台配置支持

##### 3、页面跳转

- 声明式：以标签的形式声明为一个跳转的标签

  ```
  //简单用法
  <router-link to="/">Home</router-link>
  //传递参数
  <router-link :to="{name:xxx,params:{key:value}}"></router-link>
  //url 传值（index.js设置路由）
  {
    path:'/user/:id/', //id就是传递的参数
  }
  //获取参数
  $route.params.key

  //使用name匹配路由，使用query来传递参数
  <router-link :to="{ name:'xxx',query: { key:value}}" >跳转Query</router-link>
  //获取参数
  this.$route.query.queryId
  ```

  **param 和 query 的区别**

  - 使用 params 传参只能用 name 来引入路由，即 push 里面只能是 name:'xxx',不能是 path:'/xxx',因为 params 只能用 name 来引入路由，如果这里写成了 path，接收参数页面会是 undefined。
  - query name 和 path 都支持。query，url 会加上参数信息，而 param url 并不会携带参数信息。

* 编程式

  ```
  // 不带参数
  this.$router.push({name:'xxx'})
  // 带参数 params
  this.$router.push({name:'xxx',params:{key:value}})
  //query
  this.$router.push({name:'xxx',query:{key:value}})

  this.$router.go(-1)//跳转到上一次浏览的页面
  this.$router.replace('/menu')//指定跳转的地址
  this.$router.replace({name:'menuLink'})//指定跳转路由的名字下
  ```

  **push 和 replace 的区别：**

  - 使用 push 方法的跳转会向 history 栈添加一个新的记录，当我们点击浏览器的返回按钮时可以看到之前的页面。也就是说是支持后退的。

  * 使用 replace 方法不会向 history 添加新记录，而是替换掉当前的 history 记录，即当 replace 跳转到的网页后，'后退' 按钮不能查看之前的页面。

##### 4、导航守卫

1. 全局前置守卫

   ```
   const router = new VueRouter({ ... })
   router.beforeEach((to, from, next) => {
   ...
   })
   ```

参数

- to 即将要进入的目标 路由对象
- from 当前导航正要离开的路由
- next 要执行的操作

  - next(): 进行管道中的下一个钩子（放行）。
  - next(false): 中断当前的导航。
  - next('/') 或者 next({ path: '/' }):跳转到一个不同的地址。
  - next(error): 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。

2.  全局解析守卫

    router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用

3.  全局后置钩子

    你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 next 函数也不会改变导航本身：

    ```
    router.afterEach((to, from) => {
    // ...
    })
    ```

4.  路由独享的守卫
    在路由配置上直接定义 beforeEnter 守卫：
    与全局前置守卫的方法参数是一样的。
    ```
    const router = new VueRouter({
      routes: [
        {
          path: '/foo',
          component: Foo,
          beforeEnter: (to, from, next) => {
            // ...
          }
        }
      ]
    })
    ```

- 组件内的守卫

  - beforeRouteEnter

  * beforeRouteUpdate
  * beforeRouteLeave

  ```
    const Foo = {
    template: `...`,
    beforeRouteEnter (to, from, next) {
      // 在渲染该组件的对应路由被 confirm 前调用
      // 不！能！获取组件实例 `this`
      // 因为当守卫执行前，组件实例还没被创建
    },
    beforeRouteUpdate (to, from, next) {
      // 在当前路由改变，但是该组件被复用时调用
      // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
      // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
      // 可以访问组件实例 `this`
    },
    beforeRouteLeave (to, from, next) {
      // 导航离开该组件的对应路由时调用
      // 可以访问组件实例 `this`
    }
  }
  ```

  beforeRouteEnter 守卫 不能 访问 this,可以通过传一个回调给 next 来访问组件实例。

  ```
  beforeRouteEnter (to, from, next) {
    next(vm => {
    // 通过 `vm` 访问组件实例
  })
  }
  ```

  完整的导航解析流程

  [官方文档](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB)

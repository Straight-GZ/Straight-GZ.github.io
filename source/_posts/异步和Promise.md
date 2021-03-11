---
title: 异步与Promise
tags:
  - JavaScript
---

### 一、异步与同步

同步:一定要当前任务执行完，才会执行下一个任务。

```
  const fn1=()=>{
    return '结果'
  }
  const fn2=()=>{}
  let x=fn1()  //拿到fn1的返回值之后才会执行下一句
  fn2()
```

异步：不等任务执行完，就执行下一个任务

```
function fn1 = function(callback){
  const result = setTimeout(function(){
    callback('结果')
  }, 0)
  return result
}
fn1(function callback(x){
  console.log(x)  //别的代码执行完，再回来执行
})
fn2() //先执行这一句
```

### 二、Promise

Promise 是异步编程的一种方案。从语法上讲，Promise 是一个对象，它可以获取异步操作的消息。

#### 1、 创建 Promise

```
const promise = new Promise((resolve, reject) => {
  if(){
    resolve(someValue)
    // fulfilled 已兑现: 意味着操作成功完成。
  } else{
    rejected()
    //reject("failure reason") 已拒绝: 意味着操作失败
  }
});
```

- 构造函数接受一个函数作为参数

* 调用构造函数得到实例 promise 的同时，作为参数的函数会立即执行
* 参数函数接受两个回调函数参数 resolve 和 reject

- 在参数函数被执行的过程中，如果在其内部调用 resolve，会将 promise 的状态变成 fulfilled，或者调用 reject，会将 promise 的状态变成 rejected

```
promise.then(()=>{
  //  resolve，执行这里
},()=>{
  // reject 执行这里
})
```

待定状态（pending）的 Promise 对象要么会通过一个值被兑现（fulfilled），要么会通过一个原因（错误）被拒绝（rejected）。当这些情况之一发生时，我们用 promise 的 then 方法排列起来的相关处理程序就会被调用。

#### 2、Promise 的作用

- 解决回调地狱的问题（避免了层层嵌套的回调函数）
- 语法非常简洁。Promise 对象提供了简洁的 API，使得控制异步操作更加容易

#### 3、Promise 的使用

- new Promise：创建 Promise 实例
- .then 和 .catch 和 .finally
  - `.then`:返回一个 Promise。它最多需要有两个参数：Promise 的成功和失败情况的回调函数。
  - 返回一个 Promise，并且处理拒绝的情况。(相当于只有一个参数的.then)
  - finally() 方法返回一个 Promise。在 promise 结束时，无论结果是 fulfilled 或者是 rejected，都会执行指定的回调函数。这为在 Promise 是否成功完成后都需要执行的代码提供了一种方式。
    这避免了同样的语句需要在 then()和 catch()中各写一次的情况。

* Promise.all/Promise.race/Promise.reject/Promise.resolve

  - Promise.all() 方法接收一个 promise 的数组，并且只返回一个 Promise 实例，它的 resolve 回调会在所有的 Promise 的 resolve 都执行结束之后，才会调用，它的 reject 回调只要有任何一个 Promise 的 reject 被执行，就会被调用
  - Promise.race(),接受 Promise 数组，返回一个 promise，一旦某个 promise 解决或拒绝，返回的 promise 就会解决或拒绝。
  - Promise.reject(reason)返回一个带有拒绝原因的 Promise 对象。
  - Promise.resolve(value) 方法返回一个以给定值解析后的 Promise 对象,value 可以是带有.then 方法的对象，也可以是 Promise(将直接返回这个 Promise)

        ```
        const promise1=Promise.resolve(1)
        const promise2=Promise.resolve(promise1)
        promise1===promise2 //true

        ```

    接受带有.then 方法的对象

    ```
    //抛出错误之后resolve
    let x = { then: function(resolve) {
      throw new Error("错误");
      resolve("Resolving");
    }};
    let y = Promise.resolve(x);
    y.then(function(v) {
        //不会执行
    }, function(e) {
      console.log(e);   //打出错误
    });
    ```

    ```
    //抛出错误之前 resolve
    let m = { then: function(resolve) {
      resolve("完成");
      throw new Error("错误");
    }};

    var n = Promise.resolve(m);
    n.then(function(v) {
      console.log(v); // 输出"完成"
    }, function(e) {
      // 不会被调用
    });
    ```

#### 参考链接

[阮一峰 ECMAScript 6 入门](https://es6.ruanyifeng.com/#docs/promise)

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

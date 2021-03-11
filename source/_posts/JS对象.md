---
title: JS 对象
tags:
  - JavaScript
---

### 一、常见用法

变量做属性名

除了字符串之外，变量也可以做属性名

```
  let p1 = 'name'
  let obj = { p1 : 'ss'} 这样写，属性名为 'p1'
  let obj = { [p1] : 'ss' } 这样写，属性名为 'name'
```

 <!-- more -->

查看属性值

- 中括号：`obj['key']`

- 点语法：`obj.key`

### 二、常用方法

1. Object.assign()

   ```
   const returnedTarget = Object.assign(target, source);
   ```

   将所有可枚举属性的值从一个或多个源对象分配到目标对象,回目标对象,目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。

2. Object.entries()

   ```
   const object1 = {
    a: 'somestring',
    b: 42
   };
   Object.entries(object1)
   //[["a", "somestring"],["b", 42]]
   ```

   法返回一个给定对象自身可枚举属性的键值对数组

3. Object.freeze()
   冻结一个对象。一个被冻结的对象再也不能被修改
4. Object.is()
   判断两个值是否为同一个值。
   [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is)
5. Object.keys()
   返回一个由一个给定对象的自身可枚举属性组成的数组

   ```
    Object.keys(['a', 'b', 'c'])
    //['0', '1', '2']
    Object.keys({ 0: 'a', 1: 'b', 2: 'c' })
    //['0', '1', '2']
   ```

6. Object.values()

   ```
    Object.values({ foo: 'bar', baz: 42 })
    //['bar', 42]
   ```

7. Object.create()

   ```
    Object.create({a:1,b:2});
    //__proto__:
        a: 1
        b: 2
   ```

   创建一个新对象，使用现有的对象来提供新创建的对象的**proto**

8. Object.defineProperty()

   直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

   ```
   Object.defineProperty({}, 'a',{value:1});  //{a:1}
   Object.defineProperty({}, 'property1', {
   value: 42,
   writable: false   //描述符
   });
   ```

   拥有布尔值的键 configurable、enumerable 和 writable 的默认值都是 false。
   属性值和函数的键 value、get 和 set 字段的默认值为 undefined。

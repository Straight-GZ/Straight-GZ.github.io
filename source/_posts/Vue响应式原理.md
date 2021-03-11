---
title: Vue响应式原理
tags:
  - 框架
  - Vue
---

#### 一、原理

**`Vue`会遍历`data`对象的所有`property`，并使用`Object.defineProperty`把这些`property`全部转为`getter/setter`。通过`getter/setter`让 Vue 能够追踪依赖，在`property`被访问和修改时通知变更。**

#### 二、Object.defineProperty 和 getter、setter

`Object.defineProperty`方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

```
 let data1 = {};
Object.defineProperty(data1, "n", {
  value: 0
});
console.log(data1.n);  //0
```

<br>

`getter/setter`用于对属性的读写进行监控

```
let myData2 = { n: 0 };
let data2 = proxy({ data: myData2 }); // 括号里是匿名对象，无法访问

function proxy({ data }) {
  let value = data.n;
  Object.defineProperty(data, "n", {
    get() {
      return value;
    },
    set(newValue) {
      if (newValue < 0) return;
      value = newValue;
    }
  });
  //监听 data

  const obj = {};
  Object.defineProperty(obj, "n", {
    get() {
      return data.n;
    },
    set(value) {
     if (newValue < 0) return;
      data.n = value;
    }
  });
  return obj; // obj 就是代理
}

console.log(data2.n);	//0
myData2.n = -1;
console.log(data2.n);	//0
myData2.n = 1;
console.log(data2.n);	//1
```

<br>

**当`let vm=new Vue({data:myData})`时，会让 vm 成为 Vue 的代理，对 myData 的所有属性进行监控，使得 myData 的属性变化的时候渲染页面。**

#### 三、总结

**Vue 的数据响应式，是通过`Object.defineProperty`将对象所有的属性转化为`getter/setter`来监控数据变化，即时渲染页面。**

**注意**：Vue 不能检测到对象属性的添加或删除，解决方法是手动调用 Vue.set 或者 this.$set
[文档](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%AF%B9%E4%BA%8E%E5%AF%B9%E8%B1%A1)

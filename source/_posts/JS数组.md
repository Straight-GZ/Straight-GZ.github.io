---
title: JS 数组
tags:
  - JavaScript
---

#### 1、数组对象:

- 元素的数据类型可以不同
- 内存不一定是连续的（对象随机存储）
- 不能通过数字下标，而是字符串下标
`let arr= [1,2,3] arr['xxx']=1`
<!-- more -->

#### 2、创建数组

- 新建数组

  ```
  let arr=[1,2,3] 简写形式

  let arr=new Array(1,2,3) 标准形式

  let arr=new Array(3) 一个参数表示数组长度，多个参数表示数字内容
  ```

- 转化：
  ```
  let arr='1,2,3'.split(',') 以逗号分开创建
  let arr='123'.split('') 以空字符串分开创建
  Array.from('123') 很多时候需要有数字下标，和 length
  ```
- 合并数组：

  ```
  arr1.concat(arr2)
  截取数组的一部分：

  arr1.slice(1) 从第 2 个元素开始
  arr1.slice(0) 全部截取（复制数组）
  ```

- 伪数组：

  没有数组共同属性的数组

  伪数组的原型链中并没有数组的原型，原型直接指向对象的原型

#### 3、删除数组元素

- 和对象一样

  ```
  let arr=['1','2','3']
  delete arr[0]
  arr=>[empty,'2','3']
  ```

  数组长度不会改变（稀疏数组）

* arr.shift() //arr 被修改，并返回被删除元素
* arr.pop()//arr 被修改，并返回被删除元素
* arr.splice(start,1) //从 start 位置开始，删除 1 个元素
* arr.splice(start,1,'x')//在删除位置添加'x'
* arr.splice(start,1,'x','y')//在删除位置添加'x','y'

#### 4、查看所有元素

- 查看所有属性名：

  ```
    let arr=[1,2,3,4,5];arr.x=x
    //为数组添加 x 下标,值为'x'
  ```

- Object,keys(arr1) 查看属性名
  查看数字（字符串）属性名和值
- for 循环

  ```
  for(let i=0;i<arr.length;i++){
  console.log(`${i}:${arr[i]}`)
  }
  arr.forEach()

  arr3.forEach(function(iterm,index){
  console.log(`${index}:${iterm}`)
  })
  ```

  原理:

  ```
  function forEach(array,fn){
      for(let i=0;i<array.length;i++){
          fn(array[i],i,array)
      }
  }

  ```

- for 循环与 forEach 的区别

  for 循环可以随时停止，forEach 不可以
  for 块级作用域，forEach 是函数

- 查看单个属性
  跟对象一样
  `let arr=[1,2,3] arr[0]`
- 索引越界
  ```
  arr[arr.length]===undefined
  arr[-1]===uddefined
  ```
  例：
  ```
  for(let i = 0; i<= arr.length; i++){
  console.log(arr[i].toString())
  }//报错 Cannot read property 'toString' of undefined
  ```
- 查找某个元素是否在数组里

      arr.indexof(item)//存在返回索引，否则返回-1

  使用条件查找元素

      arr.find(x=>x%2===0)//找到第一个偶数

  条件查找元素索引

      arr.findIndex(x=>x%2===0)//找到第一个偶数索引

#### 5、增加数组中的元素

- 对象的方法

  ```
  let arr=[1,2,3,4,5]
  arr[5]=6 //可以修改成功，但不推荐，数组下标如果写大了，会出问题
  ```

- 在尾部增加元素

  ```
  arr.push(item1,item2...)//修改 arr，返回新长度
  ```

- 在头部增加元素

  ```
  arr.unshift(item1,item2...)//修改 arr，返回新长度
  ```

- 在中间添加元素

  ```
  arr.splice(start,0,'x','y'...)
  ```

#### 6、修改数组中的元素

- 反转顺序

      arr.reverse()//修改原数组

  例：将字符串反转'12345'

      ```
      '12345'.split('').reverse().join('') // 54321
      ```

- 自定义顺序

  ```
  arr.sort((a,b)=>a-b)//函数返回值小于 0，即 a-b<0,a 排在 b 之前。
  ```

- 按照某个属性排序

  ```
  let items = [
      { name: 'Edward', value: 21 },
      { name: 'Sharpe', value: 37 },
      { name: 'And', value: 45 },
      { name: 'The', value: -12 },
      { name: 'Zeros', value: 37 }
  ]
  items.sort((a,b)=>a.value-b.value) //以 value 值排序
  ```

- map

  创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值

  ```
  array.map(x => x \* x) //每个元素的平方
  ```

- filter

  创建一个新数组, 其包含通过所提供函数实现的测试的所有元素 true 为通过，false 不通过

  ```
  arrary.filter(x=>x%2===1)//除以 2 余 1 就保留
  ```

- reduce（重点）

  对数组中的每个元素执行一个由您提供的函数 Arrayarray，将其结果汇总为单个返回值。

  ```
  array.reduce((sum,x)=>sum+x,0)//计算数组铬元素之和
  ```

  ```
  arr.reduce((x,y)=>{
      if(y%2===0){
          return x.concat(y)
      }else{
          return x
      }
  },[]) //把 arr 中所有偶数汇总为一个新的数组
  可以改写为
  ```

  ```
  arr.reduce((x,y)=>{
      return x.concat(y%2===0?y:[])
  },[])
  ```

- **例子**:数据变换

  ```
  let arr = [
  { 名称: "动物 ", id: 1, parent: null },
  { 名称: "狗", id: 2, parent: 1 },
  { 名称: "猫", id: 3, parent: 1 },
  ]
  ```

- 将数组变成对象

  ```
  {
  id: 1, : 名称'动物 ', children: [
      {id: 2, 名称: '狗 ', children: null},
      {id: 3, 名称: ' 猫', children: null},
  ] }
  //``代码如下：
  arr.reduce(
  (result, item) => {
      if (item.parent === null) {
      result.id = item.id
      result["名称"] = item["名称"]
      } else {
      result.children.push(item)
      delete item.parent
      item.children = null
      }
      return result
  },
  { id: null, children: [] } //初始化id、children
  )
  ```

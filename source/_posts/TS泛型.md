---
title: TS的泛型
tags:
  - Typescript
---

##### 1、泛型有什么用？

先看一个函数

```
function fn(x: string): string {
    return x
}
console.log(fn(1)) // 1

```

如果这样写，函数 fn 只能接受 string 类型，如果想要传入别的类型，返回别的类型呢？这样需要使用泛型

   <!-- more -->

```
function fn<T>(x:T):T{
    return x
}
console.log(fn(true)) // true
console.log(fn(1))   // 1
console.log(fn('1')) // '1'
console.log(fn([1, 2, 3])) // [1, 2, 3]
```

T 帮助我们捕获用户传入的类型（比如：number），我们就可以使用这个类型,之后我们再次使用了 T 当做返回值类型。这个函数 fn 就叫做泛型

- 使用

  - 传入参数、类型

  ```
  let n = identity<string>("n");
  ```

  - 类型推论

  ```
  let n = identity("n");
  ```

##### 2、泛型变量

```
function fn<T>(x: T[]): T[] {
  console.log(x.length);
  return x;
}
```

把泛型变量 T 当做类型的一部分使用，而不是整个类型，增加了灵活性。

##### 3、泛型接口

```
interface Fn<T> {
   (arg: T): T;
}

function fn<T>(x: T): T {
    return x;
}

let func: Fn<number> = fn;
```

##### 4、泛型类

```
  class X<T> {
    value: T;
    add: (x: T, y: T) => T;
  }

let x = new X<number>();
x.value = 0;
x.add = function(x, y) { return x + y; };
```

与接口一样，直接把泛型类型放在类后面，可以帮助我们确认类的所有属性都在使用相同的类型。

##### 5、泛型约束

```
interface Lengthwise {
    length: number;
}
function fn<T extends Lengthwise>(n: T): T {
    console.log(n.length);
    return n;
}
```

现在这个泛型函数被定义了约束，因此它不再是适用于任意类型。

```
fn(1)   //error，必须带有length属性
```

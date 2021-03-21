---
title: JS继承
tags:
  - JavaScript
---

### 1、原型继承

 <!-- more -->

```
function Animal(x){
	this.type=x
}
Animal.prototype.run=function(){
	console.log(this.type+"正在跑")
}
function Dog(x,name){
	Animal.call(this,x) //调用Animal的属性
    this.name=name  //自有属性
}
Dog.prototype.eat=function(){
	console.log(this.name+"正在吃")
}
Dog.prototype.__proto__=Animal.prototype
//或者
//const S=function(){}  //创建空
//S.prototype=Animal.prototype
//Dog.prototype=new S()
let dog=new Dog("狗","藏獒")
dog.name //"藏獒"
dog.type //"狗"
dog.run()//狗正在跑
dog.eat()//藏獒正在吃
```

### 2、class 继承

```
class Animal{
	constructor(x){
    	this.type=x
    }
	run(){
		console.log(this.type+"正在跑")
	}
}
class Dog extends Animal{
	constructor(x,name){
		super(x); //继承父类属性，需在使用this之前
       		this.name=name;
	}
    eat(){console.log(this.name+"正在吃")}
}
dog.name //"藏獒"
dog.type //"狗"
dog.run()//狗正在跑
dog.eat()//藏獒正在吃
```

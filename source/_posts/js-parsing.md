---
title: JavaScript深度剖析
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: DarkStrand.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-11-23 20:00:00
comments: true
tags: 
 - js
keywords: js
description: js
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---



# 1. 函数式编程

## 1.1 为什么要学习函数式编程

函数式编程是非常古老的一个概念，早于第一台计算机的诞生，[函数式编程的历史](https://zhuanlan.zhihu.com/p/24648375?refer=marisa)。
那我们为什么现在还要学函数式编程?

* 函数式编程是随着 React 的流行受到越来越多的关注
* Vue 3也开始拥抱函数式编程
* 函数式编程可以抛弃 this
* 打包过程中可以更好的利用 tree shaking 过滤无用代码 
* 方便测试、方便并行处理 
* 有很多库可以帮助我们进行函数式开发:lodash、underscore、ramda



## 1.2 函数式编程概念

函数式编程（Functional Programming，FP），FP是编程范式之一，我们常听说的编程范式还有面向过程编程、面向对象编程。

* 面向对象编程的思维方式：把现实世界中的事物抽象成程序世界中的类和对象，通过封装、继承和多态来演示事物事件的联系
* 函数式编程的思维方式：把现实世界的事物和事物之间的**联系**抽象到程序世界（对运算过程进行抽象）
  * 程序的本质：根据输入通过某种运算获得相应的输出，程序开发过程中会涉及很多有输入和输出的函数
  * x -> f（联系、映射） -> y，y=f(x)
  * **函数式编程中的函数指的不是程序中的函数（方法）**，而是数学中的函数即映射关系，例如：y=sin(x)，x和y的关系
  * **相同的输入始终要得到相同的输出**（纯函数）
  * 函数式编程用来描述数据（函数）之间的映射



```js
// 非函数式
let num1 = 2
let num2 = 3
let sum = num1 + num2 
console.log(sum)

// 函数式
function add (n1, n2) {
  return n1 + n2
}
let sum = add(2, 3)
console.log(sum)
```



## 1.3 函数是一等公民

[MDN First-class Function](https://developer.mozilla.org/zh-CN/docs/Glossary/First-class_Function)

* 函数可以存储在变量中
* 函数作为参数
* 函数作为返回值



在 JavaScript 中**函数就是一个普通的对象**（可以通过 `new Function()`），我们可以把函数存储到变量/数组中，它还可以作为另一个函数的参数和返回值，甚至我们可以在程序运行的时候通过 `new Function('alert(1)')`来构造一个新的函数。

* 把函数赋值给变量

```js
// 把函数赋值给变量
let fn = function () {
	console.log('Hello First-class Function') 
}
fn()

// 一个示例
const BlogController = {
  index (posts) { return Views.index(posts) },
  show (post) { return Views.show(post) },
  create (attrs) { return Db.create(attrs) },
  update (post, attrs) { return Db.update(post, attrs) }, 			destroy (post) { return Db.destroy(post) }
}

// 优化
const BlogController = {
  index: Views.index,
  show: Views.show,
  create: Db.create,
  update: Db.update,
  destroy: Db.destroy
}
```



* 函数是一等公民是我们后面要学习的高阶函数、柯里化等的基础。



## 1.4 高阶函数

### 1.4.1 什么是高阶函数

* 高阶函数（Higher-order function）

  * 可以把函数作为参数传递给另一个函数
  * 可以把函数作为另一个函数的返回结果

  

* 函数作为参数

```js
// forEach
function forEach (array, fn) {
	for (let i = 0; i < array.length; i++) {
    fn(array[i])
  }
}

// 测试
// let arr = [1,2,3,4]
// forEach(arr, function(item) {
//  	console.log(item)
// })


// filter
function filter (array, fn) {
  let results = []
  for (let i = 0; i < array.length; i++) {
  	if (fn(array[i])) { 
      results.push(array[i])
  	} 
  }
  return results
}
// 测试
// let arr = [1,3,4,5,7]
// let r = filter(arr, function(item) {
// 	return item % 2 === 0
// })
// console.log(r)
// [4]
```



* 函数作为返回值

```js
function makeFn () {
  let msg = 'Hello function'
  return function () {
    console.log(msg)
  }
}
const fn = makeFn()
fn()
// makeFn()()
```

```js
// once
function once (fn) {
  let done = false
  return function () {
    if (!done) {
    done = true
    return fn.apply(this, arguments)
  }
}
let pay = once(function (money) { 
  console.log(`支付:${money} RMB`)
})
// 只会支付一次 pay(5)
pay(5)
pay(5)
pay(5)
```



### 1.4.2 使用高阶函数的意义

* 抽象可以帮我们屏蔽细节，只需要关注与我们的目标
* 高阶函数是用来抽象通用的问题

```js
// 面向过程的方式
let array = [1, 2, 3, 4]
for (let i = 0; i < array.length; i++) {
	console.log(array[i]) 
}

// 高阶高阶函数
let array = [1, 2, 3, 4] 
forEach(array, item => {
  console.log(item)
})
let r = filter(array, item => { 
  return item % 2 === 0
})
```



### 1.4.3 常用的高阶函数

* forEach
* map
* filter
* every
* some
* find/findIndex
* reduce
* sort
* ...

```js
const map = (array, fn) => {
  let results = []
  for (const value of array) {
		results.push(fn(value)) 
  }
  return results
}
// 测试
let arr = [1,2,3,4]
arr = map(arr, v => v*v)
// [1,4,9,16]

const every = (array, fn) => {
  let result = true
  for (const value of array) {
    result = fn(value)
    if (!result) {
			break
		} 
  }
  return result
}
// 测试
let arr = [9,12,14]
let r = every(arr, v => v > 10)
// false

const some = (array, fn) => {
  let result = false
  for (const value of array) {
    result = fn(value)
    if (result) {
			break
		} 
  }
  return result
}
// 测试
let arr = [1,3,4,9]
let r = some(arr, v => v % 2 === 0)
```



## 1.5 闭包

### 1.5.1 概念

闭包 (Closure):函数和其周围的状态(词法环境)的引用捆绑在一起形成闭包

* 可以在另一个作用域中调用一个函数的内部函数并访问到该函数的作用域中的成员

```js
function makeFn () {
  let msg = 'Hello function'
  return function () {
    console.log(msg)
  }
}
const fn = makeFn()
fn()
```

```js
// once
function once (fn) {
  let done = false
  return function () {
    if (!done) {
    done = true
    return fn.apply(this, arguments)
  }
}
let pay = once(function (money) { 
  console.log(`支付:${money} RMB`)
})
// 只会支付一次 pay(5)
pay(5)
pay(5)
pay(5)
```



* 闭包的本质：函数在执行的时候会放到一个执行栈上当函数执行完毕之后会从执行栈上移除，**但是堆上的作用与成员因为被外部引用不能释放**，因此内部函数依然可以访问外部函数的成员

* 闭包的好处：延长了外部函数内部变量的作用范围



### 1.5.2 案例

> 结合浏览器调试工具查看，dev-tools -> sources -> f10,f11

```js
// 生成计算数字的多少次幂的函数 
function makePower (power) {
	return function (x) { 
    return Math.pow(x, power)
	} 
}
let power2 = makePower(2) 
let power3 = makePower(3)
console.log(power2(4)) 
console.log(power3(4))
```

```js
// 第一个数是基本工资，第二个数是绩效工资 
function makeSalary (x) {
    return function (y) {
      return x + y
		} 
}
let salaryLevel1 = makeSalary(1500) 
let salaryLevel2 = makeSalary(2500)
console.log(salaryLevel1(2000)) console.log(salaryLevel1(3000))
```



## 1.6 纯函数（Pure functions）

### 1.6.1 概念

* 纯函数：相同的输入永远会得到相同的输出，而且没有任何可观察的副作用

  * 纯函数就类似数学中的函数（用来描述输入和输出之间的关系），y=f(x)

  ![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/JavaScript深度剖析/1.png)

* [lodash](https://github.com/lodash/lodash) 是一个纯函数的功能库，提供了对数组、数字、对象、字符串、函数等操作的一些方法

* 数组 `slice` 和 `splice` 分别是：纯函数和不纯的函数

  * `slice` 返回数组中的指定部分，不会改变原数组
  * `splice` 对数组进行操作返回该数组，会改变原数组

```js
let numbers = [1, 2, 3, 4, 5] 
// 纯函数 
numbers.slice(0, 3) 
// => [1, 2, 3] 
numbers.slice(0, 3) 
// => [1, 2, 3] 
numbers.slice(0, 3) 
// => [1, 2, 3]

// 不纯的函数 
numbers.splice(0, 3) 
// => [1, 2, 3] 
numbers.splice(0, 3) 
// => [4, 5] 
numbers.splice(0, 3) 
// => []
```



* 函数式编程不会保留计算中间的结果，所以变量是不可变的（无状态的）
* 我们可以把一个函数的执行结果交给另一个函数去处理



### 1.6.2 lodash

 ```js
 // 演示 lodash
 // first \ last \ toUpper \ reverse \ each \ includes \ find \ findIndex
 
 const _ = require('lodash')
 const array = ['zs', 'ls', 'ww', 'zl']
 console.log(_.first(array)) // 'zs'
 console.log(_.last(array))  // 'zl'
 console.log(_.toUpper(_.first(array))) // 'ZS'
 console.log(_.reverse(array)) // ['zl', 'ww', 'ls', 'zs']
 
 const r = _.each(array, (item, index) => {
 	console.log(item, index)
 })
 console.log(r) // ['zl', 'ww', 'ls', 'zs']
 ```



### 1.6.3 纯函数好处

1. 可缓存
   1. 因为纯函数对相同的输入始终有相同的结果，所以可以把纯函数的结果缓存起来

```js
const _ = require('lodash')
function getArea(r) {
	return Math.PI * r *r
}
let getAreaWithMemory = _.memoize(getArea)
console.log(getAreaWithMemory(4))
```




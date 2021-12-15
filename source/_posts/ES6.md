---

title: ES6
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: DarkStrand.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-12-08 13:41:00
comments: true
tags: 
 - js
keywords: js
description: js
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---



# 1-ECMAScript概述

实际上 JavaScript 是 ECMAScript 的扩展语言。

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/ES6/1.png)

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/ES6/2.png)

JavaScript 语言本身指的就是 ECMAScript。

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/ES6/3.png)



# 2-ES6概述

ES2015也可以叫做ES6

* 相比于ES5.1变化较大

* 自此之后命名规则改变

  

  [ES6](http://www.ecma-international.org/ecma-262/6.0/)



* 解决原有语法上的一些问题或者不足（let,const提供的块级作用域）
* 对原有语法进行增强（解构，展开，参数默认值，模板字符串）
* 全新的对象、全新的方法、全新的功能（promise，proxy）
* 全新的数据类型和数据结构（symbol，set，map）



# 3-准备工作

nodejs、nodemon



# 4-let与块级作用域

作用域：某个成员能够起作用的范围

在此之前，ES只有两种作用域

* 全局作用域
* 函数作用域

* 块级作用域



块：`{}`包裹的范围

```js
if (true) {
	console.log('zce.me')
}
for (var i = 0; i < 10; i++) {
	console.log('zce.me')
}
```

以前块没有独立的作用域

* 在块级作用域内定义的成员外部无法访问

```js
for (var i = 0; i < 3; i++) {
	for (var i = 0; i< 3; i++) {
		console.log(i)
  }
  console.log('内层结束i = ' + i)
}
// 0
// 1
// 2
// 内层结束i = 3

for (let i = 0; i < 3; i++) {
	for (let i = 0; i< 3; i++) {
		console.log(i)
  }
  console.log('内层结束i = ' + i)
}
// 0
// 1
// 2
// 内层结束i = 0
// 0
// 1
// 2
// 内层结束i = 1
// 0
// 1
// 2
// 内层结束i = 2
```

> let解决循环嵌套计数器重名导致的问题，但一般不建议使用同名

```js
//var elements = [{},{},{}]
//for (var i = 0; i< elements.length; i++) {
//	elements[i].onclick = function() {
//		console.log(i)
//  }
//}
//elements[0].onclick() // 3
// 全局作用域中的i已经被累加到了3，不管打印哪个i都是3

//var elements = [{},{},{}]
//for (var i = 0;i < elements.length; i++) {
//	elements[i].onclick = (function(i) {
//		return function () {
//			console.log(i)
//    }
//  })(i)
//}
//elements[0].onclick() // 0
// 闭包可以解决，借助函数作用域摆脱全局作用域影响

var elements = [{},{},{}]
for (let i = 0; i< elements.length; i++) {
	elements[i].onclick = function() {
		console.log(i)
  }
}
elements[0].onclick() // 0
// 运用块级作用域
```



for循环内部有两层作用域

```js
for (let i = 0; i< 3;i++) {
	let i = 'foo'
  console.log(i)
}
// 'foo'
// 'foo'
// 'foo'
// 这两个i互不影响，也就是两个i不在一个作用域内

=> 相当于
let i = 0
if (i < 3) {
	let i = 'foo'
  console.log(i)
}
i++

if (i < 3) {
	let i = 'foo'
  console.log(i)
}
i++

if (i < 3) {
	let i = 'foo'
  console.log(i)
}
i++
```



* let声明不会提升（var 声明变量会提升）

```js
console.log(foo) // undefined
var foo = 'zce'

console.log(foo) // 报错
let foo = 'zce'
```



# 5-const(恒量/变量)

let基础上多了「只读」，声明过后不允许再被修改。声明时必须赋值。

> const声明的成员不允许被修改，只是说不允许在声明过后重新指向新的内存地址，并不是说不允许修改恒量中的属性成员。

```js
const obj = {}
obj.name = 'zce'

obj = [] // 报错
```



> 最佳实践：不用 var，主用 const，配合 let



# 6-数组的解构



```js
// 以前
const arr = [100,200,300]
const foo = arr[0]
const bar = arr[1]
const baz = arr[2]
console.log(foo, bar, baz)

// 使用解构快速赋值
//const [foo, bar, baz] = arr
//console.log(foo, bar, baz)

// 只想获取指定位置成员，需要保留','，确保格式一致
const [,,baz] = arr
console.log(baz)

// 可以加'...'，表示提取从当前位置开始往后的所有成员，这些成员会放到数组中，这三个点只能放在最后
const [foo, ...rest] = arr
console.log(rest) // [200, 300]

// 如果解构位置成员个数小于被解构的数组长度，会按照从前到后的顺序提取，多出来的不会被提取
// 反之如果解构位置成员个数大于被解构的数组长度的话，提取到的就是undefined（相当于提取数组中不存在的下标）
// 也可以设置默认值变量后面+'='
const [foo, bar, baz=123, more = 'default value'] = arr
console.log(bar, more) // 200 'default value'


// 例子：拆分字符串，获取拆分后的指定位置
const path = '/foo/bar/baz'
//const tmp = path.split('/')
//const rootdir = tmp[1] // foo

const [, rootdir] = path.split('/')
console.log(rootdir) // 'foo'
```





# 7-对象的解构

对象的解构需要根据属性名匹配提取而不是位置，因为数组中元素有下标（有顺序规则），而对象里面的成员没有固定次序，所以不能按照位置提取。

解构对象其他特点基本和解构数组一致，例如：没有匹配到的成员会返回undefined，也能设置默认值。

```js
const obj = {name: 'zce', age: 18}
//const { name } = obj

// 重命名，这样就不会出现变量冲突
const name = 'tom'
const {name: objName = 'jack'} = obj
console.log(objName)

const {log} = console
log(1)
log(2)
```



# 8-模板字符串字面量

反引号`

* 可以支持多行
* 支持插值表达式

```js
const str = `hello es2015, this is a \`string\``
console.log(str) // 'hello es2015, this is a `string`'

const name = 'tom'
const msg = `hey, ${name}---${1+2}`
console.log(msg)
```



# 9-模板字符串标签函数

```js
const name = 'tom'
const gender = true

function myTagFunc(strings, name, gender) {
	console.log(strings, name, gender)
  return strings[0] + name + strings[1] + gender + strings[2]
}

const result = myTagFunc`hey, ${name} is a ${gender}.`
console.log(result)
//[ 'hey, ', ' is a ', '.' ] tom true
//hey, tom is a true.
```



# 10-字符串的扩展方法

## 10.1-includes()

## 10.2-startsWith()

## 10.3-endsWith()

```js
const message = 'Error: foo is not defined.'
console.log(message.startsWith('Error')) // true
console.log(message.endsWith('.')) // true
console.log(message.includes('foo')) // true
```



# 11-函数参数默认值

> 注意：
>
> 1. 函数参数默认值会在没有传参或者传的是undefined的时候赋值
> 2. 有多个参数的时候设置默认值应当放在最后

```js
// 原先
function foo(enable) {
	// enable = enable || true
  enable = enable === undefined ? true : false
  console.log(enable)
}

// 现在
function foo(bar, enable = true) {
	console.log(enable)
}
```



# 12-剩余参数

```js
// 以前
// 使用arguments（伪数组）接收
function foo() {
  console.log(arguments) //[Arguments] { '0': 1, '1': 2, '2': 3, '3': 4 }
}
foo(1,2,3,4)

// 现在
function foo(first, ...args) {
	console.log(args) // [2,3,4]
}
foo(1,2,3,4)
```



# 13-展开数组

```js
const arr = ['foo', 'bar', 'baz']

// 以前
console.log(arr[0], arr[1], arr[2])

// 改进1
console.log.apply(console, arr)

// 现在
console.log(...arr)
```



# 14-箭头函数

```js
// 以前
function inc(number) {
	return number + 1
}

// 现在
const inc = (n, m) => n + 1
console.log(inc(100))

// 例子
const arr = [1,2,3,4,5,6]
arr.filter(function(item) {
	return item % 2
})
arr.filter(i => i % 2)
```



# 15-箭头函数与this

* 箭头函数不会改变 this 指向

```js
// 例子1
const person = {
	name: 'tom',
  sayHi: function() {
    console.log(`hi, my name is ${this.name}`)
  }
}
person.sayHi() // 'hi, my name is tom'

// 例子2
const person = {
	name: 'tom',
  sayHi: () => {
    console.log(`hi, my name is ${this.name}`)
  }
}
person.sayHi() // 'hi, my name is undefined'

// 例子3
const person = {
	name: 'tom',
  sayHiAsync: function() {
		setTimeout(function() {
			console.log(this.name)
    }, 1000)
  }
}
person.sayHiAsync() // undefined
// setTimeout中的普通function拿到的this指向是全局对象

// 例子4
const person = {
	name: 'tom',
  sayHiAsync: function() {
		setTimeout(()=> {
			console.log(this.name)
    }, 1000)
  }
}
person.sayHiAsync() // 'tom'
```



# 16-对象字面量增强

```js
const bar = '123'

// 以前
const obj = {
	foo: 123,
  bar: bar,
  method: function() {
		console.log('method')
  }
}
obj[Math.random()] = 123

// 现在
const obj = {
	foo: 123,
  bar,
  method() {
		console.log('method')
  },
  [Math.random()]: 123
}

console.log(obj)
```



# 17-对象的扩展方法-Object.assign

> 将多个源对象中的属性复制到一个目标对象中
>
> 即，用后面对象的属性去覆盖前面对象

```js
const source = {
	a: 123,
  b: 123
}
const target = {
	a: 456,
  c: 456
}
const result = Object.assign(target, source)
console.log(target)
console.log(result === target)

//{ a: 123, c: 456, b: 123 }
//true

// 例子
function fuc(obj) {
	obj.name = 'bb'
  console.log(obj)
}
const obj = {name: 'aa'}
fuc(obj)
console.log(obj)
// { name: 'bb' }
// { name: 'bb' }


// 改进
function fuc(obj) {
  const funcObj = Object.assign({}, obj)
  funcObj.name = 'bb'
  console.log(funcObj)
}
const obj = {name: 'aa'}
fuc(obj)
console.log(obj)
// { name: 'bb' }
// { name: 'aa' }
```



# 18-对象的扩展方法-Object.is

> 判断两个值是否相等

```js
NaN == NaN // false
Object.is(NaN, NaN) // true
+0 == -0 // true
+0 === -0 // true
OBject.is(+0, -0) // false
```



# 19-Proxy(代理对象)

Object.defineProperty: 监视数据属性读写

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”，即对编程语言进行编程。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

```js
const person = {
	name: 'zs',
  age: 20
}
const personProxy = new Proxy(person, {
	get(target, property) {
		return property in target ? target[property] : 'default'
  },
  set(target, property, value) {
    if (property === 'age') {
			if (!Number.isInteger(value)) {
				throw new TypeError(`${value} is not an int`)
      }
    }
		target[property] = value
  }
})

personProxy.age = 'hh'
console.log(personProxy.name)
```



# 20-Reflect

Reflect属于一个静态类（和Math一样）

Reflect内部封装了一系列对对象的底层操作

Reflect成员方法就是 Proxy 处理对象的默认实现

统一提供一套用于操作对象的API

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)

```js
const obj = {
	foo: '123',
  bar: '456'
}
const proxy = new Proxy(obj, {
	get(target, property) {
    console.log('watch logic~')
    return Reflect.get(target, property)
  }
})
console.log(proxy.foo)
```

```js
const obj = {
	name: 'zs',
  age: 18
}
console.log('name' in obj)
console.log(delete obj['age'])
console.log(Object.keys(obj))

// =>

console.log(Reflect.has(obj, 'name'))
console.log(Reflect.deleteProperty(obj, 'age'))
console.log(Reflect.ownKeys(obj))
```



# 21-Promise

## 21.1-Promise的含义

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件，更加合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单来说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上来说， Promise是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise` 对象有以下两个特点。

1. 对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

> 注意，为了行为方便，后面的`resolved`统一只指`fulfilled`状态，不包含`rejected`状态。

有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。

`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反映到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用[stream](https://nodejs.org/api/stream.html)模式是比部署`Promise`更好的选择。



## 21.2-基本用法

ES6规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

下面代码创造了一个`Promise`实例。

```js
const promise = new Promise(function(resolve, reject) {
	// ...some code
  if (/*异步操作成功*/) {
		resolve(value)
  } else {
    reject(error)
  }
})
```

`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变成“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错位，作为参数传递出去。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。

```js
promise.then(function(value) {
	// success
}, function(error) {
	// failure
})
```

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。

下面是一个`Promise`对象的简单例子。

```js
function timeout(ms) {
	return new Promise((resolve, reject) => {
		setTimeout(resolve, ms, 'done')
  })
}
timeout(100).then(value => {
	console.log(value)
})
```

上面代码中，`timeout`方法返回一个`Promise`实例，表示一段时间以后才会发生的结果。过了指定时间（`ms`参数）以后，`Promise`实例的状态变为`resolved`，就会触发`then`方法绑定的回调函数。

Promise 新建后就会立即执行。

```js
let promise = new Promise(function(resolve, reject) {
	console.log('Promise')
  resolve()
})
promise.then(function() {
  console.log('resolved.')
})
console.log('Hi!')

// Promise
// Hi!
// resolved
```

上面代码中，Promise新建后立即执行，所以首先输出的是`Promise`。然后，`then`方法指定的回调函数，将在当前脚本所有同步任务执行完成才会执行，所以`resolved`最后输出。



下面是异步加载图片的例子。

```js
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    const image = new Image()
    image.onload = function() {
			resolve(image)
    }
    image.onerror = function() {
      reject(new Error('Could not load image at ' + url))
    }
    image.src = url
  })
}
```

上面代码中，使用`Promise`包装了一个图片加载的异步操作。如果加载成功，就调用`resolve`方法，否则就调用`reject`方法。



下面是一个用`Promise`对象实现的 Ajax 操作的例子。

```js
const getJSON = function(url) {
	const promise = new Promise(function(resolve, reject) {
		const handler = function() {
			if (this.readyState !== 4) {
				return
      }
      if (this.status === 200) {
				resolve(this.response)
      } else {
				reject(new Error(this.statusText))
      }
    }
    const client = new XMLHttpRequest()
    client.open('GET', url)
    client.onreadystatechange = handler
    client.responseType = 'json'
    client.setRequestHeader('Accept', 'application/json')
    client.send()
  })
  return promise
}

getJSON('/posts.json').then(function(json) {
	console.log('Contents: ' + json)
}, function(error) {
	console.error('出错了', error)
})
```

上面代码中，`getJSON`是对 XMLHttpRequest 对象的封装，用于发出一个针对 JSON 数据的 HTTP 请求，并且返回一个 `Promise` 对象。需要注意的是，在`getJSON`内部，`resolve`函数和`reject`函数调用时，都带有参数。



如果调用`resolve`函数和`reject`函数时带有参数，那么它们的参数会被传递给回调函数。`reject`函数的参数通常是`Error`对象的实例，表示抛出的错误；`resolve`函数的参数除了正常的值以外，还可能是另一个 Promise 实例，比如像下面这样。

```js
const p1 = new Promise(function(resolve, reject) {
	// ...
})
const p2 = new Promise(function(resolve, reject) {
  // ...
  resolve(p1)
})
```

上面代码中，`p1`和`p2`都是 Promise 的实例，但是`p2` 的 `resolve` 方法将 `p1` 作为参数，即一个异步操作的结果是返回给另一个异步操作。

注意，这时`p1`的状态就会传递给`p2`，也就是说，`p1`的状态决定了`p2`的状态。如果`p1`的状态是`pending`，那么`p2`的回调函数就会等待`p1`的状态改变；如果`p1`的状态已经是`resolved`或者`rejected`，那么`p2`的回调函数将会立刻执行。



```js
const p1 = new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('fail')), 3000)
})

const p2 = new Promise(function (resolve, reject) {
    setTimeout(() => resolve(p1), 1000)
})

p2
    .then(result => console.log(result))
    .catch(error => console.log(error))
// Error: fail
```

上面代码中，`p1`是一个 Promise，3秒后变为`rejected`。`p2`的状态在1秒后改变，`resolve`方法返回的是`p1`。由于`p2`返回的是另一个 Promise，导致`p2`自己的状态无效了，由`p1`的状态决定`p2`的状态。所以，后面的`then`语句都变成针对后者(p1)。又过了2秒，`p1`变为`rejected`，导致触发`catch`方法指定的回调函数。



> 调用`resolve`或`reject`并不会终结 Promise 的参数函数的执行。

```js
new Promise((resolve, reject) => {
  resolve(1)
  console.log(2)
}).then(r => {
  console.log(r)
})
// 2
// 1
```

上面代码中，调用`resolve(1)`以后，后面的`console.log(2)`还是会执行，并且会首先打印出来。这是因为立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。



一般来说，调用`resolve`或`reject`以后，Promise 的使命就完成了，后续操作应该放到`then`方法里面，而不应该直接写在`resolve`或`reject`的后面。所以，最好在它们前面加上`return`语句，这样就不会有意外。

```js
new Promise((resolve, reject) => {
  return resolve(1)
  // 后面的语句不会执行
  console.log(2)
})
```



## 21.3-Promise.prototype.then()

Promise实例具有`then`方法，也就是说，`then` 方法是在定义原型对象`Promise.prototype`上的。它的作用是为Promise实例添加改变时的回调函数。前面说过，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数（可选）是`rejected`状态的回调函数。

`then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`写法。

```js
getJSON('/posts.json').then(function(json) {
	return json.post
}).then(function(post) {
	// ...
})
```

上面的代码使用`then`方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

采用链式的`then`，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个`Promise`对象（即有异步操作），这时后一个回调函数，就会等待该`Promise`对象的状态发生变化，才会被调用。

```js
getJSON('/post/1.json').then(function(post) {
	return getJSON(post.commentURL)
}).then(function(comments) {
  console.log('resolved:', comments)
}, function(err) {
	console.log('rejected:', err)
})
```

上面代码中，第一个`then`方法指定的回调函数，返回的是另一个`Promise`对象。这时，第二个`then`方法指定的回调函数，就会等待这个新的`Promise`对象状态发生变化。如果变为`resolved`，就调用第一个回调函数，如果状态变为`rejected`，就调用第二个回调函数。

如果采用箭头函数，上面的代码可以写得更简洁。

```js
getJSON('/post/1.json').then(
	post => getJSON(post.commentURL)
).then(
	comments => console.log('resolved:', comments),
  err => console.log('rejected:', err)
)
```



## 21.4-Promise.prototype.catch()

`Promise.prototype.catch`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

```js
getJSON('/posts.json').then(function(posts) {
	// ...
}).catch(function(error) {
	 // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error)
})
```

上面代码中，`getJSON`方法返回一个 Promise 对象，如果该对象状态变为`resolved`，则会调用`then`方法指定的回调函数；如果异步操作抛出错误，状态就会变为`rejected`，就会调用`catch`方法指定的回调函数，处理这个错误。另外，`then`方法指定的回调函数，如果运行中抛出错误，也会被`catch`方法捕获。

```js
p.then(val => console.log('fulfilled:', val))
	.catch(err => console.log('rejected:', err))

// 等同于
p.then(val => console.log('fulfilled:', val))
	.then(null, err => console.log('rejected:', err))
```

下面就是一个例子。

```js
const promise = new Promise(function(resolve, reject) {
	throw new Error('test')
})
promise.catch(function(error) {
	console.log(error)
})
// Error: test
```

上面代码中，`promise`抛出一个错误，就被`catch`方法指定的回调函数捕获。注意，上面的写法与下面两种写法是等价的。

```js
// 写法一
const promise = new Promise(function(resolve, reject) {
	try {
		throw new Error('test')
  } catch(e) {
		reject(e)
  }
})
promise.catch(function(error) {
	console.log(error)
})

// 写法二
const promise = new Promise(function(resolve, reject) {
	reject(new Error('test'))
})
promise.catch(function(error) {
	console.log(error)
})
```

比较上面两种写法，可以发现`reject`方法的作用，等同于抛出错误。

如果 Promise 状态已经变成 `resolved`，再抛出错误是无效的。

```js
const promise = new Promise(function(resolve, reject) {
	resolve('ok')
  throw new Error('test')
})
promise
	.then(function(value) { console.log(value) })
	.catch(function(error) { console.log(error) })
// ok
```

上面代码中，Promise 在`resolve`语句后面，再抛出错误，不会被捕获，等于没有抛出。因为 Promise 的状态一旦改变，就永久保持该状态，不会再变了。

Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。

```js
getJSON('/post/1.json').then(function(post) {
	return getJSON(post.commentURL)
}).then(function(comments) {
	// some code
}).catch(function(error) {
	// 处理前面三个Promise产生的错误
})
```

上面代码中，一共有三个 Promise 对象：一个由`getJSON`产生，两个由`then`产生。它们之中任何一个抛出的错误，都会被最后一个`catch`捕获。

一般来说，不要在`then`方法里面定义 Reject 状态的回调函数（即`then`的第二个参数），总是使用`catch`方法。

```js
// bad
promise
	.then(function(data) {
		// success
	}, function(err) {
		// error
	})

// good
promise
	.then(function(data) { //cb
		// success
	})
	.catch(function(err) {
		// error
	})
```

上面代码中，第二种写法要好于第一种写法，理由是第二种写法可以捕获前面`then`方法执行中的错误，也更接近同步的写法(`try/catch`)。因此，建议总是使用`catch`方法，而不使用`then`方法的第二个参数。



跟传统的`try/catch`代码块不同的是，如果没有使用`catch`方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。

```js
const someAsyncThing = function() {
	return new Promise(function(resolve, reject) {
		// 下面一行会报错，因为x没有声明
    resolve(x + 2)
  })
}
someAsyncThing().then(function() {
	console.log('everything is great')
})

setTimeout(() => {console.log(123)}, 2000)
// VM13477:4 Uncaught (in promise) ReferenceError: x is not defined
// 123
```

上面代码中，`someAsyncThing`函数产生的 Promise 对象，内部有语法错误。浏览器运行到这一行，会打印出错误提示`ReferenceError: x is not defined`，但是不会退出进程、终止脚本执行，2秒之后还是会输出 123 。这就是说，Promise 内部的错误不会影响到 Promise 外部的代码，通俗的说法就是“Promise会吃掉错误”。

这个脚本放在服务器执行，退出码就是`0`（即表示执行成功）。不过，Node 有一个`unhandledRejection`事件，专门监听未捕获的`reject`错误，上面的脚本会触发这个事件的监听函数，可以在监听函数里面抛出错误。

```js
process.on('unhandledRejection', function(err, p) {
	throw err
})
```

上面代码中，`unhandledRejection`事件的监听函数有两个参数，第一个是错误对象，第二个是报错的 Promise 实例，它可以用来了解发生错误的环境信息。

注意，Node 有计划在未来废除`unhandledRejection`事件。如果 Promise 内部有未捕获的错误，会直接终止进程，并且进程的退出码不为0。

再看下面的例子。

```js
const promise = new Promise(function(resolve, reject) {
	resolve('ok')
  setTimeout(function() {
		throw new Error('test')
  }, 0)
})
promise.then(function(value) {
	console.log(value)
})
// ok
// Uncaught Error: test
```

上面代码中，Promise 指定在下一轮“事件循环”再抛出错误。到了那个时候，Promise 的运行已经结束了，所以这个错误是在 Promise 函数体外抛出的，会冒泡到最外层，成了未捕获的错误。

一般总是建议，Promise 对象后面要跟`catch`方法，这样可以处理 Promise 内部发生的错误。`catch`方法返回的还是一个 Promise 对象，因此后面还可以接着调用`then`方法。

```js
const someAsyncThing = function() {
	return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2)
  })
}
someAsyncThing()
	.catch(function(error) {
		console.log('oh no', error)
	})
	.then(function() {
		console.log('carry on')
	})
// on no [ReferenceError: x is not defined]
// carry on
```

上面代码运行完`catch`方法指定的回调函数，会接着运行后面那个`then`方法指定的回调函数。如果没有报错，则会跳过`catch`方法。

```js
Promise.resolve()
.catch(function(error) {
	console.log('oh no', error)
})
.then(function() {
	console.log('carry on')
})
// carry on
```

上面的代码因为没有报错，跳过了`catch`方法，直接执行后面的`then`方法。此时，要是`then`方法里面报错，就与前面的`catch`无关了。

`catch`方法之中，还能再抛出错误。

```js
const someAsyncThing = function() {
  return new Promsie(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2)
  })
}
someAsyncThing().then(function() {
	return someOtherAsyncThing()
}).catch(function(error) {
	console.log('oh no', error)
  // 下面一行会报错，因为 y 没有声明
  y + 2
}).then(function() {
	console.log('carry on')
})
// oh no [ReferenceError: x is not defined]
```

上面代码中，`catch`方法抛出一个错误，因为后面没有别的`catch`方法了，导致这个错误不会被捕获，也不会传递到外层。如果改写一下 ，结果就不一样了。

```js
someAsyncThing().then(function() {
  return someOtherAsyncThing()
}).catch(function(error) {
	console.log('oh no', error)
  // 下面一行会报错，因为y没有声明
  y + 2
}).catch(function(error) {
	console.log('carry on', error)
})
// oh no [ReferenceError: x is not defined]
// carry on [ReferenceError: y is not defined]
```

上面代码中，第二个`catch`方法用来捕获前一个`catch`方法抛出的错误。



## 21.5-Promise.prototype.finally()

`finally`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

```js
promise
.then(result => {...})
.catch(error => {...})
.finally(() => {...})
```

上面代码中，不管`promise`最后的状态，在执行完`then`或`catch`指定的回调函数以后，都会执行`finally`方法指定的回调函数。



下面是一个例子，服务器使用 Promise 处理请求，然后使用`finally`方法关掉服务器。

```js
server.listen(port)
	.then(function() {
  	// ...
	})
	.finally(server.stop)
```

`finally`方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是 `fulfilled` 还是 `rejected`。这表明，`finally` 方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。

`finally`本质上是`then`方法的特例。

```js
promise
.finally(() => {
	// 语句
})

// 等同于
promise
.then(
	result => {
		// 语句
    return result
  },
  error => {
		// 语句
    throw error
  }
)
```

上面代码中，如果不使用`finally`方法，同样的语句需要为成功和失败两种情况各写一次。有了`finally`方法，则只需要写一次。

它的实现也很简单。

```js
Promise.prototype.finally = function(callbck) {
	let p = this.constructor
  return this.then(
  	value => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => {throw reason})
  )
}
```

上面代码中，不管前面的 Promise 是`fulfilled`还是`rejected`，都会执行回调函数`callback`。

从上面的实现还可以看到，`finally`方法总是会返回原来的值。

```js
// resolve 的值是 undefined
Promise.resolve(2).then(() => {}, () => {})

// resolve 的值是 2
Promise.resolve(2).finally(() => {})

// reject 的值是 undefined
Promise.reject(3).then(() => {}, () => {})

// reject 的值是 3
Promise.reject(3).finally(() => {})

```



## 21.6-Promise.all()

`Promsie.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.all([p1, p2, p3])
```

上面代码中，`Promise.all()`方法接受一个数组作为参数，`p1`、`p2`、`p3`都是 Promise 实例，如果不是，就会先调用下面讲到的`Promise.resolve`方法，将参数转为 Promise 实例，再进一步处理。另外，`Promise.all()`方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。

`p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

1. 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。
2. 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。



下面是一个具体的例子。

```js
// 生成一个Promise对象的数组
const promises = [2,3,5,7,11,13].map(function(id) {
	return getJSON('/post/' + id + '.json')
})
Promise.all(promises).then(function(posts) {
  // ...
}).catch(function(reason) {
	// ...
})
```

上面代码中，`promises`是包含6个 Promise 实例的数组，只有这6个实例的状态都变成`fulfilled`，或者其中有一个变为`rejected`，才会调用`Promise.all`方法后面的回调函数。



下面是另一个例子。

```js
const databasePromise = conncetDatabase()
const booksPromise = databasePromise.then(findAllBooks)
const uerPromise = databasePromise.then(getCurrentUser)

Promise.all([
  booksPromise,
  userPromise
])
.then([books, user] => 
pickTopRecommendations(books, user))
```

上面代码中，`booksPromise`和`userPromise`是两个异步操作，只有等到它们的结果都返回了，才会触发`pickTopRecommendations`这个回调函数。



> 注意，如果作为参数的 Promise 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。

```js
const p1 = new Promise((resolve, reject) => {
	resolve('hello')
})
.then(result => result)
.catch(e => e)

const p2 = new Promise((resolve, reject) => {
	throw new Error('报错了')
})
.then(result => result)
.catch(e => e)

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e))
// ['hello', Error: 报错了]
```

上面代码中，`p1`会`resolved`，`p2`首先会`rejected`，但是`p2`有自己的`catch`方法，该方法返回的是一个新的 Promise 实例，`p2`指向的实际上是这个实例。该实例执行完`catch`方法后，也会变成`resolved`，导致`Promise.all()`方法参数里面的两个实例都会`resolved`，因此会调用`then`方法指定的回调函数，而不会调用`catch`方法指定的回调函数。

如果`p2`没有自己的`catch`方法，就会调用`Promise.all()`的`catch`方法。

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// Error: 报错了
```



## 21.7-Promise.race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise实例。

```js
const p = Promise.race([p1,p2,p3])
```

上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 `p` 的回调函数。

`Promise.race()`方法的参数与`Promise.all()`方法一样，如果不是Promise实例，就会先调用下面讲到的`Promise.resolve()`方法，将将参数转为 Promise 实例，再进一步处理。

下面是一个例子，如果指定时间内没有获得结果，就将 Promise 的状态变为 `reject`，否则变为`resolve`。

```js
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function(resolve, reject) {
		setTimeout(() => reject(new Error('request timeout')), 5000)
  })
])

p
.then(console.log)
.catch(console.error)
```

上面代码中，如果5秒之内`fetch`方法无法返回结果，变量`p`的状态就会变为`rejected`，从而触发`catch`方法指定的回调函数。



## 21.8-Promise.allSettled()



## 21.9-Promise.any()



## 21.10-Promise.resolve()



## 21.11-Promise.reject()



## 21.12-应用

### 21.12.1-加载图片



### 21.12.2-Generator函数与Promise的结合



## 21.13-Promise.try()







# 22-class类

```js
// 以前
function Person(name) {
	this.name = name
}
Person.prototype.say = function() {
	console.log(`hi, my name is ${this.name}`)
}

// 现在
class Person() {
  constructor(name) {
		this.name = name
  }
  say() {
		console.log(`hi, my name is ${this.name}`)
  }
}
const p = new Person('tom')
p.say()
```



# 23-静态方法（static）

ES2015中新增添加静态成员的 static 关键词

```js
class Person() {
  constructor(name) {
		this.name = name
  }
  say() {
		console.log(`hi, my name is ${this.name}`)
  }
  static create(name) {
    return new Person(name)
  }
}
const tom = Person.create('tom')
tom.say()

```

> 静态方法是挂载到类型上的，所以静态方法内部this不会指向某个实例对象，而是当前的类型。



# 24-类的继承（extends）

```js
class Person {
	constructor(name) {
		this.name = name
  }
  say() {
		console.log(`hi, my name is ${this.name}`)
  }
}
class Student extends Person {
  constructor(name, number) {
		super(name)
    this.number = number
  }
  hello() {
		super.say()
    console.log(`my school number is ${this.number}`)
  }
}
const s = new Student('jack', '100')
s.hello()
```



# 25-Set数据结构

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

`Set`本身是一个构造函数，用来生成 Set 数据结构。

```js
const s = new Set()
s.add(1).add(2).add(3).add(4).add(2)

console.log(s)

s.forEach(i => console.log(i))

for(let i of s) {
	console.log(i)
}

console.log(s.size)

console.log(s.has(100))

console.log(s.delete(3))

console.log(s)

s.clear()
console.log(s)

const arr = [1,2,3,1,2]
const result = Array.from(new Set(arr))
const result = [...new Set(arr)]
console.log(result)
```



# 26-Map数据结构

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

```js
const obj = {}
obj[true] = 'value'
obj[123] = 'value'
obj[{a: 1}] = 'value'

console.log(Object.keys(obj))
// [ '123', 'true', '[object Object]' ]
```

```js
const m = new Map()
const tom = { name: 'tom' }
m.set(tom, 90)
console.log(m)
console.log(m.get(tom))

// m.has()
// m.delete()
// m.clear()

//Map(1) { { name: 'tom' } => 90 }
//90

m.forEach((value,key) => {
	console.log(value, key)
})
// 90 { name: 'tom' }
```



# 27-Symbol

一种全新的原始数据类型，表示独一无二的值

```js
// shared.js ==================
const ache = {}

// a.js========================
cache['a_foo'] = Math.random()

// b.js========================
cache['b_foo'] = '123'
console.log(cache )
```

```js
console.log(Symbol('foo'))
console.log(Symbol('bar'))

const obj = {}
obj[Symbol()] = '123'
obj[Symbol()] = '456'
console.log(obj)

const obj = {
	[Symbol()]: 123
}
console.log(obj)
```

> 最主要的作用就是为对象添加独一无二的属性名

```js
console.log(Symbol() === Symbol()) // false
console.log(Symbol('foo') === Symbol('foo')) // false
console.log(Symbol.for('foo') === Symbol.for('foo')) // true

console.log(Symbol.iterator) // Symbol(Symbol.iterator)
console.log(Symbol.hasInstance) // Symbol(Symbol.hasInstance)

const obj = {
	[Symbol.toStringTag]: 'XObject'
}
console.log(obj.toString()) // [object XObject]

const obj = {
	[Symbol()]: 'symbol value',
  foo: 'normal value'
}
for (var key in obj) {
	console.log(key)
}
// 'foo'
console.log(Object.keys(obj)) //[ 'foo' ]

console.log(JSON.stringify(obj)) // {"foo":"normal value"}
```



# 28-for of 循环

* for适合遍历普通数组
* for...in 遍历键值对
* 一些对象的遍历方法

这些遍历都有局限性

for...of作为遍历所有数据结构的统一方式

```js
const arr = [100,200,300,400]
for (const item of arr) {
	console.log(item)
  if (item > 100) {
		break // 随时终止循环，forEach不能跳出循环
  }
}

// 一些伪数组也可以遍历(arguments)

const s = new Set(['foo', 'bar'])
for (const item of s) {
	console.log(item)
}
// 'foo'
// 'bar'

const m = new Map()
m.set('foo', '123')
m.set('bar', '456')
for (const [key, value] of m) {
	console.log(key, value)
}
// 'foo' '123'
// 'bar' '456'

// 遍历对象
const obj = {
  foo: 123,
  bar: 456
}
for (const i of obj) {
	console.log(i) // 报错
}
```



# 29-Iterable可迭代接口

ES中能够表示有结构的数据类型越来越多。为了给各种各样的数据结构提供统一遍历方式，ES2015提供了 Iterable 接口。

> 接口（规格标准）

实现 Iterable 接口就是 for...of 的前提

![](![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/ES6/4.png))

```js
const set = new Set(['foo', 'bar', 'baz'])
const iterator = set[Symbol.iterator]()
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
console.log(iterator.next())
// { value: 'foo', done: false }
// { value: 'bar', done: false }
// { value: 'baz', done: false }
// { value: undefined, done: true }
```



# 30-实现可迭代接口

```js
const obj = {
	[Symbol.iterator]: function() {
		return {
			next: function() {
				return {
					value: 'zce',
          done: true // 用了表示迭代是否结束
        }
      }
    }
  }
}
```

```js
const obj = {
	store: ['foo', 'bar', 'baz'],
  [Symbol.iterator]: function() {
		let index = 0
    const self = this
    
    return {
			next: function() {
				const result = {
					value: self.store[index],
          done: index >= self.store.length
        }
        index++
        return result
      }
    }
  }
}
for (const item of obj) {
	console.log(item)
}
// 'foo'
// 'bar'
// 'baz'
```



# 31-迭代器模式

> 对外提供统一遍历接口，让外部不用关心数据内部结构是怎样的。
>
> ES2015中迭代器是在语言层面实现的迭代器模式，适用于任何数据结构，只需要通过代码实现Iterator方法，实现迭代逻辑。

 ```js
 const todos = {
 	life: ['吃饭', '睡觉', '打豆豆'],
   learn: ['语文', '数学', '外语'],
   work: ['喝茶'],
   
   each: function(callback) {
 		const all = [].concat(this.life, this.learn, this.work)
     for (const item of all) {
 			callback(item)
     }
   },
   
   [Symbol.iterator]: function() {
 		const all = [...this.life, ...this.learn, ...this.work]
     let index = 0
     return {
 			next: function() {
 				return {
 					value: all[index],
           done: index++ >= all.length
         }
       }
     }
   }
 }
 ```



# 32-生成器（Generator）

避免异步编程中回调嵌套过深，提供更好的异步编程解决方案

> Generator 函数有多种理解角度。语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。
>
> 执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
>
> 形式上，Generator 函数是一个普通函数，但是有两个特征。一是，`function`关键字与函数名之间有一个星号；二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）。

```js
// Generator函数
function * foo() {
	console.log('zce')
  return 100
}
const result = foo()
console.log(result)
console.log(result.next())

// Object [Generator] {}
// zce
// { value: 100, done: true }
```

```js
function * foo() {
	console.log('111')
  yield 100
  console.log('222')
  yield 200
  console.log('333')
  yield 300
}
const generator = foo()
console.log(generator.next()) 
// 111
// { value: 100, done: false }
console.log(generator.next())
// 222
// { value: 200, done: false }
console.log(generator.next())
// 333
// { value: 300, done: false }
console.log(generator.next())
// { value: undefined, done: true }
```

> 总结一下，调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。



# 33-生成器应用

```js
// 发号器
function * createIdMaker() {
  let id = 1
  while(true) {
    yield id++
  }
}
const idMaker = createIdMaker()
console.log(idMaker.value.id)
```

```js
const todos = {
	life: ['吃饭', '睡觉', '打豆豆'],
  learn: ['语文', '数学', '外语'],
  work: ['喝茶'],
  
  [Symbol.iterator]: function() {
		const all = [...this.life, ...this.learn, ...this.work]
    for (const item of all) {
			yield item
    }
  }
}

for (const item of todos) {
	console.log(item)
}
```



# 34-ES Modules



# 35-ECMAScript 2016

## 35.1-Array.prototype.includes

> indexOf不能判断数组中是否存在NaN

```js
const arr = ['foo', 1, NaN, false]
console.log(arr.indexOf('foo')) // 0
console.log(arr.indexOf('bar')) // -1
console.log(arr.indexOf(NaN)) // -1

console.log(arr.includes('foo')) // true
console.log(arr.includes(NaN)) // true
```





## 35.2-指数运算符

```js
console.log(Math.pow(2,3)) // 8

console.log(2 ** 3) // 8
```



# 36-ECMAScript 2017

## 36.1-Object.values

```js
const obj = {
	foo: 'value1',
  bar: 'value2'
}
console.log(Object.values(obj))
// [ 'value1', 'value2' ]
```



## 36.2-Object.entries

```js
const obj = {
	foo: 'value1',
  bar: 'value2'
}
console.log(Object.entries(obj))
// [ [ 'foo', 'value1' ], [ 'bar', 'value2' ] ]

for (const [key, value] of Object.entries(obj)) {
	console.log(key, value)
}
// 'foo' 'value1'
// 'bar' 'value2'

console.log(new Map(Object.entries(obj)))
// Map(2) { 'foo' => 'value1', 'bar' => 'value2' }
```



## 36.3-Object.getOwnPropertyDescriptors

> 获取对象中属性完整描述信息

```js
const p1 = {
	firstName:  'Lei',
  lastName: 'Wang',
  get fullName() {
    return this.firstName + ' ' + this.lastName
  }
}
console.log(p1.fullName) // Lei Wang
const p2 = Object.assign({}, p1)
p2.firstName = 'zce'
console.log(p2)
// { firstName: 'zce', lastName: 'Wang', fullName: 'Lei Wang' }

const descriptors = Object.getOwnPropertyDescriptors(p1)
const p2 = Object.defineProperties({}, descriptors)
p2.firstName = 'zce'
console.log(p2.fullName)
```



## 36.4-String.prototype.padStart / String.prototype.padEnd

```js
const books = {
  html: 5,
  css: 16,
  javascript: 128
}
for (const [name, count] of Object.entries(books)) {
	console.log(name, count)
}
// html 5
// css 16
// javascript 128

for (const [name, count] of Object.entries(books)) {
	console.log(`${name.padEnd(16, '-')}|${count.toString().padStart(3, '0')}`)
}
// html------------|005
// css-------------|016
// javascript------|128
```

* 参数（接收两个参数）：
  * 第一个参数，指定字符串的长度。如果当前字符串小于指定的长度，则进行补全；反之，不进行任何操作，返回原字符串。
  * 第二个参数，用于补充的字符串，如果字符串长度过长，则会删除后面的多出的字符串，进行补全。如果不写，默认空格补全。
* 适用场景：
  * 格式化时间时，个位数补0
  * 字符串拼接

下面都是以padStart来进行举例的，padEnd 的用法和 padStart 一样，一个是在开头补全，一个是在末尾进行补全。

```js
'2'.padStart(2, '0') // '02'
'abc'.padStart(10, '1234678956565') // 多余字符串截掉
// '1234678abc'
'abc'.padStart(5) // 第二个参数不写，用空格替代
// '  abc'
```

第二个参数的一些其他写法

```js
'abc'.padStart(8, null) // null将作为一个字符串来使用
// 'nullabc'
'abc'.padStart(8, undefined) // '     abc'
'abc'.padStart(8, []) // []将会原样输出
// 'abc'
'abc'.padStart(18, {}) // '[object Object]abc'
'abc'.padStart(8, false) // 'falseabc'
```



## 36.5-在函数参数中添加尾逗号

 ```js
 function foo (
 	bar,
   baz,
 ) {
 }
 
 const arr = [
   100,
   200,
   300,
   400,
 ]
 ```



# 37-Async/Await

## 37.1-含义

ES2017 标准引入了 async 函数，使得异步操作变得更加方便。

async 函数是什么？一句话，它就是 Generator 函数的语法糖。

前文有一个 Generator 函数，依次读取两个文件。

```js
const fs = require('fs');

const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

上面代码的函数`gen`可以写成`async`函数，就是下面这样。

```js
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

一比较就会发现，`async`函数就是将 Generator 函数的星号（`*`）替换成`async`，将`yield`替换成`await`，仅此而已。

`async`函数对 Generator 函数的改进，体现在以下四点。

1. 内置执行器

Generator 函数的执行必须靠近执行器，所以才有了`co`模块，而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。

```js
asyncReadFile()
```

上面的代码调用了`asyncReadFile`函数，然后它就会自动执行，输出最后结果。这完全不像 Generator 函数，需要调用 `next` 方法，或者`co`模块，才能真正执行，得到最后结果。



2. 更好的语义

`async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。



3. 更广的适用性

`co`模块约定，`yield`命令后面只能是 Thunk 函数或 Promise 对象，而 `async` 函数的 `await` 命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。



4. 返回值是 Promise

`async`函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用`then`方法指定下一步的操作。

进一步说，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而 `await` 命令就是内部`then`命令的语法糖。



## 37.2-基本用法

`async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。当函数执行的时候，一旦遇到 `await` 就会先返回，等到异步操作完成，再接着执行函数内后面的语句。

下面是一个例子。

```js
async function getStockPriceByName(name) {
	const symbol = await getStockSymbol(name)
  const stockPrice = await getStockPrice(symbol)
  return stockPrice
}
getStockPriceByName('goog').then(function (result) {
	console.log(result)
})
```

上面代码是一个获取股票报价的函数，函数前面的`async`关键字，表明该函数内部有异步操作。调用该函数时，会立即返回一个 `Promise` 对象。

下面是另一个例子，指定多少毫秒后输出一个值。

```js
function timeout(ms) {
	return new Promise(resolve => {
		setTimeout(resolve, ms)
  })
}
async function asyncPrint((value, ms) {
	await timeout(ms)
  console.log(value)
})
asyncPrint('hello world', 50)
```

上面代码指定 50 毫秒之后，输出 hello world。

由于`async`函数返回的是 Promise 对象，可以作为`await`命令的参数。所以，上面的例子也可以写成下面的形式。

```js
async function timeout(ms) {
  await new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);
```



Async 函数有多种使用形式。

```js
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 对象的方法
let obj = { async foo() {} };
obj.foo().then(...)

// Class 的方法
class Storage {
  constructor() {
    this.cachePromise = caches.open('avatars');
  }

  async getAvatar(name) {
    const cache = await this.cachePromise;
    return cache.match(`/avatars/${name}.jpg`);
  }
}

const storage = new Storage();
storage.getAvatar('jake').then(…);

// 箭头函数
const foo = async () => {};
```



## 37.3-语法

`async`函数的语法规则总体上比较简单，难点是错误处理机制。



## 37.4-返回 Promise 对象

`async`函数返回一个 Promise 对象。

`async`函数内部`return`语句返回的值，会成为`then`方法回调函数的参数。

```js
async function f() {
	return 'hello world'
}
f().then(v => console.log(v))
// 'hello world'
```

上面代码中，函数`f`内部`return`命令返回的值，会被`then`方法回调函数接收到。

`async`函数内部抛出错误，会导致返回的 Promise 对象变为`reject`状态。抛出的错误会被`catch`方法回调函数接收到。

```js
async function f() {
	throw new Error('出错了')
}
f().then(
	v => console.log(v),
  e => console.log(e)
)
// Error: 出错了
```



## 37.5-Promise对象的状态变化

`async`函数返回的 Promise 对象，必须等到内部所有`await`命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到`return`语句或者抛出错误。也就是说，只有`async`函数内部的异步操作执行完，才会执行`then`方法指定的回调函数。

下面是一个例子。

```js
async function getTitle(url) {
  let response = await fetch(url);
  let html = await response.text();
  return html.match(/<title>([\s\S]+)<\/title>/i)[1];
}
getTitle('https://tc39.github.io/ecma262/').then(console.log)
// "ECMAScript 2017 Language Specification"
```


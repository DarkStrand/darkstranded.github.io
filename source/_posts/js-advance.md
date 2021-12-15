---
title: js进阶
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



# 1. 数据类型

## 1.1 概念

JavaScript 的数据类型有下图所示的8种：

![js数据类型](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/js进阶/1.png)

其中，前7种类型为`基础类型`，最后1种（Object）为`引用类型`，也是需要重点关注的，因为它在日常工作中是使用最频繁，也是需要关注最多技术细节的数据类型。



而引用数据类型（Object）又分为图上这几种常见的类型：Array-数组对象、RegExp-正则对象、Date-日期对象、Math-数学函数、Function-函数对象。



在这里，请重点了解下面两点，因为各种 JavaScript 的数据类型最后都会在初始化之后放在不同的内存中，因此上面的数据类型大致可以分为两类来进行存储：

1. `基础类型`存储在`栈内存`，被引用或拷贝时，会创建一个完全相等的变量；
2. `引用类型`存储在`堆内存`，存储的是地址，多个引用指向同一个地址，这里会涉及一个“**共享**”的概念。



关于引用类型下面直接通过两段代码来讲解，让你深入理解一下核心“共享”的概念。



### 题目一：初出茅庐

```js
let a = {
  name: 'lee',
  age: 18
}
let b = a;
console.log(a.name);  //第一个console  'lee'
b.name = 'son';
console.log(a.name);  //第二个console  'son'
console.log(b.name);  //第三个console  'son'
```

这道题比较简单，我们可以看到第一个 console 打出来 name 是'lee'，这应该没什么疑问；但是在执行了 b.name='son' 之后，结果你会发现 a 和 b 的属性 name 都是 'son'，第二个和第三个打印结果是一样的，这里就体现了引用类型的“共享”的特性，即这两个值都存在同一块内存中 共享，一个发生了改变，另一个也随之跟着变化。



### 题目二：渐入佳境

```js
let a = {
  name: 'Julia',
  age: 20
}
function change(o) {
  o.age = 24;
  o = {
    name: 'Kath',
    age: 30
  }
  return o;
}
let b = change(a);     // 注意这里没有new，后面new相关会有专门文章讲解
console.log(b.age);    // 第一个console
console.log(a.age);    // 第二个console
```

这道题涉及了 function，你通过上述代码可以看到第一个 console 的结果是30，b最后打印结果是 {name: 'Kath', age:30}；第二个 console 的返回结果是24，而 a 最后的打印结果是{name:'Julia', age:24}。



是不是和你预想的有些区别？你要注意的是，**这里的 function 和 return 带来了不一样的东西**。



原因在于：函数传参进来的 o ，传递的是对象在堆中的内存地址值，通过调用 o.age=24 确实改变了 a 对象的 age 属性；12行把参数 o 的地址重新返回了，将{name: 'Kath',age:30}存入其中，最后返回 b 的值就变成了{name:'Kath',age:30}。而如果把第12行去掉，那么 b 就会返回 undefined。



讲完数据类型的基本概念，我们继续看下一部分，如何对数据类型进行检测，这也是比较重要的问题。



## 1.2 检测

数据类型检测也是面试过程中经常会遇到的问题，比如：如何判断是否为数组？让你写一段代码把 JavaScript 的各种数据类型判断出来，等等。类似的题目会很多，而且在平常写代码过程中我们也会经常用到。



有时候回答比如“用 typeof 来判断”，然后就没有其他答案了，但这样的回答是不能令面试官满意的，因为他要考察你对 JS 的数据类型理解的深度，所以我们先要做到的是对各种数据类型的判断方法了然于胸，然后再进行归纳总结，给面试官一个满意的答案。



数据类型的判断方法其实有很多种，比如 typeof 和 instanceof，下面介绍三种在工作中经常会遇到的数据类型检测方法。



### 1.2.1 第一种判读方法：typeof

这是比较常用的一种，那么我们通过一段代码来快速回顾一下这个方法。

```js
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
typeof null // 'object'
typeof [] // 'object'
typeof {} // 'object'
typeof console // 'object'
typeof console.log // 'function'
```

你可以看到，前6个都是基础数据类型，而为什么第6个 null 的 typeof 是'object'呢？这里要和你强调一下，虽然 typeof null 会输出 object，但这只是 JS 存在的一个悠久 Bug，不代表 null 就是引用数据类型，并且 null 本身也不是对象。因此，null 在 typeof 之后返回的是有问题的结果，不能作为判断 null 的方法。如果你需要在 if 语句中判断是否为 null，直接通过'===null'来判断就好。



此外还要注意，引用数据类型 Object，用 typeof 来判断的话，除了 function 会判断为 ok 以外，其余都是'object'，是无法判断出来的。





### 1.2.2 第二种判断方法：instanceof

想必 instanceof 的方法你也听说过，我们 new 一个对象，那么这个新对象就是它原型链继承上面的对象了，通过 instanceof 我们能判断这个对象是否是之前那个构造函数生成的对象，这样就基本可以判断出这个新对象的数据类型。下面通过代码来了解一下。

```js
let Car = function() {}
let benz = new Car()
benz instanceof Car // true
let car = new String('Mercedes Benz')
car instanceof String // true
let str = 'Covid-19'
str instanceof String // false
```

上面就是用 instanceof 方法判断数据类型的大致流程，那么如果让你自己实现一个 instanceof 的底层实现，应该怎么写呢？请看下面的代码。

```js
function myInstanceof(left, right) {
  // 这里先用typeof来判断基础数据类型，如果是，直接返回false
  if (typeof left !== 'object' || left === null) return false;
  // getProtypeOf是Object对象自带的API，能够拿到参数的原型对象
  let proto = Object.getPrototypeOf(left);
  while (true) {
    //无限循环的写法（也可以使用for）
    //循环往下寻找，直到找到相同的原型对象
    if (proto === null) return false; // 找到最顶层
    if (proto === right.prototype) return true; //找到相同原型对象，返回true
    proto = Object.getPrototypeOf(proto); //没找到继续向上一层原型链查找
  }
}
// 验证一下自己实现的myInstanceof是否OK
console.log(myInstanceof(new Number(123), Number)); // true
console.log(myInstanceof(123, Number));

```

现在你知道了两种判断数据类型的方法，那么它们之间有什么差异呢？我总结了下面两点：

1. `instanceof` `可以`准确判断复杂`引用`数据类型，但是`不能`正确判断`基础`数据类型；
2. 而 `typeof` 也存在弊端，它虽然可以判断`基础`数据类型（null除外），但是`引用`数据类型中，除了 `function` 类型以外，其他的也无法判断。

总之，不管单独用 instanceof 还是 typeof，都不能满足所有场景的需求，而只能通过二者混写的方式来判断。但是这种方式判断出来的其实也只是大多数情况，并且写起来也比较难受，其实个人比较推荐下面的第三种方法，想比上述两个而言，能更好地解决数据类型检测问题。



### 1.2.3 第三种判断方法：constructor

原理：每一个实力对象都可通过constructor来访问它的构造函数，其实也是根据原型链的原理来的。

```js
'5'.__proto__.constructor === String // true
[5].__proto__.constructor === Array // true

undefined.__proto__.constructor // Cannot read property '__proto__' of undefined

null.__proto__.constructor // Cannot read property '__proto__' of undefined
```

由于undefined和null是无效的对象，因此是没有constructor属性的，这两个值不能用这种方法判断。



### 1.2.4 第四种判断方法：Object.prototype.toString

`toString()` 是 `Object` 的原型方法，调用该方法，可以统一返回格式为 "`[object Xxx]`" 的字符串，其中 `Xxx` 就是对象的类型。对于 Object 对象，直接调用 `toString()` 就能返回 `[object Object]`；而对于其他对象，则需要通过 `call` 来调用，才能返回正确的类型信息。我们来看一下代码。

```js
Object.prototype.toString({})       // "[object Object]"
Object.prototype.toString.call({})  // 同上结果，加上call也ok
Object.prototype.toString.call(1)    // "[object Number]"
Object.prototype.toString.call('1')  // "[object String]"
Object.prototype.toString.call(true)  // "[object Boolean]"
Object.prototype.toString.call(function(){})  // "[object Function]"
Object.prototype.toString.call(null)   //"[object Null]"
Object.prototype.toString.call(undefined) //"[object Undefined]"
Object.prototype.toString.call(/123/g)    //"[object RegExp]"
Object.prototype.toString.call(new Date()) //"[object Date]"
Object.prototype.toString.call([])       //"[object Array]"
Object.prototype.toString.call(document)  //"[object HTMLDocument]"
Object.prototype.toString.call(window)   //"[object Window]"
```

从上面这段代码可以看出，`Object.prototype.toString.call()` 可以很好地判断引用类型，甚至可以把 document 和 window 都区分开来。

但是在写判断条件的时候一定要注意，使用这个方法最后返回统一字符串格式为"`[object Xxx]`"，而这里字符串里面的"Xxx"，**第一个首字母要大写**（注意：使用typeof 返回的是小写），这里需要多加留意。



那么下面来实现一个全局通用的数据类型判断方法，来加深理解，代码如下。

```js
function getType(obj){
  let type  = typeof obj;
  if (type !== "object") {    // 先进行typeof判断，如果是基础数据类型，直接返回
    return type;
  }
  // 对于typeof返回结果是object的，再进行如下的判断，正则返回结果
  return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, '$1');  // 注意正则中间有个空格
}
/* 代码验证，需要注意大小写，哪些是typeof判断，哪些是toString判断？思考下 */
getType([])     // "Array" typeof []是object，因此toString返回
getType('123')  // "string" typeof 直接返回
getType(window) // "Window" toString返回
getType(null)   // "Null"首字母大写，typeof null是object，需toString来判断
getType(undefined)   // "undefined" typeof 直接返回
getType()            // "undefined" typeof 直接返回
getType(function(){}) // "function" typeof能判断，因此首字母小写
getType(/123/g)      //"RegExp" toString返回
```



## 1.3 转换

在日常的业务开发中，经常会遇到 JavaScript 数据类型转换问题，有的时候需要我们主动进行强制转换，而有的时候 JavaScript 会进行隐式转换，隐式转换的时候就需要我们多加留心。



那么这部分都会涉及哪些内容呢？我们先看一段代码，了解下大致的情况。

```js
'123' == 123   // false or true? true
'' == null    // false or true? false
'' == 0        // false or true? true
[] == 0        // false or true? true
[] == ''       // false or true? true
[] == ![]      // false or true? true
null == undefined //  false or true? true
Number(null)     // 返回什么？0
Number('')      // 返回什么？ 0
parseInt('');    // 返回什么？NaN
{}+10           // 返回什么？10
let obj = {
    [Symbol.toPrimitive]() {
        return 200;
    },
    valueOf() {
        return 300;
    },
    toString() {
        return 'Hello';
    }
}
console.log(obj + 200); // 这里打印出来是多少？400
```



### 1.3.1 强制类型转换

强制类型转换方式包括 `Number()`、`parseInt()`、`parseFloat()`、`toString()`、`String()`、`Boolean()`，这几种方法都比较类似，通过字面意思可以很容易理解，都是通过自身的方法来进行数据类型的强制转换。

上面代码中，第8行的结果是0，第9行的结果同样是0，第10行的结果是NaN。这些都是很明显的强制类型转换，因为用到了 Number() 和 parseInt()。



#### 1.3.1.1 **Number()方法的强制转换规则**

* 如果是布尔值，true 和 false 分别被转换为 1 和 0；
* 如果是数字，返回自身；
* 如果是 null，返回0；
* 如果是 undefined，返回 NaN；
* 如果是字符串，遵循以下规则：如果字符串中只包含数字（或者是 0X/0x 开头的十六进制数字字符串，允许包含正负号），则将其转换为十进制；如果字符串中包含有效的浮点格式，将其转换为浮点数值；如果是空字符串，将其转换为0；如果不是以上格式的字符串，均返回 NaN；
* 如果是 Symbol，抛出错误；
* 如果是对象，并且部署了 `[Symbol.toPrimitive]`，那么调用此方法，否则调用对象的 valueOf() 方法，然后依据前面的规则转换返回的值；如果转换的结果是 NaN，则调用对象的 toString() 方法，再次依照前面的顺序转换返回对应的值（Object转换规则会在下面细讲）。

下面通过一段代码来说明上述规则。

```js
Number(true);        // 1
Number(false);       // 0
Number('0111');      //111
Number(null);        //0
Number('');          //0
Number('1a');        //NaN
Number(-0X11);       //-17
Number('0X11')       //17
Number(undefined)    // NaN
Number({})           // NaN
Number([])           // 0
```

其中，分别列举了比较常见的 Number 转换的例子，它们都会把对应的非数字类型转换成数字类型，而有一些实在无法转换成数字的，最后只能输出 NaN 的结果。



#### 1.3.1.2 **Boolean()方法的强制转换规则**

这个方法的规则是：除了 undefined、null、false、''、0（包括+0，-0）、NaN转换出来是 false，其他都是true。

```js
Boolean(0)          //false
Boolean(null)       //false
Boolean(undefined)  //false
Boolean(NaN)        //false
Boolean(1)          //true
Boolean(13)         //true
Boolean('12')       //true
```



#### 1.3.1.3 **parseInt()方法的强制转换规则**

只能将字符串转换成数值，与`Number()`转字符串的区别是：

* 字符串数字开头或者负号开头，往后取值，直到非数字停止。如：`parseInt('123x') -> 123`、`parseInt('-023x') -> -23`，注意：`parseInt('-0a') -> -0`、`parseInt('-0x') -> NaN`（0x为十六进制的开头）、`parseInt('-abc') -> NaN`；
* 字符串非数字或者负号开头，则为`NaN`，如：`parseInt('x123') -> NaN`；
* 空字符串，返回`NaN`，如：`parseInt('') -> NaN`；
* `parseInt('1.1') -> 1`这也是它和 parseFloat()的区别。



**Number() 和 parseInt() 对比的一张表**

| 值         | Number() | parseInt() |
| ---------- | -------- | ---------- |
| ''         | 0        | NaN        |
| true/false | 1/0      | NaN        |
| '0123'     | 123      | 123        |
| '123x'     | NaN      | 123        |
| 'x123'     | NaN      | NaN        |
| '01.1'     | 1.1      | 1          |
| {}         | NaN      | NaN        |
| []         | 0        | NaN        |



**parseInt() 还有第二个参数**

第二个参数用于指定转换时，转换成多少进制(如2进制、8进制、10进制、16进制 等等)，默认为10进制。

```javascript
parseInt('-023x', 8) // -19

parseInt('010', 10) // 10

parseInt('010', 8) // 8

parseInt('0x10',10) // 0

parseInt('0x10',16) // 16

parseInt('0xf', 16) // 15
```



说到这第二个参数，有一个非常经典的面试题：

```javascript
['1', '2', '3'].map(parseInt) // 会得到什么结果？？？
```

如果你不假思索的写出了 `[1, 2, 3]`，那你的面试可能就会Game Over了。

为什么？？？

正确答案应该是 `[1, NaN, NaN]`... 原因就出在parseInt()的第二个参数身上：



*语法*

```sh
parseInt(string, radix)
```

| 参数   | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| string | 必需。要被解析的字符串。                                     |
| radix  | 可选。表示要解析的数字的基数。该值介于 2～36之间。<br />如果省略该参数或其值为0，则数字将以10为基础来解析。如果它以“0x” 或 “0X” 开头，将以16为基数。<br />如果该参数小于2或者大于36，则parseInt()将返回 NaN。 |

然而，map的语法又是这样的：

```sh
array.map(function(currentValue, index, arr), thisValue)
```

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/js进阶/2.png)

`['1', '2', '3'].map(parseInt)`，其实拆解出来就是： `['1', '2', '3'].map((currentValue, index) => parseInt(currentValue, index))`

三个值转换相当于：`parseInt('1', 0)`、`parseInt('2', 1)`、`parseInt('3', 2)`

> 1. parseInt('1', 0) 第二个参数为0，相当没传，即默认值，也就是10进制，正确转换成1；
> 2. parseInt('2', 1)第二参数不合法，看文档，第二个参数是2-36的值，*如果该参数小于2或者大于36，则 parseInt() 将返回NaN*；
> 3. parseInt('3', 2)参数合法，但是，二进制里没有3这个数字，所以返回 NaN。



#### 1.3.1.4 **parseFloat**

与 parseInt()一样，parseFloat()也可以解析以数字开头的部分数字字符串（非数字部分字符串在转换过程中会被去除）。与 parseInt() 不同的是，parseFloat() 可以将字符串转换成浮点数；但同时，parseFloat() 只接受一个参数，且仅能处理10进制字符串。

* 字符串中的第一个小数点是有效的，而第二个小数点就是无效的了，因此它后面的字符串将被忽略。
* 如果字符串包含的是一个可解析为整数的数（没有小数点，或者小数点后面都是零），parseFloat()会返回整数。

```js
parseFloat('1234blue') // 1234
parseFloat('0xA')      // 0
parseFloat('0908.5')   // 908.5
parseFloat('3.125e7')  // 31250000
parseFloat('123.45.67')// 123.45
parseFloat('')         // NaN
parseFloat('num123')   // NaN
```



#### 1.3.1.5 **toString和String**

String()和toString()都是将其他类型的变量转换为字符串类型。

```js
let a = 1;
let b = 2;
console.log(String(a)) // '1'
console.log(typeof String(a)) // String
console.log(b.toString()) // '2'
console.log(typeof b.toString()) // String

String(true) // 'true'
String(false) // 'false'
String([])  // ''
String({}) // '[object Object]'
```

区别：toString()无法转换 null 和 undefined

```js
let a;
let b = null;
a.toString() // Uncaught TypeError: Cannot read property 'toString' of undefined
b.toString() // Uncaught TypeError: Cannot read property 'toString' of null
console.log(String(a)) // 'undefined'
console.log(String(b)) // 'null'
```



#### 1.3.1.6 valueOf()

js对象中的 `valueOf()` 方法和 `toString()` 方法非常类似，但是，当需要返回对象的原始值而非字符串的时候才调用它，尤其是转换为数字的时候。如果在需要使用原始值的上下文中使用了对象，JavaScript就会自动调用 valueOf() 方法。



**Object.prototype.valueOf()**

JavaScript 调用 `valueOf()` 方法将对象转换为原始值。你很少需要自己调用 `valueOf()` 方法。

默认情况下，`valueOf()` 方法由Object 后面的每个对象继承。每个内置的核心对象都会覆盖此方法以放回适当的值。

如果对象没有原始值，则 `valueOf()` 将返回对象本身。

你可以在自己的代码中使用 `valueOf()` 将内置对象转换为原始值。创建自定义对象时，可以覆盖 `Object.prototype.valueOf()` 来调用自定义方法，而不是默认 `Object` 方法。



*覆盖自定义对象的 valueOf()方法*

你可以创建一个取代 `valueOf()` 方法的函数，你的方法必需不能传入参数。

假设你有个对象叫 `MyNumberType` 而你想为它创建一个 `valueOf()` 方法。下面的代码为 `valueOf()` 方法赋予了一个自定义函数：

```js
MyNumberType.prototype.valueOf = function(){ 
  return customPrimitiveValue 
}
```

有了这样一个方法，下一次每当 `MyNumberType` 要被转换为原始类型值时，JavaScript 在此之前会自动调用自定义的 `valueOf()` 方法。

valueOf() 方法一般都会被 JavaScript 自动调用，但你也可以像下面代码那样自己调用：

```js
myNumberType.valueOf()
```



**String.prototype.valueOf()**

语法：`strObj.valueOf()`

返回值：表示给定`String`对象的原始值

说明：`valueOf()`方法返回一个`String`对象的原始值，该值等同于`String.prototype.toString()`。

该方法通常在 JavaScript 内部被调用，而不是在代码里显示调用。

```js
let x = new String('Hello World')
console.log(x.valueOf()) // Hello World
```



**Date.prototype.valueOf()**

语法：`dateObj.valueOf()`

返回值：表示给定`Date`对象的原始值

说明：`valueOf()`方法返回以数值格式表示的一个`Date`对象的原始值。该值从1970年1月1日0时0分0秒（UTC，即协调世界时）到该日期对象所代表时间的毫秒数。

该方法的功能和`Date.prototype.getTime()`方法一样。

该方法通常在 JavaScript 内部调用，而不是在代码中显示调用。

```js
var x = new Date(2018,1,12)
var myVar = x.valueOf()
console.log(myVar) // 1518364800000
```



**Number.prototype.valueOf()**

语法：`numObj.valueOf()`

返回值：表示给定`Number`对象的原始值。

说明：该方法通常在 JavaScript 内部调用，而不是在代码中显示调用。覆盖`Object.prototype.valueOf()`方法

案例：

```js
var numObj = new Number(2018)
console.log(typeof numObj) // object
console.log(numObj) // Number {2018}
var num = numObj.valueOf()
console.log(typeof num) // number
console.log(num) // 2018
```





**Boolean.prototype.valueOf()**

语法：`bool.valueOf()`

返回值：返回给定`Boolean`对象的原始值

说明：`Boolean`的`valueOf()`方法返回一个`Boolean`字面量的原始值作为布尔数据类型。该方法通常在 JavaScript 内部调用，而不是在代码中显示调用。

案例：

```js
var x = new Boolean()
console.log(typeof x) // object
console.log(x) // Boolean {false}
var xv = x.valueOf()
console.log(typeof xv) // boolean
console.log(xv) // false

var y = new Boolean('weiqinl')
console.log(typeof y) // object
console.log(y.valueOf()) // true
```



**Symbol.prototype.valueOf()**

语法：`Symbol.valueOf()`

返回值：返回给定`Symbol`对象的原始值

说明：`Symbol`的`valueOf()`方法返回`Symbol`对象的原始值作为`Symbol`数据类型。JavaScript 调用`valueOf()`方法将对象转换为原始值。

案例：

```js
Object(Symbol('foo')) + 'bar'
>Uncaught TypeError: Cannot convert a Symbol value to a string at <anonymous>:1:23
Object(Symbol('f00')).valueOf() + 'bar'
>uncaught TypeError: Cannot convert a Symbol value to a string at <anonymous>:1:33
Object(Symbol('foo')).toString() + 'bar'
>"Symbol(foo)bar"
```



#### 1.3.1.7 总结

| 值        | Number() | parseInt() | parseFloat() | toString()        | String()          |
| --------- | -------- | ---------- | ------------ | ----------------- | ----------------- |
| 1         | 1        | 1          | 1            | '1'               | '1'               |
| 0         | 0        | 0          | 0            | '0'               | '0'               |
| ''        | 0        | NaN        | NaN          | ''                | ''                |
| 'blue123' | NaN      | NaN        | NaN          | 'blue123'         | 'blue123'         |
| '123blue' | NaN      | 123        | 123          | '123blue'         | '123blue'         |
| '0123'    | 123      | 123        | 123          | 0123'             | '0123'            |
| '-012.5'  | -12.5    | -12        | -12.5        | '-012.5'          | '-012.5'          |
| '012.5.5' | NaN      | 12         | 12.5         | '0121.5.5'        | '012.5.5'         |
| '0xA'''   | 10       | 10         | 0            | '0xA'             | '0xA'             |
| '0x11'    | 17       | 17         | 0            | '0x11'            | '0x11'            |
| true      | 1        | NaN        | NaN          | 'true'            | 'true'            |
| false     | 0        | NaN        | NaN          | 'false'           | 'false'           |
| null      | 0        | NaN        | NaN          | 报错              | 'null'            |
| undefined | NaN      | NaN        | NaN          | 报错              | 'undefined'       |
| {}        | NaN      | NaN        | NaN          | '[object Object]' | '[object Object]' |
| []        | 0        | NaN        | NaN          | ''                | ''                |



### 1.3.2 隐式类型转换

凡是通过逻辑运算符（&&、||、!）、运算符（+、-、*、/）、关系操作符（>、<、<=、>=）、相等运算符（==）或者 if/while 条件的操作，如果遇到两个数据类型不一样的情况，都会出现隐式类型转换。



下面着重讲解一下日常用得比较多的“==” 和 “+” 这两个符号的隐式转换规则。



#### 1.3.2.1 '=='的隐式类型转换规则

* 如果类型相同，无须进行类型转换；
* 如果其中一个操作值是 null 或者 undefined，那么另一个操作符必须为 null 或者 undefined，才会返回 true，否则都返回 false；
* 如果其中一个是 Symbol 类型，那么返回 false；
* 两个操作值如果为 string 和 number 类型，那么就会将字符串转换为 number；
* 如果一个操作值是 boolean，那么转换成 number；
* 如果一个操作值为 object 且另一方为 string、number 或者 symbol，就会把 object 转为原始类型再进行判断（调用 object 的 value/toString 方法进行转换）

如果直接死记这些理论会有点懵，我们还是直接看代码，这样更容易理解一些，如下所示。

```js
null == undefined       // true  规则2
null == 0               // false 规则2
'' == null              // false 规则2
'' == 0                 // true  规则4 字符串转隐式转换成Number之后再对比
'123' == 123            // true  规则4 字符串转隐式转换成Number之后再对比
0 == false              // true  e规则 布尔型隐式转换成Number之后再对比
1 == true               // true  e规则 布尔型隐式转换成Number之后再对比
var a = {
  value: 0,
  valueOf: function() {
    this.value++;
    return this.value;
  }
};
// 注意这里a又可以等于1、2、3
console.log(a == 1 && a == 2 && a ==3);  //true f规则 Object隐式转换
// 注：但是执行过3遍之后，再重新执行a==3或之前的数字就是false，因为value已经加上去了，这里需要注意一下
```



#### 1.3.2.2 '+'的隐式类型转换规则

'+'号操作符，不仅可以用作数字相加，还可以用作字符串拼接。仅当'+'号两边都是数字时，进行的是加法运算；如果两边都是字符串，则直接拼接，无须进行隐式类型转换。

除了上述比较常规的情况外，还有一些特殊的规则，如下所示。

* 如果其中一个是字符串，另一个是 undefined、null或布尔型，则调用 String() 方法进行字符串拼接；如果是纯对象、数组、正则等，则默认调用对象的转换方法会存在优先级，然后再进行拼接。
* 如果其中有一个是数字，另外一个是 undefined、null、布尔型或数字，则会将其转换成数字进行加法运算，对象的情况还是参考上一条规则。
* 如果其中一个是字符串、一个是数字，则按照字符串规则进行拼接。

下面结合代码理解上述规则，如下所示。

```js
1 + 2        // 3  常规情况
'1' + '2'    // '12' 常规情况
// 下面看一下特殊情况
'1' + undefined   // "1undefined" 规则1，undefined转换字符串
'1' + null        // "1null" 规则1，null转换字符串
'1' + true        // "1true" 规则1，true转换字符串
'1' + 1n          // '11' 比较特殊字符串和BigInt相加，BigInt转换为字符串
1 + undefined     // NaN  规则2，undefined转换数字相加NaN
1 + null          // 1    规则2，null转换为0
1 + true          // 2    规则2，true转换为1，二者相加为2
1 + 1n            // 错误  不能把BigInt和Number类型直接混合相加
'1' + 3           // '13' 规则3，字符串拼接
```

整体来看，如果数据中有字符串，JavaScript 类型转换还是更倾向于转换成字符串，因为第三条规则中可以看到再字符串和数字相加的过程中最后返回的还是字符串，这里需要关注一下。

了解了'+'的转换规则后，我们最后再看一下Object的转换规则。





### 1.3.3 Object的转换规则

对象转换的规则，会先调用内置的 [ToPrimitive] 函数，其规则逻辑如下：

* 如果部署了 Symbol.toPrimitive 方法，优先调用再返回；
* 调用 valueOf()，如果转换为基础类型，则返回；
* 调用 toString()，如果转换为基础类型，则返回；
* 如果都没有返回基础类型，会报错。

直接理解有些晦涩，还是直接看代码。

```js
var obj = {
  value: 1,
  valueOf() {
    return 2;
  },
  toString() {
    return '3'
  },
  [Symbol.toPrimitive]() {
    return 4
  }
}
console.log(obj + 1); // 输出5
// 因为有Symbol.toPrimitive，就优先执行这个；如果Symbol.toPrimitive这段代码删掉，则执行valueOf打印结果为3；如果valueOf也去掉，则调用toString返回'31'(字符串拼接)
// 再看两个特殊的case：
10 + {}
// "10[object Object]"，注意：{}会默认调用valueOf是{}，不是基础类型继续转换，调用toString，返回结果"[object Object]"，于是和10进行'+'运算，按照字符串拼接规则来，参考'+'的规则C
[1,2,undefined,4,5] + 10
// "1,2,,4,510"，注意[1,2,undefined,4,5]会默认先调用valueOf结果还是这个数组，不是基础数据类型继续转换，也还是调用toString，返回"1,2,,4,5"，然后再和10进行运算，还是按照字符串拼接规则，参考'+'的第3条规则
```



# 2. 实现深浅拷贝

开始前，先抛出两个问题，可以思考一下

1. 拷贝一个很多嵌套的对象怎么实现？
2. 在面试官眼中，写成什么样的深拷贝代码才能算合格？

## 2.1 浅拷贝的原理和实现

对于浅拷贝的定义我们可以初步理解为：

> 自己创建一个新的对象，来接受你要重新复制或引用的对象值。如果对象属性是基本的数据类型，复制的就是基本类型的值给新对象；但如果属性是引用数据类型，复制的就是内存中的地址，如果其中一个对象改变了这个内存中的地址，肯定会影响到另一个对象。

下面总结了一些 JavaScript 提供的浅拷贝方法，一起来看看哪些方法能实现上述定义所描述的过程。



### 2.1.1 object.assign

`object.assign` 是 ES6 中 object 的一个方法，该方法可以用于 JS 对象的合并等多个用途，其中一个用途就是可以进行浅拷贝。该方法的第一个参数是拷贝的目标对象，后面的参数是拷贝的来源对象（也可以是多个来源）。

> object.assign 的语法为：Object.assign(target, ...sources)

object.assign 的示例代码如下：

```js
let target = {};
let source = { a: { b: 1 } };
Object.assign(target, source);
console.log(target); // { a: { b: 1 } };
```

从上面的代码中可以看到，通过 `object.assign` 我们的确简单实现了一个浅拷贝，“target” 就是我们新拷贝的对象，下面再看一个和上面不太一样的例子。

```js
let target = {};
let source = { a: { b: 2 } };
Object.assign(target, source);
console.log(target); // { a: { b: 10 } }; 
source.a.b = 10; 
console.log(source); // { a: { b: 10 } }; 
console.log(target); // { a: { b: 10 } };
```

从上面代码中我们可以看到，首先通过 Object.assign 将 source 拷贝到 target 对象中，然后我们尝试将 source 对象中的 b 属性由 2 修改为 10。通过控制台可以发现，打印结果中，三个 target 里的 b 属性变为 10 了，证明 Object.assign 暂时实现了我们想要的拷贝效果。



但是使用 object.assign 方法有几点需要注意：

* 它不会拷贝对象的继承属性；
* 它不会拷贝对象的不可枚举的属性；
* 可以拷贝 Symbol 类型的属性。

可以简单理解为：Object.assign 循环遍历原对象的属性，通过复制的方法将其赋值给目标对象的相应属性，来看一下这段代码，以验证它可以拷贝 Symbol 类型的对象。

```js
let obj1 = { a:{ b:1 }, sym:Symbol(1)}; 
Object.defineProperty(obj1, 'innumerable' ,{
    value:'不可枚举属性',
    enumerable:false
});
let obj2 = {};
Object.assign(obj2,obj1)
obj1.a.b = 2;
console.log('obj1',obj1);
console.log('obj2',obj2);
```

我们来看一下控制台打印的结果，如下图所示。

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/js进阶/3.png)

从上面的样例代码中可以看到，利用 object.assign 也可以拷贝 Symbol 类型的对象，但是如果到了对象的第二层属性 obj1.a.b 这里的时候，前者值的改变也会影响后者的第二层属性的值，说明其中依旧存在着访问共同堆内存的问题，也就是说这种方法还不能进一步复制，而只是完成了浅拷贝的功能。



### 2.1.2 扩展运算符方式

我们也可以利用 JS 的扩展运算符，在构造对象的同时完成浅拷贝的功能。

> 扩展运算符的语法为：let cloneObj = {...obj};

代码如下所示。

```js
/* 对象的拷贝 */
let obj = {a:1,b:{c:1}}
let obj2 = {...obj}
obj.a = 2
console.log(obj)  //{a:2,b:{c:1}} 
console.log(obj2); //{a:1,b:{c:1}}
obj.b.c = 2
console.log(obj)  //{a:2,b:{c:2}} 
console.log(obj2); //{a:1,b:{c:2}}
/* 数组的拷贝 */
let arr = [1, 2, 3];
let newArr = [...arr]; //跟arr.slice()是一样的效果
```

扩展运算符和 object.assign 有同样的缺陷，也就是实现的浅拷贝的功能差不多，但是如果属性都是基本类型的值，使用扩展运算符进行浅拷贝会更加方便。



### 2.1.3 concat拷贝数组

数组的 concat 方法其实也是浅拷贝，所以连接一个含有引用类型的数组时，需要注意修改原数组中的元素的属性，因为它会影响拷贝之后连接的数组。不过 concat 只能用于数组的浅拷贝，使用场景比较局限。代码如下所示。

```js
let arr = [1, 2, 3];
let newArr = arr.concat();
newArr[1] = 100;
console.log(arr);  // [ 1, 2, 3 ]
console.log(newArr); // [ 1, 100, 3 ]
```



### 2.1.4 slice拷贝数组

slice 方法也比较有局限性，因为它仅仅针对数组类型。slice 方法返回一个新的数组对象，这一对象由该方法的前两个参数来决定原数组截取的开始和结束时间，是不会影响和改变原始数组的。

> slice 的语法为：arr.slice(begin, end);

我们来看一下 slice 怎么使用，代码如下所示。

```js
let arr = [1, 2, {val: 4}];
let newArr = arr.slice();
newArr[2].val = 1000;
console.log(arr);  //[ 1, 2, { val: 1000 } ]
```

从上面的代码中可以看出，这就是浅拷贝的限制所在了——它只能拷贝一层对象。如果存在对象的嵌套，那么浅拷贝将无能为力。因此深拷贝就是为了解决这个问题而生的，它能解决多层对象嵌套问题，彻底实现拷贝。



### 2.1.5 手工实现一个浅拷贝

根据以上对浅拷贝的理解，如果让你自己实现一个浅拷贝，大致的思路分为两点：

1. 对基础数据做一个最基本的拷贝；
2. 对引用类型开辟一个新的存储，并且拷贝一层对象属性。

那么，围绕着这两个思路，自己实现一个浅拷贝，代码如下。

```js
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? []: {};
    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
          cloneTarget[prop] = target[prop];
      }
    }
    return cloneTarget;
  } else {
    return target;
  }
}
```





## 2.2 深拷贝的原理和实现

浅拷贝只是创建了一个新的对象，复制了原有对象的基本类型的值，而引用数据类型只拷贝了一层属性，再深层的还是无法进行拷贝。深拷贝则不同，对于复杂引用数据类型，其在堆内存中完全开辟了一块内存地址，并将原有的对象完全复制过来存放。



这两个对象是相互独立、不受影响的，彻底实现了内存上的分离。总的来说，深拷贝的原理可以总结如下：

> 将一个对象从内存中完整地拷贝出来一份给目标对象，并从堆内存中开辟一个全新的空间存放新对象，且新对象的修改并不会改变原对象，二者实现真正的分离。



### 2.2.1 JSON.stringify

JSON.stringify() 是目前开发过程中最简单的深拷贝方法，其实就是把一个对象序列化成为 JSON 的字符串，并将对象里面的内容转换成字符串，最后再用 JSON.parse() 的方法将 JSON 字符串生成一个新的对象。示例代码如下所示。

```js
let obj1 = { a:1, b:[1,2,3] }
let str = JSON.stringify(obj1)；
let obj2 = JSON.parse(str)；
console.log(obj2);   //{a:1,b:[1,2,3]} 
obj1.a = 2；
obj1.b.push(4);
console.log(obj1);   //{a:2,b:[1,2,3,4]}
console.log(obj2);   //{a:1,b:[1,2,3]}
```

从上面的代码可以看到，通过 JSON.stringify 可以初步实现一个对象的深拷贝，通过改变 obj1 的 b属性，其实可以看出 obj2 这个对象也不受影响。



但是使用 JSON.stringify 实现深拷贝还是有一些地方值得注意，总结有这几点：

1. 拷贝的对象的值中如果有函数、undefined、symbol 这几种类型，经过 JSON.stringify 序列化之后的字符串中这个键值对会消失；
2. 拷贝 Date 引用类型会变成字符串；
3. 无法拷贝不可枚举的属性；
4. 无法拷贝对象的原型链；
5. 拷贝 RegExp 引用类型会变成空对象；
6. 对象中含有 NaN、Infinity 以及 -Infinity，JSON 序列化的结果会变成 null；
7. 无法拷贝对象的循环应用，即对象成环（obj[key]=obj）。

针对这些存在的问题，可以尝试用下面这段代码亲自执行，看看如此复杂的对象，如果用 JSON.stringify 实现深拷贝会出现什么情况。

```js
function Obj() { 
  this.func = function () { alert(1) }; 
  this.obj = {a:1};
  this.arr = [1,2,3];
  this.und = undefined; 
  this.reg = /123/; 
  this.date = new Date(0); 
  this.NaN = NaN;
  this.infinity = Infinity;
  this.sym = Symbol(1);
} 
let obj1 = new Obj();
Object.defineProperty(obj1,'innumerable',{ 
  enumerable:false,
  value:'innumerable'
});
console.log('obj1',obj1);
let str = JSON.stringify(obj1);
let obj2 = JSON.parse(str);
console.log('obj2',obj2);
```

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/js进阶/4.png)



### 2.2.2 基础版（手写递归实现）

下面是一个实现 ` deepClone` 函数封装的例子，通过 `for in` 遍历传入参数的属性值，如果值是引用类型则再次递归调用该函数，如果是基础数据类型就直接复制，代码如下所示。

```js
let obj1 = {
  a:{
    b:1
  }
}
function deepClone(obj) { 
  let cloneObj = {}
  for(let key in obj) {                 //遍历
    if(typeof obj[key] ==='object') { 
      cloneObj[key] = deepClone(obj[key])  //是对象就再次调用该函数递归
    } else {
      cloneObj[key] = obj[key]  //基本类型的话直接复制值
    }
  }
  return cloneObj
}
let obj2 = deepClone(obj1);
obj1.a.b = 2;
console.log(obj2);   //  {a:{b:1}}
```

虽然利用递归能实现一个深拷贝，但是同上面的 `JSON.stringify` 一样，还是有一些问题没有完全解决，例如：

1. 这个深拷贝函数并不能复制不可枚举的属性以及 Symbol 类型；
2. 这种方法只是针对普通的引用类型的值做递归复制，而对于 Array、Date、RegExp、Error、Function 这样的引用类型并不能正确地拷贝；
3. 对象的属性里面成环，即循环引用没有解决。

这种基础版本的写法也比较简单，可以应对大部分的应用情况。但是在面试的过程中，如果只能写出这样的一个有缺陷的深拷贝方法，有可能不会通过。

所以为了“拯救”这些缺陷，下面是改进版本。



### 2.2.3 改进版（改进后递归实现）

针对上面的几个待解决问题，先通过四点想关的理论告诉你分别应该怎么做。

1. 针对能够遍历对象的`不可枚举`属性以及 `Symbol` 类型，我们可以使用 Reflect.ownKeys 方法；
2. 当参数为 `Date`、`RegExp` 类型，则直接生成一个新的实例返回；
3. 利用 Object 的 `getOwnPropertyDescriptors` 方法可以获得对象的所有属性，以及对应的特性，顺便结合 Object 的 `create` 方法创建一个新对象，并继承传入原对象的原型链；
4. 利用 `WeakMap` 类型作为 `Hash表`，因为 WeakMap 是`弱引用类型`，可以有效防止`内存泄漏`（可以关注一下 Map 和 weakMap 的关键区别，这里要用 weakMap），作为检测`循环引用`很有帮助，如果存在循环，则引用直接返回 WeakMap 存储的值。

关于第4点的 WeakMap，如果不太了解，建议在面试中不要写出这样的代码，如果只是死记硬背会给自己挖坑。

当然，如果你在考虑到循环引用的问题之后，还能用 WeakMap 来很好地解决，并且向面试官解释这样做的目的，那么你所展示的代码，以及你对问题思考的全面性，在面试官眼中应该算是合格的了。

那么针对上面这几个问题，我们来看下改进后的递归实现的深拷贝代码应该是什么样子的，如下所示。

```js
const isComplexDataType = obj => (typeof obj === 'object' || typeof obj === 'function') && (obj !== null)
const deepClone = function (obj, hash = new WeakMap()) {
  if (obj.constructor === Date) 
  return new Date(obj)       // 日期对象直接返回一个新的日期对象
  if (obj.constructor === RegExp)
  return new RegExp(obj)     //正则对象直接返回一个新的正则对象
  //如果循环引用了就用 weakMap 来解决
  if (hash.has(obj)) return hash.get(obj)
  let allDesc = Object.getOwnPropertyDescriptors(obj)
  //遍历传入参数所有键的特性
  let cloneObj = Object.create(Object.getPrototypeOf(obj), allDesc)
  //继承原型链
  hash.set(obj, cloneObj)
  for (let key of Reflect.ownKeys(obj)) { 
    cloneObj[key] = (isComplexDataType(obj[key]) && typeof obj[key] !== 'function') ? deepClone(obj[key], hash) : obj[key]
  }
  return cloneObj
}
// 下面是验证代码
let obj = {
  num: 0,
  str: '',
  boolean: true,
  unf: undefined,
  nul: null,
  obj: { name: '我是一个对象', id: 1 },
  arr: [0, 1, 2],
  func: function () { console.log('我是一个函数') },
  date: new Date(0),
  reg: new RegExp('/我是一个正则/ig'),
  [Symbol('1')]: 1,
};
Object.defineProperty(obj, 'innumerable', {
  enumerable: false, value: '不可枚举属性' }
);
obj = Object.create(obj, Object.getOwnPropertyDescriptors(obj))
obj.loop = obj    // 设置loop成循环引用的属性
let cloneObj = deepClone(obj)
cloneObj.arr.push(4)
console.log('obj', obj)
console.log('cloneObj', cloneObj)
```

> **语法**：`Object.create(proto[, propertiesObject])`
>
> **参数**：`proto` 新创建对象的原型对象
>
> ​			`propertiesObject` 可选。如果没有指定为`undefined`，则是要添加到新创建对象的可枚举属性（即其自身定义的属性，而不是其原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应`Object.defineProperties()`的第二个参数。
>
> **返回值**：一个新对象，带着指定的原型对象和属性。

> **语法**：`Object.getPrototypeOf()`
>
> **参数**：`obj` 要返回其原型的对象
>
> **返回值**：给定对象的原型。如果没有继承原型，则返回`null`。

> **语法**：`Reflect.ownKeys(target)`
>
> **参数**：`target` 获取自身属性键的目标对象。
>
> **返回值**：由目标对象的自身属性键组成的`Array`。
>
> **区别**：Object.keys()返回属性key，但不包括不可枚举的属性
> 			Reflect.ownKeys()返回所有属性key

> **语法**：`Object.getOwnPropertyDescriptors(obj)`
>
> **参数**：`obj` 任意对象
>
> **返回值**：所指定对象的所有自身属性的描述符，如果没有自身属性，则返回空对象。

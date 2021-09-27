---
title: JavaScript
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: DarkStrand.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-08-13 08：38：25
comments: true
tags: 
 - web
keywords: javascript
description: javascript
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---


## 1、介绍

js全称javascript，是一门运行在浏览器端的脚本语言，现在也可以运行在服务器端（node.js）

## 2、组成
现在说的javasript包含3个组成部分

1. **ECMAScript** 语法标准，语法规则
2. **DOM** 专门用于操作页面元素的方法
3. **BOM** 专门用于操作浏览器的方法

``webapi``有时用来统称1和2

## 3、js书写位置
1. 直接将内容编写在script标签中
2. 可以直接在一个单独的js文件中编写，需要引入

>注意点：只要script标签，设置了src属性，标签内的内容将被忽略

## 4、js的注释

注释给开发者看的，浏览器不会执行，方便维护

* 单行注释// 快捷键``ctrl+/``
* 多行注释/**/ 快捷键``alt+shift+a``

## 5、js的输出语句
> 指的就是往页面中输出的方式
* ``alert`` 警告框
* ``confirm`` 确认框（可以选择的）
* ``prompt`` 输入框，可以让用户输入内容
* ``document.write`` 往页面中追加输入内容
* 支持标签的解析（识别标签）
* ``console.log`` 在控制台打印内容，专门用于调试的，给开发者看

## 6、变量（就是可以变化的量）

### 6.1 作用
存储数据，为了将来可以运算使用

1. 先声明，后赋值
```javascript
var age;
age = 18;
```
2. 同时声明和赋值
```javascript
	var age1 = 18;
	console.log(age1);
```
3.  只声明，不赋值（``undefined``未定义，只要变量声明了但是未赋值，默认值就是undefined）
4.  不声明，直接赋值(**不推荐**，变量一定要声明了，再使用)
```javascript
age3=30;
console.log(age3);
```
5. 不声明，不赋值，直接用  xxx is not defined 一定是没有声明赋值的变量直接用了，会报错
6. 可以同时声明赋值多个变量（本质上，只是省略了var）

### 6.2 变量命名规则和规范
#### 6.2.1 规则

* 变量名必须由字母 数字 下划线和 $组成，不能以数字开头（后来也引入中文，但是不要这么做）
* 变量名不能是关键字或者是保留字
* 变量区分大小写

#### 6.2.2 规范

* 声明的变量要有意义
* 声明的变量，如果很长，遵循驼峰命名（从第二个字母开始，首字母大写，用于分割单词，可读性高）

### 6.3 变量的作用域（变量起作用的区域）
* 全局作用域（在script标签内，在函数外的区域）
	声明在全局作用域的变量，就叫全局变量
	特征：在任何地方都可以使用
* 局部作用域（函数内部的区域，就是局部作用域）
	声明在函数内部的变量，就叫局部变量
	特征：只在当前函数内可以使用，出了函数，就不能用了

### 6.4 变量的访问规则
如果自己的作用域内有这个变量，直接用，改自己的
如果没有，才会往外面找（找全局）,用外面的，改外面的 

### 6.5 隐式全局变量
如果一个没有声明过的变量，直接赋值，就叫隐式全局变量（将来要避免）

### 6.6 预解析（预先解析，浏览器会先查看有多少的变量和函数）
1. 所有的变量声明，都会提升到最顶部，只提升声明，不提升赋值
2. 所有的函数声明，都会提升到最顶部，只提升声明，不提升调用
> 补充说明:
>  1. 多个重名的var声明，后面的var将被忽略
>	 2. 多个重名的函数声明，后面的将前面的覆盖
>	 3. 如果出现了同名的变量和函数，函数优先，函数的优先级>变量
>	 4. 函数内部也会进行预解析

预解析，虽然可以提升我们的声明，但是我们开发时，还是要**先声明，后使用**！！！

## 7、js的数据类型

分为：``简单数据类型``和``复杂数据类型`` （数组、函数、对象）

### 7.1 简单数据类型
#### 7.1.1 number类型（数字类型）

1. 整数（一般使用的是十进制  逢十进一）

	``八进制，逢八进一，0开头``
    ```javascript
    var num3 = 011  //就是1*8+1 为9;
    var num4 = 022  //2*8+2 为18
    ```

	``十六进制，逢16进一，0x开头的数字  0-9abcdef``

    ```javascript
    var num2 = 0x11  //1*16+1 为17;
    var num3 = 0x1a  //16+10为26;
    ```

2. 浮点数（小数）

3. 科学计数法 
  ```javascript
  var num1 = 1e4 就是1*10^4
  ```
  > 注意点：计算机对于小数的运算是不准确的，会有很小很小的误差，所以尽量不要用  小数进行比较运算

#### 7.1.2 string类型 字符串类型
* 通过 '' 或者 "" 包裹的就是字符串，js中单双引号，没有区别，推荐''.
* 字符串通过length，可以获取长度
```
	console.log（str.length）;
```

* 转义字符	
  ``\'`` 表示一个普通的单引号
	``\"`` 表示一个普通的双引号
	``\n`` 表示换行

* 拼串（拼接字符串）
> ``+`` 有拼接字符串的功能，也有运算的功能

``+`` 的规则：
1. 只要两边有字符串（黑色），进行的就是拼串
2. 只有两边都是数字（蓝色），才进行运算

#### 7.1.3 boolean类型 布尔类型 
只有两个值，true真 false假
一般用于比较

#### 7.1.4 undefined 未定义的，变量声明了，但是未赋值，默认值就是underfined

#### 7.1.5 null 空对象



### 7.2 复杂数据类型



## 8、变量与简单数据类型的说明

* 对于简单数据类型，浏览器直接认识（直接量 或者 字面量）
* 若是将字符串当成了变量（未声明，未赋值的变量直接使用，会报错）
* 字符串需要引号包裹


## 9、运算符（操作符）

### 9.1 算术运算符 ``+ - * / %(取余)``
* ``+`` 不仅有拼串的功能，还可以运算
* 其他算数运算符，只有运算的功能，如果遇到了字符串，也会转成数字运算
```javascript
"2" - 1 = >1
```
### 9.2 赋值运算符 =
=（就是赋值）   +=   -=   *=   /=   %=
```javascript
var num = 10;
num += 10; //等价 num = num + 10
```

### 9.3 自增或自减运算符（一元运算符）
#### 9.3.1 自增：让变量的值，在原来的基础上+1
* 语法：
``++num`` 前自增，规则：先让值+1，再返回这个值
``num++`` 后自增，规则：先返回这个值，再让值+1

* 注意点： 
 * 不管是++num 还是num++ 从功能的角度一样的，都是让变量的值+1
 * 虽然++num 或 num++ 都能让变量值+1，但是++num 和 num++这个式子的结果是不一样的

#### 9.3.2 自减：让变量的值，在原来的基础上-1
*语法：
``--num``
``num--``

### 9.4 逻辑运算符（与或非）
``&&`` 并且 两边都要成立，结果才是 true，只要有一个不成立，就是false
``||`` 或 两边只要有一个成立，结果就是true
``!`` 取反

### 9.5 比较运算符 >  <  >=  <=  ==   !=  ===   !==
``==`` 规则：只看值，不看类型（只要值相等，就是相等）
``===`` 规则：看值，又看类型（值和类型都要相等）
``!=`` 规则：只看值，不看类型（只要值不等，就是true）
``!==`` 规则：看值，又看类型（只要值或者类型有一个不等，就是不等，返回true）
	
> ==有一定规则（如果类型不同，转换成相同类型，然后比较）
1. NaN，不等于任何值，包括他自己
2. null，不等于任何值，除了null和undefined
3. undefined，不等于任何值 ，除了null和undefined
4. 看是否有数字或布尔，如果有，转成数字比较
	  true：1，false:0
	  [ ]:0, { }:NaN，'':0
5. 再看是否有字符串，（字符串和复杂数据类型），转成字符串比较
	[ ].toString()--->""
	obj.toString()--->'[object Object]'
6. 都是复杂数据类型，比较的是内存地址
				
### 9.6 运算符优先级
1. 括号的优先级最高
2. 一元运算符 ++  --  !
3. 算数运算符  先乘除%，后加减
4. 比较运算符
5. 逻辑运算符  &&  ||

>记忆：（1）括号的优先级最高，逻辑运算符的优先级最低
>		  （2）先乘除，后加减

## 10、如何判断数据的类型

### 10.1 直接打印看颜色（调试，看颜色）
蓝色的数字，number
黑色	     string
蓝色的布尔值 boolean
灰色	    undefined和null

### 10.2 typeof 变量	它的返回值，也是一个字符串类型
1. number
2. string
3. boolean

> bug: typeof null = object;

## 11、数据类型转换

### 11.1 转成数字
* ``Number（xx）`` 如果拿到的是一个非数字，浏览器也不会报错，返回 ``NaN``,not  a number 
* ``parseInt（xx）`` 从第一个字符开始解析，一直解析到第一个非数字为止
* ``parseFloat（xx）`` 从第一个字符开始解析，可以识别一个``.``  然后解析到非数字为止
* 直接运算 -  *  /  %    +正号（用的最多，例如+age）  -负号		

### 11.2 转成字符串
* String（xx）
* xx.toString()
* 直接拼串 （常用）
	console.log(age+ "");

### 11.3 转成布尔类型
**所有的值，都可以转成布尔类型**

**规则**：只有这6种情况， 0  ""  NaN  null  undefined   false  可以转成false，其他所有值，都是true

* Boolean（）
* !!

### 11.4 NaN的说明（表示非数字，指一个无法用数字表示的数字，not a number）

* 如果浏览器遇到了一个无法用数字表示的数值，就用NaN表示
* 只要看到了NaN，说明代码的执行有问题
* NaN的类型是number类型
* NaN不等于任何值，包括他自己


## 12、流程控制

### 12.1 顺序结构（从上到下执行的，默认）
### 12.2 分支结构（选择结构）

#### 12.2.1 if语句
			
* 语法1
```javascript
if（条件）{
  语句1;
}
```
如果条件成立，执行语句1

* 语法2
```
if（条件）{
  语句1;
}
else {
  语句2;
}
```
如果条件成立，执行语句1，否则执行语句2

* 语法3
```
if（条件1）{
  语句1;
}
else if（条件2） {
  语句2;
}
else {
  语句3;
}
```

如果条件1成立，执行语句1
如果条件1不成立，看条件2，如果条件2成立，执行语句2
如果都不成立，执行语句3


#### 12.2.2 三元运算符（三个值参与运算，只适用于比较简单的判断，可以更加简洁）

语法  var 结果 = 条件 ？ A ：B；（如果条件满足，就是A 否则B）

#### 12.2.3 switch-case（根据变量的具体，进行判断）
```javascript
switch（变量）{
  case 值1:
	  语句1;
	  break;（break表示跳出switch，接着往下执行）
  case 值2:
	  语句2;
	  break;（break表示跳出switch，接着往下执行）
  case 值3:
	  语句3;
	  break;（break表示跳出switch，接着往下执行）
  default:
	  默认语句;
}
```

**判断变量的值**
	如果值，等于值1，执行语句1
	如果值，等于值2，执行语句2
	。。。
	如果都不满足，执行默认语句

**注意点**：
	（1）进行的值的比较，进行的是全等比较
	（2）养成写break的习惯（不写break，会一直往下执行）

#### 12.2.4 三种分支结构的使用场景：
* if else			适用于范围形的判断
* 三元运算符		只适用于比较简单的判断，条件只能写一个，但是简洁
* switch。。 case。。适用于具体值的判断

### 12.3 循环结构

#### 12.3.1 while（当条件成立时，循环执行某件事）
```javascript
while（条件）{
  循环体; //循环执行的内容
}
```
条件成立，执行循环体
判断条件，条案件成立，继续执行循环体
。。。
条件不成立，跳出循环
死循环，永远都没有结束的一个循环，开发中需要避免

* while循环一定要有条件，不然就死循环了
* i++一般放在{..}的最后面
* while循环可以用于实现不明确循环次数的循环

#### 12.3.2 do while
```javascript
do {
  循环体; //重复执行的内容
}while（条件)
```
一上来，直接执行循环体
再判断条件，如果条件成立，再执行循环体
。。。
直到条件不成立，跳出循环
		
> do while一般只适合：不管条件成立不成立，都执行一次
> while 和 do while的区别：
>	 while 如果条件不成立，一次都不执行
>	 do while不管条件成立与否，至少执行一次

#### 12.3.3 for（适用于明确范围的循环）
```
for（初始化语句;判断条件;自增或自减）{
  循环体;
}
```
例：
```javascript
for（var i = 1；i < 5; i++）{
  console.log(1);
}
```

执行语句：
  1. 初始化语句
	2. 判断条件
	3. 自增或自减
	4. 循环体

执行流程分析：1243 243 243 243...
执行流程：
  先初始化语句，判断条件，执行循环体，自增或自减；
	判断条件，执行循环体，自增或自减；
	。。。
	条件不成立，跳出循环；

#### 12.3.4 双重for循环，就是在for循环的外面再套一个for循环
* 外层控制行数
* 内层控制一行打印多少个

	break：如果循环遇到了break，跳出整个循环，整个循环就结束了，后面的次数都不执行了		
	continue：如果循环遇到了continue，跳出本次循环，执行下一次循环

#### 12.3.5 循环的使用场景：
* while比较适合不明确执行次数的循环（循环表白案例）
* do while（少用）只适用于不管成立与否，至少执行一次循环体的情况
* for比较适合于明确范围的循环

## 13、断点调试
（1）F12打开控制台，sources,点开对应的文件
（2）在对应行的行号上，点击打断点，刷新后，浏览器会自动执行停留在断点的位置
操作：
（1）watch监视，监视变量的变化
（2）F10，让代码往下一步执行
（3）F8，从当前位置，执行到下一个断点的位置，如果后面没有断点了，就会一直执行完


## 14、数组（是一个有序的值的集合，可以存储大量的数据）
简单数据类型，在存储大量数据时，一个一个的存，非常的浪费

### 14.1 创建数组的方式
1. 字面量的方式（字面量，直接量，从字面上直接就能看出是什么值的量）
		123  false  'abc'  undefined  null  [ ]:表示数组
``var arr =[ ]``创建一个空数组
> 注意点：数组里面可以存任意类型的数据，但是规范是存储同类型的数据

2. 构造函数的方式
```javascript
	var arr = new Array(); ---创建一个空数组
	var arr = new Array(5);---浪费空间，五个空的位置，没有具体值
```
### 14.2 数组的长度 arr.length
### 14.3 数组的下标（索引）：数组中的每一项，都会有一个唯一的下标
* 从0开始，最大下标arr.length-1
* 数组中下标的范围：0 -----> arr.length-1

### 14.4 数组的取值和存值

* 取值：
**语法：数组名[下标]**
 1. 如果下标存在，直接返回对应项的值
 2. 如果下标不存在，返回undefined

* 存值（改值）
**语法：数组名[下标] = ‘新的值’**
 1. 如果下标存在，直接用新的值覆盖
 2. 如果下标不存在，新建一个项，进行赋值

* 往数组最后添加项
	1. arr[arr.length] = '值';
	2. arr.push('值')；

### 14.5 数组的遍历：遍及所有，从数组的第一项，访问到数组的最后一项
* 数组正序遍历
```javascript
for (var i = 0; i < arr.length; i++) {
`console.log(arr[i]);
}
```

* 数组倒叙遍历
```
for (var i = arr.length-1; i >=0; i--) {
  console.log(arr[i]);
}
```

### 14.6 冒泡排序
1. 冒泡（指得是排序的方式）
2. 排序（就是讲一组没有按照顺序排列的数，经过排列后，按照一定的顺序排列）
	价格排序，成绩排序
	编程界，有十大排序
	
### 14.7 初级版
1. 先排出一趟，排出一个最大值
	1. 遍历数组
	2. 让arr[i] 和 arr[i+1] 比较
	3. 如果arr[i] > arr[i+1] ，交换位置
2. 双重for循环，多排几次，就排好了

### 14.8 中级版（优化）：一趟可以比出一个最大值，每趟下来都可以少比一次
		内循环-j

### 14.9 高级版（优化）：假设成立法，如果已经排好序了，后面就不用再排了
1. 在每趟排列前，先假设，已经排好了
2. 一趟下来，如果一次交换都没有发生，flag值就是true
```javascript
var arr = [7, 6, 5, 4, 3, 2, 1];
// 外层控制趟数, 一趟可以比出一个最大值, 7个数, 比6趟即可
for (var j = 0; j < arr.length-1; j++) {
  // 在每趟排列前, 先假设, 已经排好了
  var flag = true;
  for (var i = 0; i < arr.length-1-j; i++) {
    if (arr[i] > arr[i + 1]) {
      var temp = arr[i];
      arr[i] = arr[i + 1];
      arr[i+1] = temp;
      // 发生了交换, 说明没有排好
      flag = false;
    }
  }
  // 一趟下来, 如果一次交换都没有发生, flag值就是 true
  if (flag) {
    break; 
  }
}
console.log(arr);
```

## 15、函数（可以将一段重复的代码进行封装，一次声明，可以多次调用）
**好处**：可维护性高

### 15.1 声明函数
**函数名的规范**：一般都是动词+名词（一般函数都是要做某一件事情）
```
function 函数名(){
  函数体;
}
```
> 函数光声明，是不会执行的。
	
#### 15.1.1 调用函数
```
函数();
```

函数一次声明，可以多次调用！！！

#### 15.1.2 函数的参数（在函数中，需要发生变化的值，可以定义成函数的参数）
* 形参---形式参数（函数在声明时，函数名括号内的参数）
	形参默认没有值或类型！！！只在函数调用时，形参才会有具体的值或类型
	作用：占位置

* 实参---实际参数（函数调用时传递给函数的参数）
	实参有具体的值或者类型
	作用：在函数调用时将数据传递给形参

> 注意点：形参和实参一一对应！！！

#### 15.1.3 函数的声明和调用进阶写法
```javascript
function 函数名（形参1，形参2，形参3...）{
  函数体;
}
```

#### 15.1.4 函数的返回值
函数内部声明的变量 或者 形参，只能在函数内使用，出了函数就用不了了
如果希望函数的执行，有结果，需要通过``return``返回内容

> 函数的三要素---决定了一个函数怎么去使用
> 1. ``函数名``：一个函数一次声明，可以多次调用（规范：动词+名词）
> 2. ``函数参数``：可有可无，但是如果有需要变化的值，一般需要提取成形参
> 3. ``返回值``：可有可无，但是如果需要拿到函数的执行结果，就必须要return

#### 15.1.5 函数参数与返回值的说明
* 开发的时候，函数的参数要一一对应
  1. 传递的参数，如果少了，没接收到值的形参，值就是undefined（数字和undefined加起来就是NaN）
  2. 传递的参数，如果多了，多传的参数，会被忽略

* 返回值的问题
	1.return的值，就是函数的执行结果
	2.return后面的代码，不执行了！return表示函数的结束！！

#### 15.1.6 函数调试说明
* 函数可以在内部调用函数
* F10 让代码往下一步执行，如果遇到了函数调用，会跳过函数的执行过程，直接看结果
* F11让代码往下一步执行，如果遇到了函数调用，会进入函数一步步执行
* shift + F11 跳出函数的执行（将当前函数的调用的剩余代码全部执行完，直接看结果）


#### 15.1.7 声明函数的两种方式
1. ``函数声明式``（可以先调用，后声明---预解析，会提升函数的声明）
```javascript
function fn( ) {
  console.log('嘿嘿');
}
```

2. ``函数表达式``（只可以先声明赋值，后调用）---写法相对严格
```
var fn = function( ){
  console.log('哈哈');
};
```

### 15.2 匿名函数（没有名字的函数，不能直接使用）
使用场景：

1. 函数表达式 var fn = function(){...}
2. 匿名函数自调用（自执行)
	> 直接自调用会报错，可以给整个函数包一个( ),包成了一个整体，就可以调用了
  ```javascript
  (function ( ) {
    console.log(123);
  })( );
  ```

**沙箱模式--匿名函数自调用的应用=>可以用于解决全局变量污染问题**
由于全局变量，可以在任何地方都可以访问，所以不能乱用
一般都会用函数自调用包裹起来
```
（function() {...}）
```

## 16、对象（如果需要描述现实生活对象的特征和行为，就需要js中的对象）
### 16.1 概念：无序的键值对的集合

### 16.2 创建对象的方式

1. 字面量 
1 ‘abc’ true undefined null [ ] { }
``var obj = { }``创建空对象

键值对的集合，多个键值对通过"，"隔开
特征：对象的属性
行为：对象的方法---对象的函数

2. 构造函数的方式（了解）
var obj = new Object({...});
			
### 16.3 取值和赋值（点语法）
* 取值：
语法：对象名.属性名  对象名.方法名( );---方法的调用，得到整个函数
 * 有这个属性名，返回对应值
 * 如果没有这个属性名，返回undefined
			          
* 赋值：
语法：对象名.属性名 = '新的值'
 * 如果有这个属性名，覆盖
 * 如果没有这个属性名，新建一个属性并赋值


#### 16.3.1 点语法（简洁方便，不支持变量）
对象名.属性名

#### 16.3.2 中括号语法（支持字符串或变量，更加的灵活）
对象的取值：对象名['属性名']
对象的赋值：对象名['属性名'] = "新的值"
**只要访问对象属性时，需要用到变量，只能用中括号语法**
	
### 16.4 对象的遍历（访问对象的所有属性）
固定语法 （key就是键，属性名）
```javascript
for （var key in obj）{
  console.log(key);
  console.log(obj[key]);---打印属性值
}
```

### 16.5 批量创建对象（将来网站用户，不可能只有一个，就需要批量创建对象）
设计模式（工厂模式、单例模式、观察者模式）
在对象的方法中，this指代当前对象（对象名）
#### 16.5.1 工厂函数（可以将创建对象的代码封装在一个函数中，这个函数就叫工厂函数）
通过工厂函数创建的对象，没有具体的类型，都是Object
```javascript
function Student(sno,name,gender,major){
    obj=new Object();
    obj.sno=sno;
    obj.name=name;
    obj.gender=gender;
    obj.major=major;
    obj.say=function(){
      console.log("hello!");
    }
    return obj;
}
var s=new Student("123","张三","男","数学");
for(var i in s){
    document.write(i+" : "+s[i]+"<br>");
}
```
这里Student()函数相当于一个工厂，在其内部生产对象。这种方式简化了代码，但是无法细分对象，也会造成共有方法和属性的内存浪费。无法细分对象就是生产出的对象的constructor是Object，而不是自定义的
```
console.log(s.constructor);
```
![](https://img-blog.csdnimg.cn/20200831171959364.png#)
内存浪费就是因为方法声明在了对象的内部，每一个对象都有同一个方法，造成了浪费。

#### 16.5.2 构造函数（给新建的对象，添加属性和方法----实例化对象）
1. 就是一个函数
2. 首字母大写
3. js中，内置了一些构造函数，比如：``Object``，``Array``
4. 构造函数可以自定义

* 构造函数的使用步骤
 * 自己声明一个构造函数
 * 结合new一起使用，创建一个有类型的对象
```javascript
function Student(sno,name,gender,major){
    this.sno=sno;
    this.name=name;
    this.gender=gender;
    this.major=major;
    this.say=function(){
        console.log("hello!");
    }
}
var s=new Student("1234","李四","男","物理");
for(var i in s){
    document.write(i+" : "+s[i]+"<br>");
}
```
这种模式解决了对象不能细分问题，此时s的constructor就是Student()构造函数了。
```
console.log(s.constructor);
```
![](https://img-blog.csdnimg.cn/20200831184528158.png#)
但是还是没有解决公有方法造成的内存浪费问题。

#### 16.5.3 构造函数+原型对象模式
```javascript
function Student(sno,name,gender,major){
    this.sno=sno;
    this.name=name;
    this.gender=gender;
    this.major=major;
}
Student.prototype.say=function(){
    console.log("hello!");
}
var s=new Student("12345","王五","男","化学");
for(var i in s){
    document.write(i+" : "+s[i]+"<br>");
}
```
这种模式和构造函模式类似，不同点是该模式把公有的属性和方法声明到构造函数的原型对象中，解决了内存浪费。

#### 16.5.4 new的作用（创建对象）
 * 会新建一个对象，指定对象的类型
 * 让构造函数的this，指向新创建的对象----this.name = '张三';
 * 执行构造函数（给新建的对象，添加属性和方法）
 * 将新创建的对象返回


### 16.6 内置对象（js语法中，内置的一些对象，提供了很多的属性和方法，可以直接用）

#### 16.6.1 Math对象（提供了一系列和数学相关的属性和方法）
1. PI Math.PI
2. 求最大最小值 max min
  var max = Math.max(3,5,6,110);
  console.log(max);
3. 取整 ceil floor round
  （1）ceil向上取整，取大的那个数---天花板函数
  （2）floor向下取整，取小的那个数---地板函数
  （3）round四舍五入，离哪个近取哪个
4. 随机数random ``[0,1)``可以取到0，取不到1
  公式：求一个整数范围0~N，parseInt(Math.random()*(N+1))
5. 绝对值abs
6. 求次方pow
7. 求开方sqrt


#### 16.6.2 Date日期对象

js中提供的Date构造函数，可以创建日期对象

1. 如何创建日期对象
```javascript
  var now = new Date(); // 构造函数不传参，创建的是当前时间
  var date = new Date('2019-4-22 16:00:00'); // 构造函数传日期字符串，指定具体的日期
```

2. 日期格式化 （不用）---一般日期格式都是自定义的
```javascript
  var now = new Date();---当前时间
  console.log(now.toString()); // 让日期以标准化的日期字符串格式化输出,'Thu Apr 08 2021 11:13:41 GMT+0800 (GMT+08:00)'
  console.log(now.toLocaleString()); // 本地化日期字符串格式输出,'2021-4-8 11:13:41'
  console.log(now.toLocaleDateString()); //只显示日期,'2021-4-8'
  console.log(now.toLocaleTimeString()); //只显示时间,'11:13:41'
```

3. 日期格式的自定义，xx年xx月xx日--获取日期里面的各个组成部分
可以封装一个函数，专门给小于10的数，前面加上0
```javascript
function addZero（n）{
  if(n<10) {
    return '0' + n;
  }
  else {
    return n;
  }
}
var now = new Date(); // 当前时间
var year = now.getFullYear(); // 年
var month = now.getMonth()+1; // 获取月，getMonth从0开始，范围0-11
month = addZero(month);
// 获取日getDate
// 获取一周中的第几天，getDay,范围0-6，周日0，周一1
var hours = now.getHours(); // 时，getHours
var minutes = now.getMinutes(); // 分，getMinutes
var seconds = now.getSeconds(); // 秒，getSeconds
// now.getMilliseconds毫秒
```

4. 时间戳（就是数字格式的日期，便于运算，一般用于求时间差）---距离1970年1月1日 0时0分0秒，所过去的毫秒数
```javascript
  var now = new Date();
  console.log(+now);
```

**应用**
* 统计一段代码的执行时间---性能优化
```javascript
var begin = new Date();
var end = new Date();
console.log(end - begin);
```
* 用于秒杀倒计时

## 17、数组对象Array

1. ``.join(分隔符)``：将数组中的值拼接成一个字符串，返回这个字符串
默认分隔符" , "

2. 数组的增删操作``push`` ``pop`` ``unshift``  ``shift`` ***会更改原数组***
  .push在数组的最后面，添加一个或多个项，返回数组的长度
  .pop在数组的最后面，删除一个项，返回删除的项
  .unshift在数组的最前面，添加一个或多个项，返回数组的长度
  .shift在数组的最前面，删除一项，返回删除的项

3. 翻转``reverse`` ***会更改原数组***
4. 排序``sort``（默认按照字符串的方式进行排序，先比较第一个字符）***会更改原数组***
	**如果要制定排序规则，是需要传参的（参数：一个函数）** 
  ```javascript
  arr.sort(function(a,b) {
    return a-b;---从小往大
    return b-a;---从大往小
  })
  ```
  > a表示前一项，b表示后一项
  > 函数的返回值>0,a和b交换位置
  > 函数的返回值=0，不换位置
  > 函数的返回值<0,不换位置
	
5. 合并 ``arr.concat(arr2,arr3....)``，返回合并后的新数组
6. 截取``arr.slice``（从数组中，截取一部分出来，返回一个新数组）
	* arr.slice();从开始一直截取到最后（将整个数组截取，复制一份）
	* arr.slice(begin)从begin（下标）开始一直截取到最后	
	* arr.slice(begin,end)从begin（下标）开始一直截取到end结束（**包括begin,不包括end**）

7. ``splice``方法，可以在数组的任意位置添加、删除、替换任意项---会更改原数组
  ```
	splice（从哪开始删，删几个，添加的项1，添加的项2...）
	splice(begin,deleteCounts,item1,item2...)
  ```

8. ``indexOf``查找值在数组中第一次出现的下标（可以用来查重）
	``lastIndexOf``查找值在数组中最后一次出现的下标
如果值在数组中不存在，返回 ``-1``
	
9. 清空一个数组
```javascript
arr = [ ];
arr.length = 0;
arr.splice(0,arr.length)
```


## 18、基本包装类型（解释了简单数据类型，为什么可以直接使用属性和方法）

**js内部，对于除了undefined和null的简单数据类型，都提供了一个对应的复杂数据类型**

简答数据类型：没有属性和方法，只有值
复杂数据类型：可以有多个属性和方法
	
* 在js中，如果简单数据类型，在访问复杂数据类型的属性或方法时
* 为了方便，自动将简单数据类型，包装成复杂数据类型，然后获取对应的值
* 会将值变回简单数据类型	
	
### 18.1 Number
	通过new Number()创建的对象，``toString``   ``toFixed``两个方法
	num.toFixed(3) 保留3位小数

### 18.2 Boolean	
		通过new Boolean()创建的对象，toString

### 18.3 String
1. 字符串可以和数组一样，进行遍历，字符串不是数组，不能混用方法
2. ``indexOf``和``lastIndexOf`` 查找值在字符串中第一次/最后一次出现的下标
3. ``trim``去除字符串``首尾``的空格
4. 转大小写
 ``toUpperCase``转大写
 ``toLowerCase``转小写
5. 拼接``.concat``将字符串进行拼接，返回新的字符串）
  一般用``+``
6. 截取
  * slice(begin,end) 
    > 从begin开始截取，截取到end结束，包括begin，不包括end
    > begin（必需）：规定从何处开始选取。如果是负数，那么它规定从字符串尾部开始算起的位置。也就是说，-1 指最后一个字符，-2 指倒数第二个字符，以此类推。
    > end（可选）：规定从何处结束选取，即结束处的字符下标。如果没有指定该参数，那么截取的字符串包含从 start 到结束的所有字符。如果这个参数是负数，那么它规定的是从数组尾部开始算起的字符。
    ```javascript
      var str = "0123456789";
      console.log("原始字符串：", str);
      console.log("从索引为3的字符起一直到结束：", str.slice(3));  //3456789
      console.log("从倒数第3个字符起一直到结束：", str.slice(-3));  //789
      console.log("从开始一直到索引为5的前一个字符：", str.slice(0,5));  //01234
      console.log("从开始一直到倒数第3个字符的前一个字符：", str.slice(0,-3));  //0123456
      console.log("从索引为3的字符起到索引为5的前一个字符：", str.slice(3,5));  //34
      console.log("从索引为3的字符起到倒数第3个字符的前一个字符：", str.slice(3,-3));  //3456
    ```
  * substring(begin,end) 从begin开始截取，截取到end结束，包括begin，不包括end
    ```javascript
      var str = "0123456789";
      console.log("原始字符串：", str);
      console.log("从索引为3的字符起一直到结束：", str.substring(3));  //3456789
      console.log("从索引为20的字符起一直到结束：", str.substring(20));  //
      console.log("从索引为3的字符起到索引为5的前一个字符结束：", str.substring(3,5));  //34
      console.log("start比end大会自动交换，结果同上：", str.substring(5,3));  //34
      console.log("从索引为3的字符起到索引为20的前一个字符结束：", str.substring(3,20));  //3456789
      console.log("substring将负数转化为0:", str.substring(-3)); //
    ```
  * substr(begin,length) 从begin开始截取，截取length个
    ```javascript
      var str = "0123456789";
      console.log("原始字符串：", str);
      console.log("从索引为3的字符起一直到结束：", str.substr(3));  //3456789
      console.log("从索引为20的字符起一直到结束：", str.substr(20));  //
      console.log("从索引为3的字符起截取长度为5的字符串：", str.substr(3,5));  //34567
      console.log("从索引为3的字符起截取长度为20的字符串：", str.substr(3,20));  //3456789
    ```
  * 三者比较
  ```javascript
    var str = '中华人民共和国万岁'
    // 一，start为负数，end不传
    str.slice(-3) // '国万岁'
    str.substring(-3) // '中华人民共和国万岁'
    str.substr(-3) // '国万岁'
    // 说明： 1，slice与substr第一个参数为负数时，将从字符串反方向开始计数，末位记为-1,等同于str.slice(6)，str.substr(6)
    //       2，substring将负数转化为0，既str.substring(0)

    // 二，start为负数，end为负数时
    str.slice(-3,-1) // '国万'
    str.substring(-3,-1) // ''
    str.substr(-3,-1) // ''
    // 说明：
    //    1，slice正常截取字符串，相比substring灵活很多
    //    2，substring将所有参数转化为0，既str.substring(0,0)
    //    3，substr end参数不能为负数

    // 三，start与end均大于零，且start > end
    str.slice(5,3) // ''
    str.substring(5,3) // '民共'
    str.substr(5,3) // '和国万'
    // 说明：
    //   1，substr正常截取字符串，代表从第五位开始，截取字符串长度为3
    //   2，当start>end时，substring在提取子串之前会先交换这两个参数，既转换为substring(3,5)；而slice不能进行此转换，所以截取的为空字符串

    // 四，start与end均大于零，且start < end
    str.slice(3,5) //'民共'
    str.substring(3,5) // '民共'
    str.substr(3,5) // '民共和国万' 
    // 说明：三种方法均正常截取字符串，只是substr第二个参数含义不同，代表截取的字符串长度而不是终止位置。

  ```
7. 通过.split(分隔符):可以将字符串拆分成数组，会返回一个拆分得到的数组
    通过join:可以将数组的值，拼成字符串，会返回一个拼接成的字符串

8. replace替换，会返回替换的结果
```javascript
str.replace('aa','bb'); // 将字符串中的第一个aa替换成bb
```

想要全部替换，需要用到正则``str.replace(/aa/g,'bb')``;

	
## 19、值类型和引用类型
从内存的存储角度，分成了``值类型``和``引用类型``（内存是可以释放，可以重复利用的）

**值类型（简单数据类型）**：存储在变量中，存的是值本身
**引用类型（复杂数据类型）**：存储在变量中，存的是``内存地址``----会单独在内存中开辟一块空间存储

### 19.1 值类型和引用类型的赋值类型
* 值类型：存储在变量中 ，存的是``值本身``，所以在赋值给其他变量时，赋值的是``值本身``
* 引用类型：存储在变量中，存的是``内存地址``，所以在赋值给其他变量时，赋值的是``内存地址``

### 19.2 值类型和引用类型的值传递（让值作为函数的参数进行传递）
* 值类型：存储在变量中，存的是值本身，所以在值传递时，传的也是值本身
* 引用类型：存储在变量中，存的是内存地址，所以在值传递时，传的也是内存地址
	
		

				
## 20、typeof 关键字
1. typeof获取简单数据类型，可以直接返回对应的类型
***特例***：typeof null  返回object
2. typeof获取复杂数据类型，一般返回object
***特例***：typeof 函数  返回function    函数是js的一等公民

## 21、逻辑中断（短路运算）&& ||
&&找假值，只要遇到了假值，就中断短路（后面的不看了）
||找真值，只要看到了真值，就中断短路（后面的不看了）

```javascript
function demo(fn){
  fn&&fn(); //fn存在，才去调用
}	
demo(function(){
  console.log(111);
});
demo();
```
	
**逻辑``或``，一般可以用于设置默认值,也可以解决兼容性**
```javascript
function getSum(a,b){
  a=a || 0;
  b=b || 0;
  var sum = a+b;
  return sum;
}
```


## 22、如何拷贝一个对象
封装一个方法，可以拷贝一个对象，返回（浅拷贝，只拷贝了 一层）
```javascript
function copy(obj){
  var newObj = {};
  for (var k in obj){
    newObj[k] = obj[k];
  }
}
```

* 浅拷贝，只拷贝了一层，如果全是简单类型的属性，没有问题
	但是如果有复杂类型的属性（对象），此时拷贝的只是地址，需要处理的
* 深拷贝
```
	newObj[k] = typeof obj[k] === 'object' ? copy(obj[k]) : obj[k];
```

```javascript
// 1.遍历+递归
function deepClone(obj){
  let newObj =  {}
  for(let key in obj){
      if(obj.hasOwnProperty(key)){
          if(typeof(obj[key]) === 'object' && obj[key] !== null){
            newObj[key] = (Array.isArray(obj[key])  ? [] : {})
            newObj[key] = (Array.isArray(newObj[key]) ? [...obj[key]] : deepClone(obj[key]));  
          }else{
              newObj[key] = obj[key];
          }
      }
  }
  return newObj;
}

// 2.Object.assign + 递归
function deepClone(obj){
  let newObj = {}
  for(let key in obj){
    if(obj.hasOwnProperty(key)){
      newObj[key] = (typeof obj[key] === 'object' ? Object.assign(obj[key]) : obj[key])
    }
  }
  return newObj
}

// 3.对象扩展+递归
function deepClone(obj){
  var newObj ={}
  for(let key in obj){
    if(obj.hasOwnProperty(key)){
      if(typeof obj[key] === 'object'){
        newObj[key] = (Array.isArray(obj[key]) ? [] : {})
        newObj[key] = (Array.isArray(newObj[key]) ? [...obj[key]] : {...obj[key]}) 
      } 
      else{
        newObj[key] = obj[key]
      }
    }
  }
  return newObj
}

// 4.JSON复制（对NaN和undefined无法正确复制，会丢失）
function deepClone(obj) {
  var newObj = JSON.parse(JSON.stringify(obj))
  return newObj
}
```


## 23、面向过程：注重点是过程和步骤，亲力亲为，事无巨细的！！
## 24、面向对象：关注的是找一个对象，去做某件事
***面向对象不是面向过程的替代，是面向过程的封装***
### 24.1 特性：
1. 封装性
2. 继承性
3. 多态性（js中没有）
	
## 25、原型

JavaScript是一门基于原型的语言，在软件设计模式中，有一种模式叫做原型模式，JavaScript正是利用这种模式而被创建出来

### 25.1 原型模式 
原型模式是用于创建重复的对象，同时又能保证性能，这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。
原型模式的``目的``是用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象，也就是说利用已有的一个原型对象，可以快速地生成和原型对象一样的新对象实例

### 25.2 原型：一个可以被复制（或者叫克隆）的一个类，通过复制原型可以创建一个一模一样的新对象，也可以说原型就是一个模板，在设计语言中更准确的说是一个对象模板
1. 原型是定义了一些公用的属性和方法，利用原型创建出来的新对象实例会共享原型的所有属性和方法
实例代码：
```javascript
// 创建原型
var Person = function(name){
    this.name = name;
};

// 原型的方法
Person.prototype.sayHello = function(){
    console.log(this.name+",hello");
};

// 实例化创建新的原型对象，新的原型对象会共享原型的属性和方法
var person1 = new Person("zhangsan");
var person2 = new Person("lisi");

// zhangsan,hello
person1.sayHello();
// lisi,hello
person2.sayHello();
```

2. 严格模式下，原型的属性和方法还是会被原型实例所共享的
实例代码：
```javascript
// 开启严格模式，原型的属性和方法还是会被原型实例所共享的
"use strict";

// 创建原型
var Person = function(name){
    this.name = name;
};

// 原型的方法
Person.prototype.sayHello = function(){
    console.log(this.name+",hello");
};

// 实例化创建新的原型对象，新的原型对象会共享原型的属性和方法
var person1 = new Person("zhangsan");
var person2 = new Person("lisi");

// zhangsan,hello
person1.sayHello();
// lisi,hello
person2.sayHello();
```

3. 通过原型创建的新对象实例是相互独立的，为新对象实例添加的方法只有该实例拥有这个方法，其它实例是没有这个方法的
实例代码：
```javascript
// 创建原型
var Person = function(name){
  this.name = name;
};

// 原型的方法
Person.prototype.sayHello = function(){
  console.log(this.name+",hello");
};

// 实例化创建新的原型对象，新的原型对象会共享原型的属性和方法
var person1 = new Person("zhangsan");
var person2 = new Person("lisi");

// zhangsan,hello
person1.sayHello();
// lisi,hello
person2.sayHello();


// 为新对象实例添加方法
// 通过原型创建的新对象实例是相互独立的
person1.getName = function(){
    console.log(this.name);
}

// zhangsan
person1.getName();
// Uncaught TypeError: person2.getName is not a function
person2.getName();
```

4. 原型的总结：

* 所有引用类型都有一个__proto__(隐式原型)属性，属性值是一个普通的对象
* 所有函数都有一个prototype(原型)属性，属性值是一个普通的对象
* 所有引用类型的__proto__属性指向它构造函数的prototype

5. 函数的原型prototype：函数才有prototype，prototype是一个对象，指向了当前构造函数的引用地址
6. 函数的原型对象__proto__：所有对象都有__proto__属性， 当用构造函数实例化（new）一个对象时，会将新对象的__proto__属性指向 构造函数的prototype

7. 原型对象和函数的原型的关系
![原型对象和函数的原型的关系](https://img-blog.csdnimg.cn/20190623221321362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxNDA4MA==,size_16,color_FFFFFF,t_70)

> 说明：
> * 所有函数的__proto__都是指向Function的prototype
> * 构造函数new出来的对象__proto__指向构造函数的prototype
> * 非构造函数实例化出的对象或者对象的prototype的__proto__指向Object的prototype
>   Object的prototype指向null

8. 所有的原型对象都会自动获得一个``constructor``（构造函数）属性，这个属性（是一个指针）指向 prototype 属性所在的函数（Person）
9. 实例的构造函数属性（constructor）指向构造函数 ：person1.constructor == Person
10. 原型对象（Person.prototype）是 构造函数（Person）的一个实例
11. 原型的分类：
* ``隐式原型（_proto_）``：上面说的这个原型是JavaScript中的内置属性[[prototype]]，此属性继承自object对象，在脚本中没有标准的方式访问[[prototype]]，但Firefox、Safari和Chrome在每个对象上都支持一个属性_proto_，隐式原型的作用是用来构成原型链，实现基于原型的继承
* ``显示原型``（prototype）``：每一个函数在创建之后，便会拥有一个prototype属性，这个属性指向函数的原型对象，显示原型的作用是用来实现基于原型的继承与属性的共享
12. 原型的使用方式：
通过给Calculator对象的prototype属性赋值对象字面量来设定Calculator对象的原型
在赋值原型prototype的时候使用function立即执行的表达式来赋值，可以封装私有的function，通过return的形式暴露出简单的使用名称，以达到public/private的效果

#### 25.2.1 原型链
原型链是原型对象创建过程的历史记录，当访问一个对象的某个属性时，会先在这个对象本身属性上查找，如果没有找到，则会去它的__proto__隐式原型上查找，即它的构造函数的prototype，如果还没有找到就会再在构造函数的prototype的__proto__中查找，这样一层一层向上查找就会形成一个链式结构

#### 25.2.2 原型设计的问题
当查找一个对象的属性时，JavaScript 会根据原型链向上遍历对象的原型，直到找到给定名称的属性为止，直到到达原型链的顶部仍然没有找到指定的属性，就会返回 undefined
也可以理解为原型链继承时查找属性的过程是先查找自身属性，当自身属性不存在时，会在原型链中逐级查找

#### 25.2.3 hasOwnProperty 函数：
可以用来检查对象自身是否含有某个属性，返回值是布尔值，当属性不存在时不会向上查找对象原型链，*hasOwnProperty是 JavaScript 中``唯一一个``处理属性但是不查找原型链的函数*

#### 25.2.4 getOwnPropertyNames 函数
可以获取对象所有的自身属性，返回值是由对象自身属性名称组成的数组，同样不会向上查找对象原型链

#### 25.2.5 原型链的小结：
一直往上层查找，直到到null还没有找到，则返回undefined
Object.prototype.__proto__ === null
所有从原型或更高级原型中的得到、执行的方法，其中的this在执行时，指向当前这个触发事件执行的对象

6）JavaScript的原型是为了实现对象间的联系，解决构造函数无法数据共享而引入的一个属性，而原型链是一个实现对象间联系即继承的主要方法

#### 25.2.6 常见面试题

* 谈谈你对原型的理解？
在 JavaScript 中，每当定义一个对象（函数也是对象）时候，对象中都会包含一些预定义的属性。其中每个函数对象都有一个prototype 属性，这个属性指向函数的原型对象，使用原型对象的好处是所有对象实例共享它所包含的属性和方法

* 什么是原型链？原型链解决的是什么问题？
 1. 原型链解决的主要是继承问题
 2. 每个对象拥有一个原型对象，通过 proto 指针指向其原型对象，并从中继承方法和属性，同时原型对象也可能拥有原型，这样一层一层，最终指向 null(Object.proptotype.__proto__指向的是null)。这种关系被称为原型链(prototype chain)，通过原型链一个对象可以拥有定义在其他对象中的属性和方法
 3. 构造函数 Parent、Parent.prototype 和 实例 p 的关系如下:(p.__proto__ === Parent.prototype)
![](https://img-blog.csdnimg.cn/20190623221912165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxNDA4MA==,size_16,color_FFFFFF,t_70)

* prototype 和 proto 区别是什么？
 1. prototype是构造函数的属性
 2. __proto__是每个实例都有的属性，可以访问 [[prototype]] 属性
 3. 实例的__proto__与其构造函数的prototype指向的是同一个对象

### 25.3 补充（原先的）
#### 25.3.1 属性搜索原则：自己有就访问自己的，自己没有，去原型链中就近查找
1. 如果自己有这个属性，就访问自己的
2. 如果自己没有这个属性，会到原型中找，如果找到，不找了，直接返回
3. 如果原型中也没有，会到原型的原型中找...
4. 一直找到Object.prototype，如果老祖宗也没有，返回undefined

#### 25.3.2 设置属性：
如果有这个属性，直接修改赋值
如果自己没有这个属性，给自己添加一个新属性（不会改到原型）

#### 25.3.3 Object.prototype的成员
1. ``hasOwnProperty``
	语法：``对象.hasOwnProperty('属性名')`` ---判断属性是否是自己的，而不是原型的（不是继承来的）
	for in 遍历，不仅自己的属性可以遍历，原型上的属性也会遍历到（对于constructor等浏览器内置的属性，被浏览器进行来了处理，不会被for in便利出来）
	遍历时，只打印自身的（用hasOwnProperty进行判断）
	in操作符：判断属性是否可以被对象所访问（只要能够访问到这个属性，就返回true）---这个不是成员
	语法：'属性名' in 对象

2. ``A.isPrototypeOf(B)``:判断A是不是B的原型
	
3. ``对象.propertyIsEnumerable('属性名')`` 判断属性是否可以遍历（可枚举）
4. toSting()
5. valueof()

6. instanceof运算符
	语法：A instanceof B（判断A是否是B的实例，B构造函数）
	进阶原理：判断B.prototype在不在A的原型链上
	使用typeof无法区分具体的对象类型！！！

```javascript
console.log(p.constructor.name)---可以用来打印类型
Object.prototype.toString.call([]) ---可以用来获取复杂数据是什么类型 ---[object Array]
```


## 26、js数据类型
堆和栈（只是将Java中概念，拿过来类比了）
js中没有特别明确的堆和栈的概念，而且js的实现，也不需要堆和栈的概念
java中堆内存和栈内存
1.所有的``简单数据``类型，存在``栈``中
2.所有的``复杂数据``类型，存在``堆``中---真实存在变量中的，也是地址



## 27、this的规则：
xx.fn();---fn函数调用时里面的this，指向调用者（谁调用的，this就指向谁）
1. 如果是直接调用的方法，this指向window
		（1）function fn(){console.log(this)}
		         fn();
		（2）setInterval(function(){console.log(this)},2000)---定时器中的this指向window
2. 如果对象中的方法，被调用了，this指向调用者，谁调用的，指向谁
3. 特例，构造函数中执行的this，this指向新创建的对象

全局的var a = 123;  等价于 window.a = 123;


## 28、继承（拿来主义，自己没有的属性和方法，别人有，拿过来用，就实现了继承）⭐️⭐️⭐️
### 28.1 原型链继承
```javascript
function Person() {
  this.name = 'Hello World';
}
Person.prototype.getName = function() {
  console.log(this.name)
}
function Child() {

}
Child.prototype = new Person()
var child1 = new Child()
child1.getName() // Hello World
```
***重点：***
让新实例的原型等于父类的实例。

***优点：***
实例可继承的属性有：实例的构造函数的属性，父类构造函数属性，父类原型的属性。（新实例不会继承父类实例的 属性！）

***缺点：***
1. 新实例无法向父类构造函数传参。
2. 继承单一。
3. 所有新实例都会共享父类实例的属性。（原型上的属性是共享的，一个实例修改了原型属性，另一个实例的原型属性 也会被修改！）

### 28.2 构造函数继承
```javascript
function Person(){
  this.name = 'xiaoming';
  this.colors = ['red', 'blue', 'green'];
}

Person.prototype.getName = function(){
  console.log(this.name);
}

function Child(age){
  Person.call(this);
  this.age = age
}

var child1 = new Child(23);
var child2 = new Child(12);
child1.colors.push('yellow');
console.log(child1.name); // xiaoming
console.log(child1.colors); // ["red", "blue", "green", "yellow"]
console.log(child2.colors); // ["red", "blue", "green"]
```

***重点：***
用.call()和.apply()将父类构造函数引入子类函数（在子类 函数中做了父类函数的自执行（复制））

***优点：***
 1. 只继承了父类构造函数的属性，没有继承父类原型的属性。
 2. 解决了原型链继承缺点1、2、3。
 3. 可以继承多个构造函数属性（call多个）。
 4. 在子实例中可向父实例传参。

***缺点：***
 1. 只能继承父类构造函数的属性。
 2. 无法实现构造函数的复用。（每次用每次都要重新调用）
 3. 每个新实例都有父类构造函数的副本，臃肿。

### 28.3 组合继承（组合原型链继承和借用构造函数继承）（常用）
```javascript
function Parent(name){
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function(){
    console.log(this.name);
}

function Child(name,age){
    Parent.call(this,name);// 第二次调用 Parent()
    this.age = age;
}

Child.prototype = new Parent(); // 第一次调用 Parent()

var child1 = new Child('xiaopao',18);
var child2 = new Child('lulu',19);
```

***重点：***
结合了两种模式的优点，传参和复用

***优点：***
1. 可以继承父类原型上的属性，可以传参，可复用。
2、每个新实例引入的构造函数属性是私有的。

***缺点：***
调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。

### 28.4 原型式继承
```javascript
function CreateObj(o){
    function F(){}
    F.prototype = o;
    console.log(o.__proto__ === Object.prototype);
    console.log(F.prototype.constructor === Object); // true
    return new F();
}

var person = {
    name: 'xiaopao',
    friend: ['daisy','kelly']
}

var person1 = CreateObj(person);

// var person2 = CreateObj(person);

person1.name = 'person1';
// console.log(person2.name); // xiaopao
person1.friend.push('taylor');
// console.log(person2.friend); // ["daisy", "kelly", "taylor"]
// console.log(person); // {name: "xiaopao", friend: Array(3)}
person1.friend = ['lulu'];
// console.log(person1.friend); // ["lulu"]
// console.log(person.friend); //  ["daisy", "kelly", "taylor"]
// 注意： 这里修改了person1.name的值，person2.name的值并未改变，并不是因为person1和person2有独立的name值，而是person1.name='person1'是给person1添加了name值，并非修改了原型上的name值
// 因为我们找对象上的属性时，总是先找实例上对象，没有找到的话再去原型对象上的属性。实例对象和原型对象上如果有同名属性，总是先取实例对象上的值
```

***重点：***
用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。object.create()就是这个原理。

***优点：***
类似于复制一个对象，用函数来包装。

***缺点：***
1. 所有实例都会继承原型上的属性。
2. 无法实现复用。（新实例属性都是后面添加的）

### 28.5 寄生式继承
```javascript
var ob = {
    name: 'xiaopao',
    friends: ['lulu','huahua']
}

function CreateObj(o){
    function F(){};  // 创建一个构造函数F
    F.prototype = o;
    return new F();
}

// 上面CreateObj函数 在ECMAScript5 有了一新的规范写法，Object.create(ob) 效果是一样的 , 看下面代码
var ob1 = CreateObj(ob);
var ob2 = Object.create(ob);
console.log(ob1.name); // xiaopao
console.log(ob2.name); // xiaopao

function CreateChild(o){
    var newob = CreateObj(o); // 创建对象 或者用 var newob = Object.create(ob)
    newob.sayName = function(){ // 增强对象
        console.log(this.name);
    }
    return newob; // 指定对象
}

var p1 = CreateChild(ob);
p1.sayName(); // xiaopao 
```
***重点：***
就是给原型式继承外面套了个壳子。

***优点：***
没有创建自定义类型，因为只是套了个壳子返回对象（这个），这个函数顺理成章就成了创建的新对象。

***缺点：***
没用到原型，无法复用。

### 28.6 寄生组合式继承（常用）
```javascript
function Parent(name){
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.sayName = function(){
    console.log(this.name);
}

function Child(name,age){
    Parent.call(this,name); 
    this.age = age;
}

function CreateObj(o){
    function F(){};
    F.prototype = o;
    return new F();
}

// Child.prototype = new Parent(); // 这里换成下面
function prototype(child,parent){
    var prototype = CreateObj(parent.prototype);
    prototype.constructor = child;
    child.prototype = prototype;
}
prototype(Child,Parent);

var child1 = new Child('xiaopao', 18);
console.log(child1); 
```
***重点：***
修复了组合继承的问题

### 28.7 class继承
```javascript
class Parent5 {
  constructor() {
    this.name = ['super5']
  }
  reName() {
    this.name.push('new 5')
  }
}
class Child5 extends Parent5 {
  constructor() {
    super()
  }
}
var child51 = new Child5()
var child52 = new Child5()
```

## 29、定义函数的三种方式
### 29.1 函数声明式
```
function fn(){}
fn();
```
### 29.2 函数表达式
```
var fn = function(){};
fn();
```
### 29.3 构造函数的方式 
```
new Function(...)
```
作用：可以直接执行字符串
参数：都是字符串类型，最后一个参数是函数体，前面其他所有参数是定义的形参
		 只有一个参数，就会当成函数体
```javascript
var fn = new Function('a' , 'b' , 'console.log(a+b)');
fn(1,2);
```

## 30、try和catch
	
* try和catch必须一起使用
*	try表示尝试执行某段代码，就算发生了错误，js也会继续执行
*	catch，只要try中代码发生了错误，就会执行catch中的代码
*	finally:不管try中的代码执行是否成功，都会执行finally的代码
```javascript
	try {  ----尝试执行某段代码
		var fn = new Function(value);
		fn();
	}
	catch（e）{ ---- 抓取，可以抓取到try执行中的错误，可以进行处理，可以不处理
		console.log(e)
	}
	finally{
	}	
```

## 31、eval(str)可以执行字符串，非常强大，但是不用（风险性太大，会造成全局变量的污染）
```javascript
var a = 100;
var str = 'var a = 1; var b = 2; console.log(a + b)';
eval(str); 
```

## 32、四种调用模式

* 函数：指的是普通的，不属于任何对象的函数
* 方法：作为对象的属性存在的函数（对象中的函数）
*	函数内的this指向谁，只跟怎么调用有关系，跟函数定义在什么地方，没有任何关系

### 32.1 函数调用模式
```javascript
fn( );   // this指向window
```    

### 32.2 方法调用模式
```javascript
obj.fn();  // this指向obj，谁调用指向谁
```
> 可以把数组当成一个对象，访问0属性，就是方法，方法调用模式，指向arr

### 32.3 构造函数调用模式
```javascript
var p = new Person();  // this指向实例p---new改变了this的指向，让this指向了新的实例
```
	
### 32.4 上下文调用模式：（this在这边是可以自定义的，想要this指向谁，函数执行时this就指向谁）

1. call
		
 * 任何函数（都可以看成一个特殊的对象，也可以设置属性和方法）都有一个call方法
 * call方法也可以用于调用函数,还可以指定函数执行的this的指向---fn.call(this指向);
 * 如果不传参，默认this指向window，如果传递第一个参数，那么就会指定this的指向
		
 ***使用call来调用函数和普通调用函数的方式，唯一的区别，就是多了一个参数，call的第一个参数用于指定this指向***

``数组``：有着数组的方法，可以遍历
``伪数组``，本质上是对象，不是数组，数组的方法不能直接调用，但是伪数组是可以遍历的
常见的伪数组：``arguments`` ``document.querySelector()``  ``jQuery``

> 小结：学习call方法，fn.call(this指向,x,y,z);
>		   call方法调用函数和普通函数调用的唯一区别，在于多了一个参数，第一个参数，用于指定this
>		   call方法，可以用于函数借调（借用别人的函数，借来调用）
>		   别人.方法.call(自己)；将this改成自己，借用别人的方法

2. apply
		
 * apply的功能和call的功能是一样的，只是调用的方法不同了
 * 每个函数，都有一个apply方法，可以用于调用函数，且可以指定this指向
 * ***apply的语法：fn.apply(this指向，[x,y,z]);***
   * 参数1：用于指定this的指向
   * 参数2：接收一个数组，里面存放着所有传递的参数
	
	> 如果参数比较少，一般使用call，比较简单
	> 如果参数比较多，一般使用apply，可以将所有需要传递的参数，放在一个数组中，一次性传递
	
3. bind（复制一个新函数，并且将新函数的this固定死指向传入的this值）
  ```javascript
		var newFn = fn.bind(this指向)
  ```

4. arguments

* 任何一个函数，都有一个对象，arguments，是一个伪数组，用于收集所有传递的参数（实参）
* 一般用于参数不确定的情况

  new会和最近的 函数名() 结合，是一个整体，new fn()

> 补充：（1）定时器中的this ，指向window
>       (2）事件处理函数中，浏览器让this指向了事件源


			
## 33、函数也是对象
函数也是一种特殊的对象，可以添加属性和方法

## 34、js的规则：
1. 任何``函数``，都是由``Function``创建出来的，包括他自己，包括Object函数
2. 任何的``原型对象``，都是直接由``Object``创建出来的

js中的作用域：词法作用域，静态作用域，函数的作用域在函数声明时，就已经确定好了，跟调用没有关系
作用域链：每个函数都有自己的作用域，如果是定义在函数内的函数，里面的函数又会有自己的作用域（一层套一层，形成了作用域链）
变量的查询规则：先看自己作用域有没有这个变量，如果没有，往外一层一层的就近查找，如果一直找到全局都没有找到，就会报错



## 35、递归函数：在一个函数内部，自己调用自己

函数在调用时，其实是占用内存，函数在调用时，会开辟一块内存，进行执行

**特点：**
1. 自己调用自己
2. 必须要有结束条件，要有出口

*递归：是一种算法，一种思想 ，化归思想，复杂的问题简单化*

### 35.1 斐波那契数列（兔子数列）：从第三个数开始，后面的数等于前两个数的和
```javascript
// 直接这么写会有性能问题
function getFib(n) {
  if (n === 1 || n === 2) {
    return 1;
  }
  return getFib(n-1) + getFib(n-2);
}
```

***为什么会有性能问题？***
一个函数内部，调用了两次自己，真正执行时进行了大量的重复运算

***如何优化：将已经算过的第n个斐波那契数列存起来***
运算过程中：
 1. 先判断这个数，有没有算过，如果算过了，直接用
 2. 如果没有算过，接着自己算，算完，存起来
```javascript
// 找规律: getFib(n) = getFib(n-1) + getFib(n-2);
var arr = [];  // 专门用于存储已经算好的数,  arr[n] = 第n个斐波纳契数
function getFib(n) {
  if (n === 1 || n === 2) {
    return 1;
  }

  if (arr[n]) {
    return arr[n];
  }
  else {
    arr[n] = getFib(n-1) + getFib(n-2);
    return arr[n];
  }
}
```
  
  
***利用闭包解决斐波那契数列***
```javascript
// 省去了外部的函数名, 利用函数自调用

var result = (function() {
  var arr = [];  // 专门用于存储已经算好的数,  arr[n] = 第n个斐波纳契数
  function getFib(n) {
    if (n === 1 || n === 2) {
      return 1;
    }

    if (arr[n]) {
      return arr[n];
    }
    else {
      arr[n] = getFib(n - 1) + getFib(n - 2);
      return arr[n];
    }
  }
  return getFib;
})();
```

## 36、闭包：一个函数的内部，有另一个函数，内部函数引用了外部函数的局部变量，这个模型，称之为闭包
* 作用：保护变量（变量私有化）
*	闭包的内存，不会直接释放（会占用内存）
*	函数执行调用时，必然会开辟一快内存空间，一般来说，执行完就会释放

***闭包的基本模型：***
1. outer里面包inner
2. 匿名函数自调用
```javascript
var result = (function(){
  var count = 0;
  return function(){
    count++;
    console.log(count);
  }
})();
```

## 37、js垃圾回收机制（会将一些不用的内存空间，释放掉）

**内存泄漏：**如果一块内存空间，一直得不到释放，就认为这块内存泄漏了
**机制：**
 1. 引用计数：如果一块空间的引用次数，最终变成0，就会被释放掉
		bug：如果两个对象，互相引用，形成了循环引用，使用引用计数，就会得不到释放（内存泄漏）
 2. 标记清除：如果一块内存空间，可以访问到，就不释放；如果访问不到了，就会释放掉
		闭包用完了，只需要将指向函数内部的引用干掉，此时内存就会释放了

	
## 38、正则表达式：用于匹配规律规则的表达式（经常用于表单校验）

### 38.1 创建正则表达式：
1. 通过构造函数 
```javascript
var reg = new RegExp(//);
```
\d 表示数字，0-9，/\d/可以匹配所有的数字
通过 ``test`` 方法，判断字符串，是否符合正则规则
2. 字面量
```javascript
var reg = /\d/;
```

	
### 38.2 正则-元字符

#### 38.2.1 普通字符：a b c
#### 38.2.2 元字符：有特殊含义的

1. ``\d`` 数字，0-9
2. ``\D`` 非数字
3. ``\w`` 匹配单词字符（字母数字下划线），0-9  a-z  A-Z  _
4. ``\W`` 非\w（只要不是\w中的字符，都匹配）
5. ``\s`` 匹配不可见字符（换行``\n`` ``空格`` ）
6. ``\S`` 匹配可见字符
7. ``.`` 匹配任意字符（除了``\n``）----a.b可以匹配acb  aab a1b......
  有时，就需要匹配点，需要转义 \.-->表示普通的点

### 38.3 正则表达式的优先级
``|`` 表示或，优先级最低
``()`` 优先级最高，一般用于提升优先级

### 38.4 正则表达式的字符
``[ ]``这个位置，可以出现的字符---[abc]，这个位置，可以出现a或者b或者c
``[a-z]``，这个位置，可以出现a-z的所有小写字符
``[a-zA-Z0-9_]``，这个位置，可以出现所有的字母、数字、下划线
[ ]内的 ^ 表示非
	
### 38.5 正则的边界（严格匹配）
``^``必须以...开头
``$`` 必须以...结尾
console.log(/^$/);---严格匹配，严格到字符数都是确定的

### 38.6 正则的量词
``*`` 出现0次或多次
``+`` 出现1次或多次
``?`` 出现0次或1次
``{m,n}`` 表示可以出现m次到n次
``{m,}`` 表示至少出现m次
``{m}`` 出现m次
汉字也是字符的一种，也有范围[\u4e00-u9fa5\]---一 yu
	
### 38.7 正则的replace（不严格的替换）
正则也比较懒，不加参数，只替换一次（加上g，表示 全局替换）
```javascript
str.replace(/aa/g,'xx')
```

## 39、js校正计算
/**
 ** 加法函数，用来得到精确的加法结果
 ** 说明：javascript的加法结果会有误差，在两个浮点数相加的时候会比较明显。这个函数返回较为精确的加法结果。
 ** 调用：accAdd(arg1,arg2)
 ** 返回值：arg1加上arg2的精确结果
 **/
```javascript
function accAdd(arg1, arg2) {
    var r1, r2, m, c;
    try {
        r1 = arg1.toString().split(".")[1].length;
    }
    catch (e) {
        r1 = 0;
    }
    try {
        r2 = arg2.toString().split(".")[1].length;
    }
    catch (e) {
        r2 = 0;
    }
    c = Math.abs(r1 - r2);
    m = Math.pow(10, Math.max(r1, r2));
    if (c > 0) {
        var cm = Math.pow(10, c);
        if (r1 > r2) {
            arg1 = Number(arg1.toString().replace(".", ""));
            arg2 = Number(arg2.toString().replace(".", "")) * cm;
        } else {
            arg1 = Number(arg1.toString().replace(".", "")) * cm;
            arg2 = Number(arg2.toString().replace(".", ""));
        }
    } else {
        arg1 = Number(arg1.toString().replace(".", ""));
        arg2 = Number(arg2.toString().replace(".", ""));
    }
    return (arg1 + arg2) / m;
}
 
//给Number类型增加一个add方法，调用起来更加方便。
Number.prototype.add = function (arg) {
    return accAdd(arg, this);
};
```



/**
 ** 减法函数，用来得到精确的减法结果
 ** 说明：javascript的减法结果会有误差，在两个浮点数相减的时候会比较明显。这个函数返回较为精确的减法结果。
 ** 调用：accSub(arg1,arg2)
 ** 返回值：arg1加上arg2的精确结果
 **/
```javascript
function accSub(arg1, arg2) {
    var r1, r2, m, n;
    try {
        r1 = arg1.toString().split(".")[1].length;
    }
    catch (e) {
        r1 = 0;
    }
    try {
        r2 = arg2.toString().split(".")[1].length;
    }
    catch (e) {
        r2 = 0;
    }
    m = Math.pow(10, Math.max(r1, r2)); //last modify by deeka //动态控制精度长度
    n = (r1 >= r2) ? r1 : r2;
    return ((arg2 * m - arg1 * m) / m).toFixed(n);
}
 
// 给Number类型增加一个mul方法，调用起来更加方便。
Number.prototype.sub = function (arg) {
    return accSub(arg, this);
};
```



/**
 ** 乘法函数，用来得到精确的乘法结果
 ** 说明：javascript的乘法结果会有误差，在两个浮点数相乘的时候会比较明显。这个函数返回较为精确的乘法结果。
 ** 调用：accMul(arg1,arg2)
 ** 返回值：arg1乘以 arg2的精确结果
 **/
```javascript
function accMul(arg1, arg2) {
    var m = 0, s1 = arg1.toString(), s2 = arg2.toString();
    try {
        m += s1.split(".")[1].length;
    }
    catch (e) {
    }
    try {
        m += s2.split(".")[1].length;
    }
    catch (e) {
    }
    return Number(s1.replace(".", "")) * Number(s2.replace(".", "")) / Math.pow(10, m);
}
 
// 给Number类型增加一个mul方法，调用起来更加方便。
Number.prototype.mul = function (arg) {
    return accMul(arg, this);
};
```




/** 
 ** 除法函数，用来得到精确的除法结果
 ** 说明：javascript的除法结果会有误差，在两个浮点数相除的时候会比较明显。这个函数返回较为精确的除法结果。
 ** 调用：accDiv(arg1,arg2)
 ** 返回值：arg1除以arg2的精确结果
 **/
```javascript
function accDiv(arg1, arg2) {
    var t1 = 0, t2 = 0, r1, r2;
    try {
        t1 = arg1.toString().split(".")[1].length;
    }
    catch (e) {
    }
    try {
        t2 = arg2.toString().split(".")[1].length;
    }
    catch (e) {
    }
    with (Math) {
        r1 = Number(arg1.toString().replace(".", ""));
        r2 = Number(arg2.toString().replace(".", ""));
        return (r1 / r2) * pow(10, t2 - t1);
    }
}
 
//给Number类型增加一个div方法，调用起来更加方便。
Number.prototype.div = function (arg) {
    return accDiv(this, arg);
};
```
		

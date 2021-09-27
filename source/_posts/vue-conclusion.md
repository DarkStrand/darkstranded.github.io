---
title: vue面试题总结
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
keywords: vue
description: vue
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---


[原文](https://juejin.cn/post/6850037277675454478#comment)

## 1. vue框架篇

### 1.1 vue的优点

* 轻量级框架：只关注视图层，是一个构建数据的视图集合，大小只有几十kb；
* 简单易学：国人开发，中文文档，不存在语言障碍 ，易于理解和学习；
* 双向数据绑定：保留了angular的特点，在数据操作方面更为简单；
* 组件化：保留了react的优点，实现了html的封装和重用，在构建单页面应用方面有着独特的优势；
* 视图，数据，结构分离：使数据的更改更为简单，不需要进行逻辑代码的修改，只需要操作数据就能完成相关操作；
* 虚拟DOM：dom操作是非常耗费性能的，不再使用原生的dom操作节点，极大解放dom操作，但具体操作的还是dom不过是换了另一种方式；
* 运行速度更快:相比较与react而言，同样是操作虚拟dom，就性能而言，vue存在很大的优势。


### 1.2 请详细说下你对vue生命周期的理解？

总共分为8个阶段创建前/后，载入前/后，更新前/后，销毁前/后。

>`创建前/后` 
>1. 在``beforeCreate``阶段，vue实例的挂载元素el和数据对象data都为undefined，还未初始化。
>2. 在``created``阶段，vue实例的数据对象data有了，el为undefined，还未初始化。


>`载入前/后`：
>1. 在``beforeMount``阶段，vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点，data.message还未替换。
>2. 在``mounted``阶段，vue实例挂载完成，data.message成功渲染。


>`更新前/后`：当data变化时，会触发``beforeUpdate``和``updated``方法


>`销毁前/后`：在执行``destroy``方法后，对data的改变不会再触发周期函数，说明此时vue实例已经解除了事件监听以及和dom的绑定，但是dom结构依然存在

### 1.3 为什么vue组件中data必须是一个函数？
* 对象为引用类型，当复用组件时，由于数据对象都指向同一个data对象，当在一个组件中修改data时，其他重用的组件中的data会同时被修改；
* 而使用返回对象的函数，由于每次返回的都是一个新对象（Object的实例），引用地址不同，则不会出现这个问题。

### 1.4 vue中v-if和v-show有什么区别？
v-if和v-show看起来似乎差不多，当条件不成立时，其所对应的标签元素都不可见，但是这两个选项是有区别的:
1. 手段：v-if是通过控制dom节点的存在与否来控制元素的显隐；v-show是通过设置DOM元素的display样式，block为显示，none为隐藏；
2. 编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换；
3. 编译条件：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留；
4. 性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗；

>用法推荐：
1. v-if更适合带有权限的操作，渲染时判断权限数据，有则展示该功能，没有则删除。
2. v-show更适合于日常使用，可以减少数据的渲染，减少不必要的操作。
综上，v-if有更高的切换消耗，而v-show有更高的初始渲染消耗。
因此，如果需要频繁切换v-show较好，如果在运行时条件不大可能改变，更倾向功能权限性的话v-if较好。

### 1.5 computed和watch的区别
#### 1.5.1 计算属性computed：

* 支持缓存，只有依赖数据发生改变，才会重新进行计算
* 不支持异步，当computed内有异步操作时无效，无法监听数据的变化
* computed 属性值会默认走缓存，计算属性是基于它们的响应式依赖进行缓存的，也就是基于data中声明过或者父组件传递的props中的数据通过计算得到的值
* 如果一个属性是由其他属性计算而来的，这个属性依赖其他属性，是一个多对一或者一对一，一般用computed
* 如果computed属性属性值是函数，那么默认会走get方法；函数的返回值就是属性的属性值；在computed中的，属性都有一个get和一个set方法，当数据变化时，调用set方法。

#### 1.5.2 侦听属性watch：

* 不支持缓存，数据变，直接会触发相应的操作；
* watch支持异步；监听的函数接收两个参数，第一个参数是最新的值；第二个参数是输入之前的值；
* 当一个属性发生变化时，需要执行对应的操作；一对多；
* 监听数据必须是data中声明过或者父组件传递过来的props中的数据，当数据变化时，触发其他操作，函数有两个参数：

> 1. immediate：组件加载立即触发回调函数执行

```javascript
watch: {
  firstName: {
    handler(newName, oldName) {
      this.fullName = newName + ' ' + this.lastName;
    },
    // 代表在wacth里声明了firstName这个方法之后立即执行handler方法
    immediate: true
  }
}
```


>2. deep: deep的意思就是深入观察，监听器会一层层的往下遍历，给对象的所有属性都加上这个监听器，但是这样性能开销就会非常大了，任何修改obj里面任何一个属性都会触发这个监听器里的 handler
```javascript
watch: {
  obj: {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    deep: true
  }
}
```

> 优化：我们可以使用字符串的形式监听
```javascript
watch: {
  'obj.a': {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    // deep: true
  }
}
```

这样Vue.js才会一层一层解析下去，直到遇到属性a，然后才给a设置监听函数。

### 1.6 vue-loader是什么？使用它的用途有哪些？
vue文件的一个加载器，跟template/js/style转换成js模块。

### 1.7 $nextTick是什么？
vue实现响应式并不是数据发生变化后dom立即变化，而是按照一定的策略来进行dom更新。

>nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用nextTick，则可以在回调中获取更新后的 DOM

### 1.8 v-for key的作用
* 当Vue用 ``v-for`` 正在更新已渲染过的元素列表时，它默认用``就地复用``策略。如果数据项的顺序被改变，Vue将不是移动DOM元素来匹配数据项的改变，而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。
* 为了给Vue一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。key属性的类型只能为 string或者number类型。
* key 的特殊属性主要用在Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes。如果不使用 key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。使用key，它会基于key的变化重新排列元素顺序，并且会移除 key 不存在的元素。

### 1.9 简述MVVM
* ``MVVM``是 ``Model-View-ViewModel`` 的缩写。``MVVM`` 是一种设计思想。 
  > ``Model层`` 代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑
  > ``View层`` 代表UI组件，它负责将数据模型转化成UI展现出来
  > ``ViewModel`` 是一个同步 View 和 Model 的对象。

* 在 ``MVVM`` 架构下， ``View ``和 ``Model`` 之间并没有直接的联系，而是通过``ViewModel``进行交互，``Model``和``ViewModel``之间的交互是双向的，因此``View``数据的变化会同步到``Model``中，而``Model``数据的变化也会立即反映到``View``上。

* ``ViewModel``通过双向数据绑定把``View层``和``Model``层连接了起来，而``View``和``Model``之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作``DOM``，不需要关注数据状态的同步问题，复杂的数据状态维护完全由``MVVM``来统一管理。

![MVVM](https://upload-images.jianshu.io/upload_images/13038962-96704c499078e5b7.png?imageMogr2/auto-orient/strip|imageView2/2/w/560/format/webp)


### 1.10 Vue的双向数据绑定原理是什么？
vue.js 是采用数据劫持结合``发布者-订阅者模式``的方式，通过``Object.defineProperty()``来劫持各个属性的``setter``，``getter``，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下几个步骤：

1. 需要observer的数据对象进行递归遍历，包括子属性对象的属性，都加上setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化


2. compiler解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图


3. Watcher订阅者是Observer和Compiler之间通信的桥梁，主要做的事情是:
    * 在自身实例化时往属性订阅器(dep)里面添加自己
    * 自身必须有一个update()方法
    * 待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退。


4. MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。

[vue双向绑定原理](https://www.jianshu.com/p/bb5d1bede3ea)
![图](https://upload-images.jianshu.io/upload_images/7120480-7d53305530ed75a1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



### 1.11 组件传值
#### 1.11.1 父传子
通过props传递
>父组件： ``<child value = '传递的数据' />``
>
>子组件: ``props['value']``,接收数据,接受之后使用和data中定义数据使用方式一样

#### 1.11.2 子传父
在父组件中给子组件绑定一个自定义的事件，子组件通过$emit()触发该事件并传值。
>父组件： ``<child @receive = 'receive' />``
>
>子组件: this.$emit('receive','传递的数据')

#### 1.11.3 兄弟组件传值

* 通过中央通信 let bus = new Vue()

>A：methods :{ 函数{bus.$emit(‘自定义事件名’，数据)} 发送


>B：created （）{bus.$on(‘A发送过来的自定义事件名’，函数)} 进行数据接收


* 通过vuex

### 1.12 prop 验证，和默认值
我们在父组件给子组件传值的时候，可以指定该props的默认值及类型，当传递数据类型不正确的时候，vue会发出警告
```javascript
props: {
    visible: {
        default: true,
        type: Boolean,
        required: true
    },
}
```

### 1.13 请说下封装 vue 组件的过程
* 首先，组件可以提升整个项目的开发效率。能够把页面抽象成多个相对独立的模块，解决了我们传统项目开发：效率低、难维护、复用性等问题。
* 然后，使用Vue.extend方法创建一个组件，然后使用Vue.component方法注册组件。子组件需要数据，可以在props中接受定义。而子组件修改好数据后，想把数据传递给父组件。可以采用emit方法。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue组件</title>
    <script src="vue.js"></script>
</head>
<body>
    <div id="app1">
        <my-com></my-com>
    </div>
    <div id="app2">
        <my-com></my-com>
    </div>
 
    <script>
        /*创建组件*/
        var myCom = Vue.extend({
            template: '<div>这是我的组件</div>'
        });
        /*全局注册组件*/
        Vue.component('my-com',myCom);
 
        /*定义vue实例app1*/
        var app1 = new Vue({
            el: '#app1'
        });
 
        /*定义vue实例app2*/
        var app2 = new Vue({
            el: '#app2'
        });
    </script>
</body>
</html>
```

### 1.14 Vue.js的template编译
简而言之，就是先转化成AST树，再得到的render函数返回VNode（Vue的虚拟DOM节点），详细步骤如下：

>首先，通过compile编译器把template编译成AST语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），compile是createCompiler的返回值，createCompiler是用以创建编译器的。另外compile还负责合并option。


>然后，AST会经过generate（将AST语法树转化成render funtion字符串的过程）得到render函数，render的返回值是VNode，VNode是Vue的虚拟DOM节点，里面有（标签名、子节点、文本等等）

### 1.15 scss是什么？在vue.cli中的安装使用步骤是？有哪几大特性？
css的预编译,使用步骤如下：
* 第一步：用npm 下三个loader（sass-loader、css-loader、node-sass）
* 第二步：在build目录找到webpack.base.config.js，在那个extends属性中加一个拓展.scss
* 第三步：还是在同一个文件，配置一个module属性
* 第四步：然后在组件的style标签加上lang属性 ，例如：lang=”scss”

特性主要有:

* 可以用变量，例如（$变量名称=值）
* 可以用混合器，例如（）
* 可以嵌套

### 1.16 vue如何监听对象或者数组某个属性的变化
当在项目中直接设置数组的某一项的值，或者直接设置对象的某个属性值，这个时候，你会发现页面并没有更新。这是因为Object.defineProperty()限制，监听不到变化。
解决方式：

* this.$set(你要改变的数组/对象，你要改变的位置/key，你要改成什么value)
```javascript
this.$set(this.arr, 0, "OBKoro1"); // 改变数组
this.$set(this.obj, "c", "OBKoro1"); // 改变对象
```

* 调用以下几个数组的方法
```javascript
splice()、 push()、pop()、shift()、unshift()、sort()、reverse()
```

vue源码里缓存了array的原型链，然后重写了这几个方法，触发这几个方法的时候会observer数据，意思是使用这些方法不用我们再进行额外的操作，视图自动进行更新。 推荐使用splice方法会比较好自定义,因为splice可以在数组的任何位置进行删除/添加操作

### 1.17 常用的事件修饰符

* .stop:阻止冒泡
* .prevent:阻止默认行为.
* .self:仅绑定元素自身触发
* .once: 2.1.4 新增,只触发一次
* .passive: 2.3.0 新增,滚动事件的默认行为 (即滚动行为) 将会立即触发,不能和.prevent 一起使用
* .sync 修饰符

从 2.3.0 起vue重新引入了.sync修饰符，但是这次它只是作为一个编译时的语法糖存在。它会被扩展为一个自动更新父组件属性的 v-on 监听器。示例代码如下：
```
<comp :foo.sync="bar"></comp>
```
会被扩展为：
```
<comp :foo="bar" @update:foo="val => bar = val"></comp>
```
当子组件需要更新 foo 的值时，它需要显式地触发一个更新事件：
```
this.$emit('update:foo', newValue)
```

[vue中.sync修饰符](https://blog.csdn.net/liushijun_/article/details/92426854)

### 1.18 vue如何获取dom
先给标签设置一个ref值，再通过this.$refs.domName获取，例如：
```
<div ref="test"></div>

const dom = this.$refs.test
```

### 1.19 v-on可以监听多个方法吗？
是可以的，来个例子：
```
<input type="text" v-on="{ input:onInput,focus:onFocus,blur:onBlur, }">
```

### 1.20 assets和static的区别
这两个都是用来存放项目中所使用的静态资源文件。

两者的区别：

* ``assets``中的文件在运行npm run build的时候会打包，简单来说就是会被压缩体积，代码格式化之类的。打包之后也会放到static中。

* ``static``中的文件则不会被打包。

>建议：将图片等未处理的文件放在assets中，打包减少体积。而对于第三方引入的一些资源文件如iconfont.css等可以放在static中，因为这些文件已经经过处理了。

### 1.21slot插槽
很多时候，我们封装了一个子组件之后，在父组件使用的时候，想添加一些dom元素，这个时候就可以使用slot插槽了，但是这些dom是否显示以及在哪里显示，则是看子组件中slot组件的位置了。

> 以下为详细扩展
#### 1.21.1 插槽是什么
* 写个父组件：test.vue

```vue
<template>
  <div>
    <div>大家好我是父组件</div>
    <myslot>
      <p>测试一下吧内容写在这里了能否显示</p>
    </myslot>
  </div>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
  }
</script>

<style>
</style>
```

* 写个子组件：myslot.vue

```vue
<template>
  <div>
    <div>我是子组件</div>
  </div>
</template>

<script>
</script>

<style>
</style>
```

运行代码，发现，最终渲染的效果是
>大家好我是父组件
>
>我是子组件

那如果我想实现显示父组件中p标签的内容怎么办 修改子组件：myslot.vue

```vue
<template>
  <div>
      <div>我是子组件</div>
      <p>现在测试一下slot</p>
      <slot></slot>
  </div>
</template>

<script>
</script>

<style>
</style>
```

运行代码，可以看到以下效果
>大家好我是父组件<br>我是子组件<br>现在测试一下slot<br>测试一下吧内容写在这里了能否显示

官方文档对于插槽的应用场景是这样描述的: 我们经常需要向一个组件传递内容 Vue 自定义的 ``<slot>`` 元素让这变得非常简单 只要在需要的地方加入插槽就行了——就这么简单！ 
结合上面的例子来理解就是这样的： 
1. 父组件在引用子组件时希望向子组件传递模板内容``<p>``测试一下吧内容写在这里了能否显示``</p>`` 
2. 子组件让父组件传过来的模板内容在所在的位置显示 
3. 子组件中的``<slot>``就是一个槽，可以接收父组件传过来的模板内容，``<slot>`` 元素自身将被替换 
4. ``<myslot></myslot>``组件没有包含一个 ``<slot>`` 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃

#### 1.21.2 插槽的作用
让用户可以拓展组件，去更好地复用组件和对其做定制化处理

#### 1.21.3 插槽的分类
##### 1.21.3.1 默认插槽
在一个 ``<submit-button>`` 组件中：
```
<button type="submit">
  <slot></slot>
</button>
```
* 我们可能希望这个 ``<button>`` 内绝大多数情况下都渲染文本“Submit”，但是有时候却希望渲染文本为别的东西，那怎么实现呢？ 我们可以将“Submit”作为后备内容，我们可以将它放在 ``<slot>`` 标签内：
```
<button type="submit">
  <slot>Submit</slot>
</button>
```

现在当我在一个父级组件中使用 ``<submit-button>`` 并且不提供任何插槽内容时：
```
<submit-button></submit-button>
```
后备内容“Submit”将会被渲染：
```
<button type="submit">
  Submit
</button>
```

* 但是如果我们提供内容：

```
<submit-button>
  Save
</submit-button>
```

则这个提供的内容将会被渲染从而取代后备内容：
```
<button type="submit">
  Save
</button>
```

##### 1.21.3.2 具名插槽
有时我们写了一个子组件，我们希望
```
<template>
  <div class="container">
    <header>
      <!-- 我们希望把页头放这里 -->
    </header>
    <main>
      <!-- 我们希望把主要内容放这里 -->
    </main>
    <footer>
      <!-- 我们希望把页脚放这里 -->
    </footer>
  </div>
</template>
```

对于这样的情况，``<slot>`` 元素有一个特殊的 attribute：name。这个 attribute 可以用来定义额外的插槽：
```
<template>
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>
```

一个不带 name 的 ``<slot>`` 出口会带有隐含的名字“default”。 父组件在向具名插槽提供内容的时候，我们可以在一个 ``<template>`` 元素上使用 v-slot 指令，并以 v-slot 的参数的形式提供其名称：
```vue
<template>
  <myslot>
    <div>大家好我是父组件</div>
    <template v-slot:header>
      <h1>Here might be a page title</h1>
    </template>

    <p>A paragraph for the main content.</p>
    <p>And another one.</p>

    <template v-slot:footer>
      <p>Here's footer info</p>
    </template>
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
  }
</script>
<style>
</style>
```

最终的渲染结果可以看到
```
Here might be a page title
大家好我是父组件
A paragraph for the main content.

And another one.

Here's footer info
```

父组件中会向子组件中具名传递对应的模板内容，而没有指定名字的模板内容会传递给子组件中不带 name 的 ``<slot>`` 当然，如果父组件中
```
<template v-slot:default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
</template>
```

* 同样是传递给子组件中不带 name 的 ``<slot>`` 注意: v-slot 只能添加在 ``<template>`` 上 具名插槽在书写的时候可以使用缩写,v-slot用``#``来代替

```vue
<template>
  <myslot>
    <div>大家好我是父组件</div>
    <template #header>
      <h1>Here might be a page title</h1>
    </template>

    <p>A paragraph for the main content.</p>
    <p>And another one.</p>

    <template #footer>
      <p>Here's footer info</p>
    </template>
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
  }
</script>

<style>
</style>
```

##### 1.21.3.3 作用域插槽
这里主要解决的是父组件在向子组件插槽传递模板内容时存在访问子组件数据的问题 还记得默认插槽吗？如果子组件中写在 ``<slot>`` 标签内后备内容是与 该组件的data属性双向数据绑定的
```vue
<template>
  <div>
    <span>
      <slot>{{user.firstName}}</slot>
    </span>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        user:{
          firstName:'gerace',
          lastName:'haLi'
        }
      }
    }
  }
</script>
<style>
</style>
```

父组件在引用子组件时，希望能够换掉备用内容
```vue
<template>
  <myslot>{{ user.firstName }}</myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
  }
</script>
<style>
</style>
```

运行代码这时你会发现提示报错
```
Property or method "user" is not defined on the instance but referenced during render.
TypeError: Cannot read property 'firstName' of undefined
```

这里为什么？vue官方文档给出了答案 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的 那应该怎么解决这个问题？ 为了让 user 在父级的插槽内容中可用，我们可以将 user 作为 ``<slot>`` 元素的一个 attribute 绑定上去：
```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

绑定在 ``<slot>`` 元素上的 attribute 被称为插槽 prop。现在在父级作用域中，我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字：
```vue
<template>
  <myslot>
      <template v-slot:default="slotProps">
      {{ slotProps.user.firstName }}
      </template>
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
</script>
<style>
</style>
```

上面例子，我们选择将包含所有插槽 prop 的对象命名为 slotProps，但你也可以使用任意你喜欢的名字。 针对上面只给默认插槽传递模板内容的例子，在写法上可以采用默认插槽的缩写语法
```vue
<template>
  <myslot v-slot:default="slotProps">
     {{ slotProps.user.firstName }}
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
</script>
<style>
</style>

↓↓↓
<template>
  <myslot v-slot="slotProps">
     {{ slotProps.user.firstName }}
  </myslot>
</template>

```

>注意： 默认插槽的缩写语法不能和具名插槽混用，因为它会导致作用域不明确：
```
<template>
  <myslot v-slot="slotProps">
     {{ slotProps.user.firstName }}
     <template v-slot:other="otherSlotProps">
   		slotProps is NOT available here
     </template>
  </myslot>
</template>
```

* 下面再看一下多个插槽的情况 子组件

```vue
<template>
  <div>
    <span>
      <slot v-bind:userData="user" name="header">
        {{ user.msg }}
      </slot>
      <slot v-bind:hobbyData="hobby" name="footer">
        {{ hobby.fruit }}
      </slot>
    </span>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        user:{
          firstName: 'gerace',
          lastName: 'haLi',
        },
        hobby:{
          fruit: "apple",
          color: "blue"
        }
      }
    }
  }
</script>
<style>
</style>
```

父组件
```vue
<template>
  <myslot>
      <template v-slot:header="slotProps">
        {{ slotProps.userData.firstName }}
      </template>
      <template v-slot:footer="slotProps">
        {{ slotProps.hobbyData.fruit }}
      </template>
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
</script>
<style>
</style>
```

针对多个插槽的情况，在写法上可以解构插槽prop，父组件的写法如下
```
<template>
  <myslot>
      <template v-slot:header="{userData}">
        {{ userData.firstName }}
      </template>
      <template v-slot:footer="{hobbyData}">
        {{ hobbyData.fruit }}
      </template>
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
  }
</script>
<style>
</style>
```

在具名插槽的介绍部分有讲过，具名插槽可以使用缩写，v-slot可以使用#来代替，所以以上代码可以写成：

```vue
<template>
  <myslot>
      <template #header="{userData}">
        {{ userData.firstName }}
      </template>
      <template #footer="{hobbyData}">
        {{ hobbyData.fruit }}
      </template>
  </myslot>
</template>

<script>
  import myslot from './myslot';
  export default {
    components: {
      myslot
    }
  }
</script>
<style>
</style>
```

但是需要注意的是该缩写只在其有参数的时候才可用。这意味着以下语法是无效的：
```
<!-- 这样会触发警告 -->
<template>
  <myslot>
      <template #="{userData}">
        {{ userData.firstName }}
      </template>
      <template #="{hobbyData}">
        {{ hobbyData.fruit }}
      </template>
  </myslot>
</template>
```

### 1.22 vue初始化页面闪动问题
使用vue开发时，在vue初始化之前，由于div是不归vue管的，所以我们写的代码在还没有解析的情况下会容易出现花屏现象，看到类似于{{message}}的字样，虽然一般情况下这个时间很短暂，但是我们还是有必要让解决这个问题的。
首先：在css里加上以下代码
```
[v-cloak] {
    display: none;
}
```

如果没有彻底解决问题，则在根元素加上``style="display: none;" :style="{display: 'block'}"``


## 2. vue插件篇
### 2.1 状态管理（vuex）

#### 2.1.1 vuex是什么
Vuex 是一个专为 Vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 devtools extension，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。

#### 2.1.2 怎么使用vuex

1. 第一步安装
```javascript
npm install vuex -S
```
2. 第二步创建store
```javascript
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);
//不是在生产环境debug为true
const debug = process.env.NODE_ENV !== 'production';
//创建Vuex实例对象
const store = new Vuex.Store({
  strict:debug,//在不是生产环境下都开启严格模式
  state:{
  },
  getters:{
  },
  mutations:{
  },
  actions:{
  }
})
export default store;
```
3. 第三步注入vuex
```javascript
import Vue from 'vue';
import App from './App.vue';
import store from './store';
const vm = new Vue({
    store:store,
    render: h => h(App)
}).$mount('#app')
```

#### 2.1.3 vuex中有几个核心属性，分别是什么？
一共有5个核心属性，分别是:

1. ``state`` 唯一数据源,Vue 实例中的 data 遵循相同的规则

2. ``getters`` 可以认为是 store 的计算属性,就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。Getter 会暴露为 store.getters 对象，你可以以属性的形式访问这些值.

```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})

store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]

```

3. `mutation` 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation,非常类似于事件,通过store.commit 方法触发

```javascript
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})

store.commit('increment')
```

4. `action` Action 类似于 mutation，不同在于Action 提交的是 mutation，而不是直接变更状态，Action 可以包含任意异步操作

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

5. ``module``  由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。

```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

#### 2.1.4 ajax请求代码应该写在组件的methods中还是vuex的actions中
如果请求来的数据是不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入vuex 的state里。

如果被其他地方复用，这个很大几率上是需要的，如果需要，请将请求放入action里，方便复用。

#### 2.1.5 从vuex中获取的数据能直接更改吗？
从vuex中取的数据，不能直接更改，需要浅拷贝对象之后更改，否则报错；

#### 2.1.6 vuex中的数据在页面刷新后数据消失
用sessionstorage 或者 localstorage 存储数据
```
存储： sessionStorage.setItem( '名', JSON.stringify(值) )
使用： sessionStorage.getItem('名') ---得到的值为字符串类型，用JSON.parse()去引号；
```

> 也可以引入插件vuex-persist，使用方法如下：

* 安装
```javascript
npm install --save vuex-persist
or
yarn add vuex-persist
```

* 引入
```javascript
import VuexPersistence from 'vuex-persist'
```

* 先创建一个对象并进行配置
```javascript
const vuexLocal = new VuexPersistence({
    storage: window.localStorage
})
```

* 引入进vuex插件
```javascript
const store = new Vuex.Store({
  state: { ... },
  mutations: { ... },
  actions: { ... },
  plugins: [vuexLocal.plugin]
}) 
```
通过以上设置，在图3中各个页面之间跳转，如果刷新某个视图，数据并不会丢失，依然存在，并且不需要在每个 mutations 中手动存取 storage 。

#### 2.1.7 Vuex的严格模式是什么,有什么作用,怎么开启？
在严格模式下，无论何时发生了状态变更且不是由mutation函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。
在Vuex.Store 构造器选项中开启,如下
```javascript
const store = new Vuex.Store({
    strict:true,
})
```

#### 2.1.8 怎么在组件中批量使用Vuex的getter属性
使用``mapGetters``辅助函数, 利用对象展开运算符将getter混入computed 对象中
```
import {mapGetters} from 'vuex'
export default{
    computed:{
        ...mapGetters(['total','discountTotal'])
    }
}
```

#### 2.1.9 组件中重复使用mutation
使用``mapMutations``辅助函数,在组件中这么使用
```vue
import { mapMutations } from 'vuex'
methods:{
    ...mapMutations({
        setNumber:'SET_NUMBER',
    })
}
```

然后调用this.setNumber(10)相当调用this.$store.commit('SET_NUMBER',10)

#### 2.1.10 mutation和action有什么区别

* action 提交的是 mutation，而不是直接变更状态。mutation可以直接变更状态
* action 可以包含任意异步操作。mutation只能是同步操作
* 提交方式不同

>action 是用this.store.dispatch('ACTION_NAME',data)来提交。
>
>mutation是用this.$store.commit('SET_NUMBER',10)来提交

* 接收参数不同，mutation第一个参数是state，而action第一个参数是context，其包含了
```
{
    state,      // 等同于 `store.state`，若在模块中则为局部状态
    rootState,  // 等同于 `store.state`，只存在于模块中
    commit,     // 等同于 `store.commit`
    dispatch,   // 等同于 `store.dispatch`
    getters,    // 等同于 `store.getters`
    rootGetters // 等同于 `store.getters`，只存在于模块中
}
```

#### 2.1.11 在v-model上怎么用Vuex中state的值？
需要通过computed计算属性来转换。
```
<input v-model="message">
// ...
computed: {
  message: {
    get () {
        return this.$store.state.message
    },
    set (value) {
        this.$store.commit('updateMessage', value)
    }
  }
}
```

### 2.2 路由页面管理（vue-router）
#### 2.2.1 什么是vue-router
Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

* 嵌套的路由/视图表
* 模块化的、基于组件的路由配置
* 路由参数、查询、通配符
* 基于 Vue.js 过渡系统的视图过渡效果
* 细粒度的导航控制
* 带有自动激活的 CSS class 的链接
* HTML5 历史模式或 hash 模式，在 IE9 中自动降级
* 自定义的滚动条行为

#### 2.2.2 怎么使用vue-router
1. 第一步安装
```
npm install vue-router -S
```
2. 第二步在main.js中使用Vue Router组件
```javascript
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)

new Vue({
  el: '#app',
  render: h=> h(App)
})
```

3. 第三步配置路由

* 定义 (路由) 组件
路由组件可以是直接定义，也可以是导入已经定义好的组件。这里导入已经定义好的组件。如下
```javascript
// 1.定义组件（导入组件）
import Home from './components/home.vue'
import News from '.components/news.vue'
```

* 定义路由（路由对象数组）
定义路由对象数组。对象的path是自定义的路径（即使用这个路径可以找到对应的组件），component是指该路由对应的组件。如下：
```javascript
// 2.定义路由
const routes = [
  { path: '/home', components: Home },
  { path: '/news', components: News },
  { path: '*', redirect: '/home' }, // 表示没有匹配到，是默认重定向到home组件
]
```


* 实例化Vue Router对象
调用Vue Router的构造方法创建一个Vue Router的实例对象，将上一步定义的路由对象数组作为参数对象的值传入。如下
```javascript
// 3. 实例化Vue Router
const router = new VueRouter({routes}) // 此处相当于routes: routes
```


* 挂载根实例
```javascript
// 4. 挂载根实例
new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

第四步在App.vue中使用路由
在App.vue中使用标签来显示路由对应的组件，使用标签指定当点击时显示的对应的组件，to属性就是指定组件对应的路由。如下：
```vue
<template>
  <div id="app">
    <router-link to="/home">Home</router-link>
    <router-link to="/news">News</router-link>
    <br />
    <router-view></router-view> /* 显示组件的内容 */
  </div>
</template>
```

#### 2.2.3 怎么定义vue-router的动态路由？怎么获取传过来的动态参数？
在router目录下的index.js文件中，对path属性加上/:id。使用router对象的params.id获取动态参数

#### 2.2.4 vue-router的导航钩子
常用的是router.beforeEach(to,from,next)，在跳转前进行权限判断。一共有三种：

* 全局导航钩子：router.beforeEach(to,from,next)
* 组件内的钩子
* 单独路由独享组件

#### 2.2.5 vue路由传参

* 页面刷新数据不会丢失

```javascript
methods: {
  jump(id) {
    // 直接调用$router.push实现携带参数的跳转
    this.$router.push({
      path: `/particulars/${id}`
    })
  }
}

// 对应路由配置
{
  path: '/particulars/:id',
  name: 'particulars',
  component: particulars
}

// 对应子组件：这样来获取参数
this.$route.params.id
```

* 使用params方式传入的参数
> 这种方法页面刷新数据会丢失
>
>通过路由属性中的name来确定匹配的路由，通过params来传递参数
```javascript
methods: {
  jump(id) {
    this.$router.push({
      path: '/particulars',
      params: {
        id: id
      }
    })
  }
}

// 对应路由配置
{
  path: '/particulars',
  name: 'particulars',
  component: particulars
}

// 对应子组件：这样来获取参数
this.$route.params.id
```

* 使用query方法传入的参数
> 使用path来匹配路由，然后通过query来传递参数
>
> 这种情况下，query传递的参数会显示在url后面?id=?
```javascript
methods: {
  jump(id) {
    this.$router.push({
      path: '/particulars',
      query: {
        id: id
      }
    })
  }
}

// 对应路由配置
{
  path: '/particulars',
  name: 'particulars',
  component: particulars
}

// 对应子组件：这样来获取参数
this.$route.query.id
```

#### 2.2.6 router和route的区别

* ``route``为当前router跳转对象里面可以获取name、path、query、params等

* ``router``为VueRouter实例，想要导航到不同URL，则使用router.push方法

#### 2.2.7 路由 TypeError: Cannot read property 'matched' of undefined 的错误问题
找到入口文件main.js里的new Vue()，必须使用router名，不能把router改成Router或者其他的别名
```javascript
// 引入路由
import router from './routers/router.js'

new Vue({
    el: '#app',
    router,    // 这个名字必须使用router
    render: h => h(App)
});
```

#### 2.2.8 路由按需加载
随着项目功能模块的增加，引入的文件数量剧增。如果不做任何处理，那么首屏加载会相当的缓慢，这个时候，路由按需加载就闪亮登场了。
```javascript
// webpack< 2.4 时
{ 
    path:'/', 
    name:'home',
    components:resolve=>require(['@/components/home'],resolve)
} 

// webpack> 2.4 时
{ 
    path:'/', 
    name:'home', 
    components:()=>import('@/components/home')
}
```

import()方法是由es6提出的，动态加载返回一个Promise对象，then方法的参数是加载到的模块。类似于Node.js的require方法，主要import()方法是异步加载的。

#### 2.2.9 Vue里面router-link在电脑上有用，在安卓上没反应怎么解决
Vue路由在Android机上有问题，babel问题，安装``babel polypill``插件解决

#### 2.2.10 Vue2中注册在router-link上事件无效解决方法
* 使用``@click.native``。
* 原因：router-link会阻止click事件，.native指直接监听一个原生事件


#### 2.2.11 RouterLink在IE和Firefox中不起作用（路由不跳转）的问题

* 只用a标签，不使用button标签
* 使用button标签和Router.navigate方法

### 2.3 网络请求(axios)
axios的二次封装，主要包括请求之前、返回响应以及使用等
#### 2.3.1 请求之前
一般的接口都会有鉴权认证（token）之类的，因此在接口的请求头里面，我们需要带上token值以通过服务器的鉴权认证。但是如果每次请求的时候再去添加，不仅会大大的加大工作量，而且很容易出错。好在axios提供了拦截器机制，我们在请求的拦截器中可以添加token。
```javascript
// 请求拦截
axios.interceptors.request.use((config) => {
  // ...省略代码
  config.headers.x_access_token = token
  return config
}, function (error) => {
  return Promise.reject(error)
})
```
当然请求拦截器中，除了处理添加token以外，还可以进行一些其他的处理，具体的根据实际需求进行处理。

#### 2.3.2 响应之后
请求接口，并不是每一次请求都会成功。那么当接口请求失败的时候，我们又怎么处理呢？每次请求的时候处理？封装axios统一处理？应该选择封装axios进行统一处理。axios不仅提供了请求的拦截器，其也提供了响应的拦截器。在此处，可以获取到服务器返回的状态码，然后根据状态码进行相对应的操作。
```javascript
// 响应拦截
axios.interceptors.response.use(function (response) {
  if (response.data.code === 401) {
    // 用户token失效
    // 清空用户信息
    sessionStorage.user = ''
    sessionStorage.token = ''
    window.location.href = '/' // 返回登录页
    return Promise.reject(msg) // 接口Promise返回错误状态，错误信息msg可由后端返回，也可以我们自己定义一个码--信息的关系。
  }
  if (response.status !== 200 || response.data.code !== 200) {
    // 接口请求失败，具体根据实际情况判断
    message.error(mes) // 提示错误信息
    return Promise.reject(msg) // 接口Promise返回错误状态
  }
  return response
}, function (error) {
  if (axios.isCancel(error)) {
    requestList.length = 0
    // store.dispatch('changeGlobalState', { loading: false })
    throw new axios.Cancel('cancel request')
  } else {
    message.error('网络请求失败，请重试')
  }
  return Promise.reject(error)
})

```
当然响应拦截器同请求拦截器一样，还可以进行一些其他的处理，具体的根据实际需求进行处理。

#### 2.3.3 使用axios
axios使用的时候一般有三种方式：
* 执行get请求
```javascript
axios.get('url', {
  params: {} // 接口参数
}).then(function(res){
  console.log(res) // 处理成功的函数，相当于success
}).catch(function(error) {
  console.log(error) // 错误处理，相当于error
})
```

* 执行post请求
```javascript
axios.post('url', 
  {
    data: xxx // 参数
  },
  {
    headers: xxx //请求头信息
}).then(function(res) {
  console.log(res) // 处理成功的函数，相当于success
}).catch(function(error) {
  console.log(error) // 错误处理，相当于error
})
```

* axios API通过相关配置传递给axios完成请求
```javascript
axios({
  method: 'delete',
  url: 'xxx',
  cache: false,
  params: { id: 123 },
  headers: xxx
})

/* ----------------------- */
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'monkey',
    lastName: 'soft'
  }
})
```

直接使用api的方式虽然简单，但是不同请求参数的名字不一样，在实际开发过程中很容易写错或者忽略，容易为开发造成不必要的时间损失。

前面两种方式虽然没有参数不一致的问题，但是使用的时候，太过于麻烦，那么怎么办呢？

前面两种虽然过于麻烦，但是仔细观察，是可以发现有一定的相似点，我们便可以给予这些相似点二次封装，形成适合我们使用的一个请求函数。
```javascript
/* 
*url: 请求的url
*params: 请求的参数
*config: 请求时的header信息
*method: 请求方法
*/
const request = function({ url, params, config, method }) {
  // 如果是get请求，需要拼接参数
  let str = ''
  if(method === 'get' && params) {
    Object.keys(params).forEach(item => {
      str += `${item}=${params[item]$}`
    })
  }
  return new Promise((resolve, reject) => {
    axios[method](str ? (url + '?' + str.substring(0, str.length - 1)) : url, params, Object.assign({}, config)).then(response => {
      resolve(response.data)
    }, err => {
      if(err.Cancel) {

      } else {
        reject(err)
      }
    }).catch(err => {
      reject(err)
    })
  })
}

```

### 2.4 视频播放(video.js)
[视频播放video.js](https://juejin.cn/post/6850037269227634702)

### 2.5 vue常用ui库
#### 2.5.1 移动端

* [mint-ui](http://mint-ui.github.io/#!/zh-cn)
* [Vant](https://youzan.github.io/vant/#/zh-CN/home)
* [VUX](https://vux.li/)

#### 2.5.2 pc端

* [element-ui](https://element.eleme.cn/2.13/#/zh-CN/component/installation)
* [Ant Design of Vue](https://www.antdv.com/docs/vue/introduce-cn/)
* [Avue](https://avuejs.com/)


## 3. 常用webpack配置

> vue-lic3脚手架（vue.config.js）

### 3.1 publicPath

* 类型：String
* 默认：'/'

部署应用包时的基本 URL。默认情况下，Vue CLI会假设你的应用是被部署在一个域名的根路径上，例如https://www.my-app.com/。如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。例如，如果你的应用被部署在https://www.my-app.com/my-app/，则设置publicPath为/my-app/

这个值也可以被设置为空字符串 ('') 或是相对路径 ('./')，这样所有的资源都会被链接为相对路径，这样打出来的包可以被部署在任意路径，也可以用在类似 Cordova hybrid 应用的文件系统中。

### 3.2 productionSourceMap
* 类型：boolean
* 默认：true

不允许打包时生成项目来源映射文件，在生产环境下可以显著的减少包的体积

> 注 Source map的作用：针对打包后的代码进行的处理，就是一个信息文件，里面储存着位置信息。也就是说，转换后的代码的每一个位置，所对应的转换前的位置。有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码。这无疑给开发者带来了很大方便

### 3.3 assetsDir
放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录,默认是'',

### 3.4 indexPath
指定生成的 index.html 的输出路径(相对于outputDir)。也可以是一个绝对路径。默认是'index.html'

### 3.5 lintOnSave
是否在每次保存时使用eslint检查，这个对语法的要求比较严格，对自己有要求的同学可以使用
### 3.6 css
```javascript
css: {
  // 是否启用css分离插件，默认是true，如果不启用css样式分离插件，打包出来的css是通过内联样式的方式注入至dom中的，
  extract: true,
  sourceMap: false, // 效果同上
  modules: false, // 为所有的 CSS 及其预处理文件开启 CSS Modules。
  // 这个选项不会影响 `*.vue` 文件。
},
```

### 3.7 devServer
本地开发服务器配置，此处直接贴上我常用的配置，以注释的方式介绍
```javascript
devServer: { 
  // 配置开发服务器
  host: "0.0.0.0",
  // 是否启用热加载，就是每次更新代码，是否需要重新刷新浏览器才能看到新代码效果
  hot: true,
  // 服务启动端口
  port: "8080",
  // 是否自动打开浏览器默认为false
  open: false,
  // 配置http代理
  proxy: { 
    "/api": { 
      //如果ajax请求的地址是http://192.168.0.118:9999/api1那么你就可以在ajax中使用/api/api1路径,其请求路径会解析
      // http://192.168.0.118:9999/api1，当然你在浏览器上开到的还是http://localhost:8080/api/api1;
      target: "http://192.168.0.118:9999",
      //是否允许跨域，这里是在开发环境会起作用，但在生产环境下，还是由后台去处理，所以不必太在意
      changeOrigin: true,
      pathRewrite: {
          //把多余的路径置为''
        "api": ""
      }
    },
    "/api2": {//可以配置多个代理，匹配上那个就使用哪种解析方式
      target: "http://api2",
      // ...
    }
  }
},
```

### 3.8 pluginOptions
这是一个不进行任何 schema 验证的对象，因此它可以用来传递任何第三方插件选项，例如：
```javascript
{
  //定义一个全局的less文件，把公共样式变量放入其中，这样每次使用的时候就不用重新引用了
  'style-resources-loader': {
    preProcessor: 'less',
    patterns: [
      './src/assets/public.less'
    ]
  }
}
```

### 3.9 chainWebpack
是一个函数，会接收一个基于 webpack-chain 的 ChainableConfig 实例。允许对内部的 webpack 配置进行更细粒度的修改。例如：
```javascript
chainWebpack(config) { 
//添加一个路径别名 假设有在assets/img/menu/目录下有十张图片，如果全路径require("/assets/img/menu/img1.png")
//去引入在不同的层级下实在是太不方便了，这时候向下方一样定义一个路劲别名就很实用了
    config.resolve.alias
      //添加多个别名支持链式调用
      .set("assets", path.join(__dirname, "/src/assets"))
      .set("img", path.join(__dirname, "/src/assets/img/menu"))
      //引入图片时只需require("img/img1.png");即可
}
```

## 4. webpack优化
[带你深度解锁Webpack系列(优化篇)](https://segmentfault.com/a/1190000022205477)

[vue-cli中Webpack配置优化（一）](https://www.cnblogs.com/zhurong/p/12603887.html)

[vue-cli中Webpack配置优化（二）](https://www.cnblogs.com/zhurong/p/12611360.html#_label2_1_3)

### 4.1 量化

#### 4.1.1 speed-measure-webpack-plugin

`speed-measure-webpack-plugin` 插件可以测量各个插件和`loader`所花费的时间，使用之后，构建时，会得到类似下面这样的信息：

![](https://image-static.segmentfault.com/334/233/3342331922-6a0fde0ed940ffa7_fix732)



> Vue-cli 2.x

```js
//webpack.config.js
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();

const config = {
    //...webpack配置
}

module.exports = smp.wrap(config);
```



> Vue-cli 3.x（主要区别是包裹 configureWebpack ）

```js
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin')
const smp = new SpeedMeasurePlugin({
  outputFormat: 'human'
})
module.exports = {
  configureWebpack: smp.wrap({
    plugins: []
  })
}
```



#### 4.1.2 webpack-bundle-analyzer

这个是分析打包后，各个文件的大小，用于分析bundle的

> 安装

```
npm i webpack-bundle-analyzer -D
```



在 Vue-cli 3.x 下，安装这个包会报错，是因为用 Vue-cli 3.x 构建的项目在 node_modules 中已经存在，但是项目的 package.json 中没有引用。

需要在 node_modules 中删除这个包，重新安装就可以。



> 使用：（下面是vue-cli 3.x）

```
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin

  configureWebpack: smp.wrap({
    plugins: [
      // 这个要放在所有 plugins 最后
      new BundleAnalyzerPlugin()
    ]
  })
```

在构建完成后，会直接启动一个服务，有一个可视化的界面查看构建后的bundle。



### 4.2 缓存

#### 4.2.1 cache-loader

在一些性能开销较大的`loader`前面添加`cache-loader`，将结果缓存在磁盘中

> 安装：

```
npm install cache-loader -D
```



> 使用：
>
> 在vue-cli2.x 中

```js
module.exports = {
    //...
    module: {
        //我的项目中,babel-loader耗时比较长，所以我给它配置了`cache-loader`
        rules: [
            {
                test: /\.jsx?$/,
                use: ['cache-loader','babel-loader']
            }
        ]
    }
}
```



> 在vue-cli3.x中，这个配置是默认的配置，分别对：`vue-loader`、`babel-loader`两个进行了缓存，其他的需要缓存再自己配置。



#### 4.2.2 hard-source-webpack-plugin

这个是为模块提供中间缓存，效率提升很大。

> 安装

```
npm i hard-source-webpack-plugin -D
```



> 使用

直接在 plugins 中 new就可以。

```js
const HardSourceWebpackPlugin = require('hard-source-webpack-plugin')
module.exports = {
  configureWebpack: smp.wrap({
    plugins: [
      // 为模块提供中间缓存，缓存路径是：node_modules/.cache/hard-source
      new HardSourceWebpackPlugin(),
      new BundleAnalyzerPlugin()
    ]
  })
}
```



构建后效果：

![](https://img2020.cnblogs.com/blog/592961/202003/592961-20200331111737573-1701609808.png)

![](https://img2020.cnblogs.com/blog/592961/202003/592961-20200331111750107-1452016351.png)

![](https://img2020.cnblogs.com/blog/592961/202003/592961-20200331111800199-1571336442.png)

上面三幅图，分别是配置后第一次、第二次、第三次构建的，第三次构建可以达到80%的提升。













### 4.3 exclude/include

我们可以通过`exclue`、`include`配置来确保转移尽可能少的文件。顾名思义，`exclude`指定要排除的文件，`include`指定要包含的文件。



`exclude`的优先级高于`include`，在`include`和`exclude`中使用绝对路径数组，尽量避免`exclude`，更倾向于使用`include`。

```js
//webpack.config.js
const path = require('path');
module.exports = {
    //...
    module: {
        rules: [
            {
                test: /\.js[x]?$/,
                use: ['babel-loader'],
                include: [path.resolve(__dirname, 'src')]
            }
        ]
    },
}
```

下图是未配置`include`和配置了`include`的构建结果对比：

![](https://image-static.segmentfault.com/341/005/3410059882-f459f782062d01e1_fix732)





## 5. 历史好文推荐
1. [【万字长文】史上最强css、html总结~看完涨薪不再是梦](https://juejin.im/post/6850418118695583758)
2. [【万字长文】最全JavaScript基础总结~建议收藏](https://juejin.im/post/6854573211451932685)
3. [Event Loop我知道，宏任务微任务是什么鬼？](https://juejin.im/post/6847902222882340872)
4. [锋利码农武器之vscode](https://juejin.im/post/6847009771493523464)
5. [面试宝典带你走上人生巅峰](https://juejin.im/post/6847009771371888653)

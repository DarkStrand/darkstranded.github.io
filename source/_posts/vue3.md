---
title: vue3
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: DarkStrand.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-08-13 08：38：25
comments: true
tags: 
 - vue3
 - vue
keywords: vue
description: vue
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---



# 1. 安装

## 1.1 CDN

对于制作原型或学习，你可以这样使用最新版本：

```html
<script src="https://unpkg.com/vue@next"></script>
```

对于生产环境，我们推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏。



```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="js/vue3.js"></script>
  <!-- <script src="https://unpkg.com/vue@next"></script> -->
</head>
<body>
  <div id="counter">
    <h1>counter: {{ num }}</h1>
  </div>

  <script>
    const Counter = {
      data() {
        return {
          num: 0
        }
      }
    }
    // 创建一个应用，将配置的对象counter的内容渲染到选择器是#counter的元素上
    let app = Vue.createApp(Counter).mount('#counter')
    console.log(app)
  </script>
</body>
</html>
```





## 1.2 命令行工具（CLI）

Vue提供了[官方的CLI](https://github.com/vuejs/vue-cli)，为单页面应用（SPA）快速搭建繁杂的脚手架。它为现代前端工作流提供了功能齐备的构建设置。只需要几分钟的时间就可以运行起来并带有热重载、保存时 lint 校验，以及生产环境可用的构建版本。更多详情可查阅[Vue CLI的文档](https://cli.vuejs.org/)。

对于 Vue3，你应该使用`npm`上可用的 Vue CLI v4.5 作为 `@vue/cli`。要升级，你应该需要全局重新安装最新版本的 `@vue/cli`：

```sh
yarn global add @vue/cli
# 或
npm install -g @vue/cli
```

然后在 Vue 项目中运行：

```sh
vue upgrade --next
```



## 1.3 Vite

[Vite](https://github.com/vitejs/vite)是一个 web 开发构建工具，由于其原生 ES 模块导入方式，可以实现闪电般的冷服务启动。

通过在终端中运行以下命令，可以使用 Vite 快速构建 Vue 项目。

使用 npm：

```sh
# npm 6.x
$ npm init vite@latest <project-name> --template vue

# npm 7+，需要加上额外的双短横线
$ npm init vite@latest <project-name> -- --template vue

$ cd <project-name>
$ npm install
$ npm run dev
```

或者 yarn：

```sh
$ yarn create vite <project-name> --template vue
$ cd <project-name>
$ yarn
$ yarn dev
```





# 2. 介绍

## 2.1 声明式渲染

Vue.js的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<div id="counter">
  Counter: {{ counter }}
</div>
```

```js
const Counter = {
	data() {
		return {
			counter: 0
    }
  }
}
Vue.createApp(Counter).mount('#counter')
```

我们已经成功创建了第一个 Vue 应用！看起来这跟渲染一个字符串模板非常类似，但是 Vue 在背后做了大量工作。现在数据和DOM已经被建立了关联，所有东西都是**响应式**的。我们要怎么确认呢？请看下面的示例，其中`counter`property每秒递增，你将看到渲染的DOM是如何变化的：

```js
const Counter = {
  data() {
    return {
      counter: 0
    }
  },
  mounted() {
    setInterval(() => {
      this.counter++
    }, 1000)
  }
}
```



除了文本插值，我们还可以像这样绑定元素的 attribute：

```html
<div id="bind-attribute">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

```js
const AttributeBinding = {
  data() {
    return {
      message: 'You loaded this page on ' + new Date().toLocaleString()
    }
  }
}

Vue.createApp(AttributeBinding).mount('#bind-attribute')
```

这里我们遇到了一点新东西。你看到的`v-bind`attribute被称为**指令**。指令带有前缀`v-`，以表示它们是Vue提供的特殊 attribute。可能你也猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。

在这里，该指令的意思是：*“将这个元素节点的`title`attribute和当前活跃实例的`message`property保持一致”*。



## 2.2 处理用户输入

为了让用户和应用进行交互，我们可以用`v-on`指令添加一个事件监听器，通过它调用在实例中定义的方法：

```html
<div id="event-handling">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">反转 Message</button>
</div>
```

```js
const EventHandling = {
  data() {
    return {
      message: 'Hello Vue.js!'
    }
  },
  methods: {
    reverseMessage() {
      this.message = this.message
        .split('')
        .reverse()
        .join('')
    }
  }
}

Vue.createApp(EventHandling).mount('#event-handling')
```

注意在这个方法中，我们更新了应用的状态，但没有触碰DOM——所有的DOM操作都是由Vue来处理，你编写的代码只需关注逻辑底层即可。

Vue还提供了`v-model`指令，它能轻松实现表单输入和应用状态之间的双向绑定。

```html
<div id="two-way-binding">
  <p>{{ message }}</p>
  <input v-model="message" />
</div>
```

```js
const TwoWayBinding = {
  data() {
    return {
      message: 'Hello Vue!'
    }
  }
}

Vue.createApp(TwoWayBinding).mount('#two-way-binding')
```



## 2.3 条件与循环

控制切换一个元素是否显示也相当简单：

```html
<div id="conditional-rendering">
  <span v-if="seen">现在你看到我了</span>
</div>
```

```js
const ConditionalRendering = {
  data() {
    return {
      seen: true
    }
  }
}

Vue.createApp(ConditionalRendering).mount('#conditional-rendering')
```

这个例子演示了我们不仅可以把数据绑定到DOM文本或 attribute，还可以绑定到 DOM 的结构。此外，Vue 也提供了一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用过渡效果。

还有其它很多指令，每个都有特殊的功能。例如，`v-for`指令可以绑定数组的数据来渲染一个项目列表：

```html
<div id="list-rendering">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
const ListRendering = {
  data() {
    return {
      todos: [
        { text: 'Learn JavaScript' },
        { text: 'Learn Vue' },
        { text: 'Build something awesome' }
      ]
    }
  }
}

Vue.createApp(ListRendering).mount('#list-rendering')
```



## 2.4 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象成一个组件树：

![](https://v3.cn.vuejs.org/images/components.png)

在 Vue 中，组件本质上是一个具有预定义选项的实例。在 Vue 中注册组件很简单：如对`App`对象所做的那样创建一个组件对象，并将其定义在父级组件的`components`选项中：

```js
const TodoItem = {
  template: `<li>This is a todo</li>`
}

// 创建 Vue 应用
const app = Vue.createApp({
  components: {
    TodoItem // 注册一个新组件
  },
  ... // 组件的其它 property
})

// 挂载 Vue 应用
app.mount(...)
```

现在，你可以将其放到另一个组件的模板中：

```html
<ol>
  <!-- 创建一个 todo-item 组件实例 -->
  <todo-item></todo-item>
</ol>
```

但是这样会为每个待办项渲染同样的文本，这看起来并不酷炫。我们应该能将数据从父组件传入子组件才对。让我们来修改一下组件的定义，使之能够接受一个`prop`：

```js
app.component('todo-item', {
  props: ['todo'],
  template: `<li>{{ todo.text }}</li>`
})
```

现在，我们可以使用`v-bind`指令将待办项传到循环输出的每个组件中：

```html
<div id="todo-list-app">
  <ol>
     <!--
      现在我们为每个 todo-item 提供 todo 对象
      todo 对象是变量，即其内容可以是动态的。
      我们也需要为每个组件提供一个“key”，稍后再
      作详细解释。
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```

```js
const TodoList = {
  data() {
    return {
      groceryList: [
        { id: 0, text: 'Vegetables' },
        { id: 1, text: 'Cheese' },
        { id: 2, text: 'Whatever else humans are supposed to eat' }
      ]
    }
  }
}

const app = Vue.createApp(TodoList)

app.component('todo-item', {
  props: ['todo'],
  template: `<li>{{ todo.text }}</li>`
})

app.mount('#todo-list-app')
```

尽管这是一个刻意设计的例子，但是我们已经设法将应用分割成了两个更小的单元。子单元通过prop接口与父单元进行了良好的解耦。我们现在可以进一步改进`<todo-item>`组件，提供更为复杂的模板和逻辑，而不会影响到父应用。



在一个大型应用中，有必要将整个应用程序划分为多个组件，以使开发更易管理。在后面将详述组件，不过这里有一个（假象的）例子，以展示使用了组件的应用模板是什么样的：

```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```



### 2.4.1 与自定义元素的关系

你可能已经注意到 Vue 组件与自定义元素非常类似——它是[Web Components规范](https://www.w3.org/wiki/WebComponents/)的一部分。确实，Vue的组件设计（如插槽API）在浏览器原生支持该规范前就部分受到了它的影响。



它们之间主要的不同在于，Vue组件的数据模型是作为框架的一部分而设计的，而该框架为构建复杂应用提供了很多必要的附加功能。例如响应式模板和状态管理——这两者都没有被该规范所覆盖。



Vue也为创建和使用自定义元素提供了很好的支持。更多请浏览[Vue和Web Components](https://v3.cn.vuejs.org/guide/web-components.html)章节。



# 3. 应用&组件实例

## 3.1 创建一个应用实例

每个 Vue 应用都是通过用`createApp` 创建一个新的**应用实例**开始的：

```js
const app = Vue.createApp({
  /* 选项 */
})
```

该应用实例是用来在应用中注册”全局“组件的。我们会在后面详细讨论，简单的例子：

```js
const app = Vue.createApp({})
app.component('SearchInput', SearchInputComponent)
app.directive('focus', FocusDirective)
app.use(LocalePlugin)
```

可以在[API参考](https://v3.cn.vuejs.org/api/application-api.html)中浏览完整的应用API。



## 3.2 根组件

传递给`createApp`的选项用于配置**根组件**。当我们**挂载**应用时，该组件被用作渲染的起点。

一个应用需要被挂载到一个DOM元素中。例如，如果你想把一个Vue应用挂载到`<div id="app">` ，应传入`#app`：

```js
const RootComponent = { 
  /* 选项 */ 
}
const app = Vue.createApp(RootComponent)
const vm = app.mount('#app')
```

与大多数应用方法不同的是，`mount`不返回应用本身。相反，它返回的是根组件实例。

虽然没有完全遵循[MVVM模型](https://en.wikipedia.org/wiki/Model_View_ViewModel)，但是Vue的设计也受到了它的启发。因此在文档中经常会使用`vm`（ViewModel的缩写）这个变量名表示组件实例。

尽管本页面上的所有实例都只需要一个单一的组件就可以，但是大多数的真实应用都是被组织成一个嵌套的、可重用的组件树。举个例子，一个todo应用组件树可能是这样的：

```
Root Component
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

每个组件将有自己的组件实例`vm`。对于一些组件，如`TodoItem`，在任何时候都可能有多个实例渲染。这个应用中的所有组件实例将共享同一个应用实例。

我们会在稍后的[组建基础](https://v3.cn.vuejs.org/guide/component-basics.html)章节具体展开。不过现在，你只需要明白根组件和其他组件没什么不同，配置选项是一样的，所对应的组件实例行为也是一样的。



## 3.3 组件实例property

在前面的指南中，我们认识了`data`property。在`data`中定义的property是通过组件实例暴露的：

```js
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.count) // => 4
```

还有各种其他的组件选项，可以将用户定义的property添加到组件实例中，例如`methods`，`props`，`computed`，`inject`和`setup`。我们将在后面的指南中深入讨论它们。组件实例的所有property，无论如何定义，都可以在组件的模板中访问。

Vue还通过组件实例暴露了一些内置property，如`$attrs`和`$emit`。这些property都有一个`$`前缀，以避免与用户自定义的property名冲突。



## 3.4 生命周期钩子

每个组件在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到DOM并在数据变化时更新DOM等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己代码的机会。

比如`created`钩子可以用来在一个实例被创建之后执行代码：

```js
Vue.createApp({
  data() {
    return { count: 1}
  },
  created() {
    // `this` 指向 vm 实例
    console.log('count is: ' + this.count) // => "count is: 1"
  }
})
```

也有一些其它的钩子，在实例生命周期的不同阶段被调用，如`mounted`、`updated`和`unmounted`。生命周期钩子的`this`上下文指向调用它的当前活动实例。

> TIP
>
> 不要在选项property或回调上使用*箭头函数*，比如`created:()=>console.log(this.a)`或`vm.$watch('a', newValue=>this.myMethod())`。因为箭头函数没有`this`，`this`会作为变量一直向上级词法作用域查找，直至找到为止，经常导致`Uncaught TypeError: Cannot read property of undefined`或`Uncaught TypeError: this.myMethod is not a function`之类的错误。



## 3.5 生命周期图示

下图展示了实例的生命周期。

![](https://v3.cn.vuejs.org/images/lifecycle.svg)



# 4. 模板语法

Vue.js使用了基于 HTML  的模板语法，允许开发者声明式地将 DOM 绑定至底层组件实例的数据。所有Vue.js的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。



在底层的实现上， Vue 将模板编译成虚拟 DOM 渲染函数。结合响应性系统， Vue 能够智能地计算出最少需要重新渲染多少组件，并把DOM操作次数减到最少。



如果你熟悉虚拟DOM并且偏爱JavaScript的原始力量，你也可以不用模板，[直接写渲染（render）函数](https://v3.cn.vuejs.org/guide/render-function.html)，使用可选的JSX语法。



## 4.1 插值

### 4.1.1 文本

数据绑定最常见的形式就是使用”Mustache“语法（双大括号）的文本插值：

```html
<span>Message: {{ msg }}</span>
```

Mustache标签将会被替代为对应组件实例中`msg`property的值。无论何时，绑定的组件实例上`msg`property发生了改变，插值处的内容都会更新。

通过[v-once指令](https://v3.cn.vuejs.org/api/directives.html#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其他数据绑定：

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```



### 4.1.2 原始HTML

双大括号会将数据解释为普通文本，而非HTML代码。为了输出真正的HTML，你需要使用`v-html`指令：

```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/vue3/原始HTML.png)



这个`span`的内容将会被替换成为property值`rawHtml`，直接作为HTML——会忽略解析property值中的数据绑定。注意，你不能使用`v-html`来复合局部模板，因为Vue不是基于字符串的模板引擎。反之，对于用户界面（UI），组件更适合作为可重用和可组合的基本单位。

> TIP
>
> 在你的站点上动态渲染任意的 HTML 是非常危险的，因为它很容易导致[XSS攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。请只对可信内容使用 HTML 插值，绝不要将用户提供的内容作为插值。



### 4.1.3 Attribute

Mustache语法不能在 HTML attribute 中使用，然而，可以使用`v-bind`指令：

```html
<div v-bind:id="dynamicId"></div>
```

如果绑定的值是`null`或`undefined`，那么该 attribute 将不会被包含在渲染的元素上。

对于布尔 attribute （它们只要存在就意味着值为 true），`v-bind`工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">按钮</button>
```

如果`isButtonDisabled`的值是truthy[^1]，那么`disabled`attribute将被包含在内。如果该值是一个空字符串，它也会被包括在内，与``保持一致。对于其他 falsy[^2]的值，该attribute将被省略。



### 4.1.4 使用JavaScript表达式

迄今为止，在我们的模板中，我们一直都只绑定简单的property键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在当前活动实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含**单个表达式**，所以下面的例子都**不会**生效。

```html
<!--  这是语句，不是表达式：-->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```



## 4.2 指令

指令（Directives）是带有`v-`前缀的特殊attribute。指令attribute的值预期是**单个JavaScript表达式**（`v-for`和`v-on`是例外情况，稍后我们再讨论）。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM。回顾我们在介绍中看到的例子：

```html
<p v-if="seen">现在你看到我了</p>
```

这里，`v-if`指令将根据表达式`seen`的值的真假来插入/移除`<p>`元素。



### 4.2.1 参数

一些指令能够接收一个”参数“，在指令名称之后以冒号表示。例如，`v-bind`指令可以用于响应式地更新HTML attribute：

```html
<a v-bind:href="url"> ... </a>
```

在这里`href`是参数，告知`v-bind`指令将该元素的`href`attribute与表达式`url`的值绑定。

另一个例子是`v-on`指令，它用于监听DOM事件：

```html
<a v-on:click="doSomething"> ... </a>
```

在这里参数是监听的事件名。



### 4.2.2 动态参数

也可以在指令参数中使用JavaScript表达式，方法是用方括号括起来：

```html
<!--
注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。
-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的`attributeName`会被作为一个JavaScript表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的组件实例有一个 data property `attributeName`，其值为`"href"`，那么这个绑定将等价于`v-bind:href`。

同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

在这个示例中，当`eventName`的值为`”focus“`时，`v-on:[eventName]`将等价于`v-on:focus`



### 4.2.3 修饰符

修饰符（modifier）是以半角句号`.`指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent`修饰符告诉`v-on`指令对于触发的事件调用`event.preventDefault()`：

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

在接下来对`v-on`和`v-for`等功能的探索中，你会看到修饰符的其它例子。



## 4.3 缩写

`v-`缩写前缀作为一种视觉提示，用来识别模板中 Vue 特定的 attribute。当你在使用 Vue.js 为现有标签添加动态行为（dynamic behavior）时，v- 前缀很有帮助。然后，对于一些频繁用到的指令来说，就会感到使用繁琐。同时，在构建由 Vue 管理所有模板的单页面应用程序（[SPA - single page application](https://en.wikipedia.org/wiki/Single-page_application)）时，`v-` 前缀也变得没那么重要了。因此，Vue 为`v-bind`和`v-on`这两个最常用的指令，提供了特定简写：



### 4.3.1 `v-bind`缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url"> ... </a>

<!-- 缩写 -->
<a :href="url"> ... </a>

<!-- 动态参数的缩写 -->
<a :[key]="url"> ... </a>
```

 

### 4.3.2 `v-on`缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething"> ... </a>

<!-- 缩写 -->
<a @click="doSomething"> ... </a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

它们看起来可能与普通的HTML略有不同，但`:`与`@`对attribute名来说都是合法字符，在所有支持 Vue 的浏览器都能被正确解析。而且，它们不会出现在最终渲染的标记中。缩写语法是完全可选的，但随着你更深入了解它们的作用，你会庆幸拥有它们。



### 4.3.3 注意事项

#### 4.3.3.1 对动态参数值约定

动态参数预期会求出一个字符串，异常情况下值为`null`。这个特殊的`null`值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。



#### 4.3.3.2 对动态参数表达式约定

动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。

例如：

```html
<!-- 这会触发一个编译警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

变通的办法是使用没有空格或引号的表达式，或用`计算属性`代替这种复杂表达式。



在DOM中使用模板（直接在一个HTML文件里撰写模板），还需要避免使用大写字符来命名键名，因为浏览器会把attribute名全部强制转为小写：

```html
<!--
在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
除非在实例中有一个名为“someattr”的 property，否则代码不会工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```



#### 4.3.3.3 JavaScript表达式

模板表达式都被放在沙盒中，只能访问一个[受限的列表](https://github.com/vuejs/vue-next/blob/master/packages/shared/src/globalsWhitelist.ts#L3)，如`Math`和`Date`。你不应该在模板表达式中试图访问用户定义的全局变量。





# 5. Data Property和方法

## 5.1 Data Property

组件的`data`选项是一个函数。 Vue 在创建新组件实例的过程中调用此函数。它应该返回一个对象，然后 Vue 会 通过响应性系统将其包裹起来，并以`$data`的形式存储在组件实例中。为方便起见，该对象的任何顶级 property 也直接通过组件实例暴露出来：

```js
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})

const vm = app.mount('#app')

console.log(vm.$data.count) // => 4
console.log(vm.count)       // => 4

// 修改 vm.count 的值也会更新 $data.count
vm.count = 5
console.log(vm.$data.count) // => 5

// 反之亦然
vm.$data.count = 6
console.log(vm.count) // => 6
```







[^1]: truthy不是`true`，详见[MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)的解释。



[^2]: falsy不是`false`，详见[MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)的解释。

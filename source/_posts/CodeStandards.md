---
title: 前端代码规范
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
keywords: 前端代码规范
description: 前端代码规范
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---

[原文](https://www.yuque.com/docs/share/332b7c4f-c991-41a1-8822-d5edc7ef153f)

***前端编码规范***

## 序言
编码风格没有太多的好坏之分, 最重要的是风格保持一致，编码规范有助于规范我们编码的风格，使代码具有更好的可读性。本文档风格约定部分可能跟你的喜好有冲突，请尽量用包容的心态来阅读。有任何问题或建议，欢迎跟我们讨论。

## 一、通用规范

### 1.1 `强制` 所有代码统一缩进两个空格

### 1.2 `强制` 使用UTF-8编码

## 二、VUE编码规范

### 2.1 `强制` 所有单文件组件命名统一首字母大写示例：MyComponent.vue

### 2.2 `强制`指令缩写 (统一用 : 表示 v-bind: 和用 @ 表示 v-on:)

示例：
```html
<input
  :value="newTodoText"
  :placeholder="newTodoInstructions"
><input
  @input="onInput"
  @focus="onFocus"
>
```

### 2.3 `强制`使用 v-for 时配合 key 使用
示例：<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>[强制]单文件组件页的私有样式使用 scoped 锁定作用域避免污染到全局样式示例：<style lang="scss" scoped>
.button {
  border: none;
  border-radius: 2px;
}
</style>[建议]组件名应该倾向于完整单词而不是缩写编辑器中的自动补全已经让书写长命名的代价非常之低了，而其带来的明确性却是非常宝贵的。不常用的缩写尤其应该避免。示例：components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue[建议]组件名以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。示例：components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue[建议]在声明 prop 的时候，命名用驼峰命名，在模板和 JSX中用短横线拼接命名。示例：props: {
  greetingText: String
}

<welcome-message greeting-text="hi"/>三、Javascript编码规范空格与符号[强制]二元运算符的两侧统一加一个空格，一元运算符与操作对象之间不要空格示例：var a = !arr.length;
a++;
a = b + c;[强制]用作代码块起始的左花括号 { 前必须有一个空格，并且与前面的代码写在同一行，右花括号 } 换第二行。示例：// good
if (condition) {
}

// bad
if (condition){
}
if (condition) 
{
}[强制]花括号 { 之间需要加一个空格// good
import { button, popup } from 'vant';

// bad
import {button, popup} from 'vant';[强制] if / else / for / while / function / switch / do / try / catch / finally 关键字后，必须有一个空格示例：// good
if (condition) {
}

// bad
if(condition) {
}
[强制]在对象创建时，属性中的冒号 : 之后必须有空格，冒号 : 之前不允许有空格示例：// good
var obj = {
  a: 1,
  b: 2,
  c: 3
};

// bad
var obj = {
  a : 1,
  b:2,
  c :3
};[强制]在函数声明、具名函数表达式、函数调用中，函数名和左圆括号 ( 之间不允许有空格。示例：// good
function funcName() {
}
var funcName = function funcName() {
};
funcName();

// bad
function funcName () {
}
var funcName = function funcName () {
};
funcName ();[强制] 逗号 , 和分号 ; 前不允许有空格，逗号 , 之后统一加一个空格示例：// good
callFunc(a, b);

// bad
callFunc(a , b) ;
callFunc(a,b) ;[强制] () 和 [] 内紧贴括号部分不允许有空格示例：// good
save(this.list[this.indexes[i]]);

// bad
callFunc( param1, param2, param3 );
save( this.list[ this.indexes[ i ] ] );[强制] 语句结束统一加分号 ; 函数结束统一不加分号（函数表达式除外）注释[强制] 函数注释统一使用 /**/ 的写法并且标明参数与返回值（如果有返回值的话）参数需要标明参数的数据类型返回值尽量标明值、类型及描述示例：/**
 * @param {string} id 参数描述
 * @param {boolean} isTrue 参数描述
 * @return {objtct} 返回值描述
 */[建议] 单行注释使用 // 尽量独占一行，如果有多行注释尽量使用多个单行注释进行其他[强制]变量、函数的命名统一使用驼峰命名[建议]每行代码尽量不超过120个字符四、HTML编码规范[强制]标签需要对齐，必须有开始标签和结束标签，如果是空标签要加/示例：// good
<tr>
  <td>A</td>
  <td>Description of A</td>
</tr>[强制]标签与属性使用小写英文字母[强制]class的命名使用短括号连接方式示例<div class="button-add"></div>[强制]html标签中使用双引号引入属性示例：// good
<div class="button-add"></div>

//bad
<div class='button-add'></div>
<div class=button></div>[建议]图片尽量添加 alt 属性添加 alt 属性，在图片不能显示时，它能替代图片显示五、CSS编码规范[强制]选择器 与 { 之间必须包含空格示例：// good
.selector {
}

// bad
.selector{
}[强制]当长度单位为0时后面不加px示例：// good
margin: 0;

// bad
margin: 0px;[强制]每行样式结尾统一加分号示例：// good
margin: 0;

// bad
margin: 0若有收获，就点个赞吧陈宇鑫05-21 09:42180投诉加入语雀，参与知识分享与交流注册 或 登录 语雀进行评论立即加入回复注册 或 登录 语雀进行评论关于语雀使用帮助数据安全服务协议English快速注册
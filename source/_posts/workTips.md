---
title: 日常小知识点
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
keywords: js
description: js
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---

## 包含数字字母字符串，6-12位
```javascript
/^(?=.*[a-zA-Z])(?=.*\d)(?=.*[~!@#$%^&*()_+`\-={}:";'<>?,.\/]).{6,12}$/
```

## 对中文进行排序
```javascript
let arr = [
  {name: 'zs', age: 1},
  {name: 'zs', age: 2},
  {name: 'ls', age: 3},
  {name: 'ww', age: 4},
  {name: 'zs', age: 5},
  {name: 'ww', age: 6},
  {name: 'ls', age: 7},
  {name: 'zs', age: 8}
]
let new_arr = arr.sort((a,b) => {
  return b.name.charCodeAt(0) - a.name.charCodeAt(0)
})
```

## post请求不希望url拼接参数，但参数形式Query String Parameters

希望是：
![](https://segmentfault.com/img/bVbAFBd)

现实是：
![](https://segmentfault.com/img/bVbAFBz)

> 使用params作参数名会自动拼接到url上面，去axios那里把参数名设置为data就好了



## ??,?.,??=

###  `?.`（可选链）

> 举例

```javascript
let a;
let b = a.name;
```

![](https://img-blog.csdnimg.cn/20210319202635609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1bl9tYXN0ZXI=,size_16,color_FFFFFF,t_70)



这种情况就相当于直接在`undefined`上面访问`name`属性。`undefined`和`null`是两个比较特殊的数据类型，是不能用点操作符去访问属性的。那么在一个变量可能为`null`、或者`undefined`的时候，恰巧我又需要访问这个变量的一个属性，那我们应该这样做

```js
let a;
let b;
if (!!a) {
	b = a.name;
} else {
	b = undefined;
}
```

> 简单的方法

```js
let a;
let b = a?.name;
```



###  `??`（空值合并运算符）

> 举例

```js
let b;
let a = 0;
let c = { name:'buzhimingqianduan' }

if (!!a || a === 0 ){
	b = a;
} else {
	b = c;
}
```

当我们想判断一个值存在，但是它等于0的时候，我们也需要当作它存在，于是就有上面的例子，其实我们可以这样做

```js
let b;
let a = 0;
let c = { name: 'aa' };

b = a ?? c;
```

上面的例子中，当`a`除了`undefined`、或者`null`之外的任何值，`b`都会等于`a`，否则就等于`c`。



### `??=`（空值赋值运算符）

```js
let b = '你好';
let a = 0
let c = null;
let d = ’123‘
b ??= a;  // b = “你好”
c ??= d  // c = '123'

```

当`??=`左侧的值为`null`、`undefined`的时候，才会将右侧变量的值赋值给左侧变量。其他所有值都不会进行赋值。同样在一些场景下，可以省略很多代码。


## Vue项目在IE下缓存的问题
[解决vue在IE11读取缓存的问题](解决vue在IE11读取缓存的问题)

> IE11get，post请求时会读取接口缓存，我们要使用时间戳拼接在url来解决。最好不要直接加载参数上，怕后端会进行设置。
```js
function filterUrl (url) {
  return url.indexOf('?') !== -1 ? `${url}&time=${new Date().getTime()}` : `${url}?time=${new Date().getTime()}`
}
```

```js
request.interceptors.request.use(
  (config) => {
    if (config.method === 'get') {
      // 加上这一句
      config.url = filterUrl(config.url);
      
      config.paramsSerializer = (params) => {
        return qs.stringify(params, { arrayFormat: 'brackets' });
      };
    }
    return config;
  },
  (err) => {
    return Promise.reject(err);
  }
);
```



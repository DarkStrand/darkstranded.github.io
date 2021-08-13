---
title: Nodejs
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: hojun.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-08-12 19：05：08
comments: true
tags: 
 - 后端
keywords: node
description: nodejs教程
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/5.jpeg

---


# 01-Node.js基础

## 一、Node.js是什么

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.



### 1、 特性

Node.js可以解析JS代码（没有浏览器安全级别的限制）提供很多系统级别的API，如：

- 文件的读写（File System）
- 进程的管理（Process）
- 网络通信（HTTP/HTTPS）



### 2、举例

#### 2.1 浏览器安全级别的限制

##### Ajax限制

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>browser-safe-sandbox</title>
</head>
<body>
  <div>browser-safe-sandbox</div>
  <script>
    const xhr = new XMLHttpRequest()
    xhr.open('get', 'https://m.maoyan.com/ajax/moreClassicList?sortId=1&showType=3&limit=10&offset=30&optimus_uuid=A5518FF0AFEC11EAAB158D7AB0D05BBBD74C9789D9F649898982E6542C7DD479&optimus_risk_level=71&optimus_code=10', false)
    xhr.send()
  </script>
</body>
</html>
```



##### 浏览器预览

```
browser-sync start --server --files **/* --directory
```





#### 2.2 文件的读写（File System）

```javascript
const fs = require('fs')

fs.readFile('./ajax.png', 'utf-8', (err, content) => {
  console.log(content)
})
```



#### 2.3 进程的管理（Process）

```javascript
function main(argv) {
  console.log(argv)
}

main(process.argv.slice(2))
```



> 运行

```
node 2.3-process.js argv1 argv2
```



#### 2.4 网络通信（HTTP/HTTPS）

```javascript
const http = require("http")

http.createServer((req,res) => {
  res.writeHead(200, {
    "content-type": "text/plain"
  })
  res.write("hello nodejs")
  res.end()
}).listen(3000)
```





## 二、Node相关工具

### 1、NVM：Node Version Manager

#### 1.1 Mac安装nvm

```
https://github.com/nvm-sh/nvm/blob/master/README.md
```



#### 1.2 Windows安装nvm

```
nvm-windows
nodist
```



#### 1.3 常用的nvm命令

- `nvm list`：查看当前环境安装了哪些版本
- `nvm use 14.15.0`：切换node版本
- `nvm alias default (v)14.15.0`：切换node默认版本



### 2、NPM：Node Package Manager

- `npm view jquery versions`：查看包的所有版本



#### 2.1 全局安装package

```
$ npm install forever --global (-g)
$ forever
$ npm uninstall forever --global
$ forever

```



> 全局安装包的目录

- Mac

  ```
  /Users/felix/.nvm/versions/node/nvm各个版本/bin/
  
  ```

  

- Windows

  ```
  C:\Users\你的用户名\AppData\Roaming\npm\node_modules
  
  ```



#### 2.2 本地安装package

```
$ cd ~/desktop
$ mkdir gp-project
$ cd gp-project
$ npm install underscore
$ npm list (ls)

```



#### 2.3 package.json初始化

```
$ pwd
$ npm init -y
$ ls
$ cat package.json

```



#### 2.4 使用package.json

- `npm install —production`：只拉取生产环境的包

```
$ npm install underscore --save
$ cat package.json
$ npm install lodash --save-dev
$ cat package.json
$ rm -rf node_modules
$ ls
$ npm install
$ npm uninstall underscore --save
$ npm list | grep underscore  // 查看underscore包的树(依赖关系)
$ cat package.json

```





> "dependencies"：这些包是你的应用程序在生产环境中所需要的。
>
> "devDepedencies"：这些包只是在开发和测试中需要的。



![npm](https://img-blog.csdn.net/2018071517553381?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p3bF93aWxsb24=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
// package.json
{
...
"dependencies": { // --save / -S
},
"devDependencies": { // --save-dev / -D
}

}

```





#### 2.5 安装指定版本的包

- `npm install jquery@2.2.4`： 安装指定版本
- `npm install jquery@1 -S`：安装1最高的版本

```
$ pwd
$ npm list
$ npm info underscore
$ npm view underscore versions
$ npm install underscore@1.8.0
$ npm list
$ npm uninstall underscore
$ npm list

```



#### 2.6 更新本地安装的包

> -13.4.6
>
> major: 13（主版本号）,minor: 4（次版本号），patch：6（补丁，单数不稳定，双数稳定）

- `npm outdated`：查看哪些包过期
- `npm update`：更新所有的包

```
$ npm info underscore
$ npm view underscore versions // 查看underscore包所有的版本
$ npm install underscore@1.4.4 --save-dev // 安装1.4.4的underscore到开发环境 
$ npm list | grep gulp // 查看gulp包的依赖关系
$ npm outdated //~2.0.0表示patch, ^2.0.0表示minor * 表示xx最新版本
$ npm list | grep gulp
$ npm update

```



```json
{
	"dependencies": {
		"jquery": "^1.12.4", // ^锁定主版本号
        "jquery": "~1.12.4", // ~锁定主版本号和次版本号
        jquery: "1.12.4", // 全部锁定
        jquery: "*" // 最新版本
    }
}

```



#### 2.7 清除缓存

- `npm cache clean --force`



#### 2.8 上传自己的包

##### 2.8.1 编写模块

保存为index.js

```javascript
exports.sayHello = function(){ 
  return 'Hello World'; 
}

```



##### 2.8.2 初始化描述文件

- npm init package.json

```json
{ 
  "name": "gp19-npm", 
  "version": "1.0.1", 
  "description": "gp19 self module", 
  "main": "index.js",
  "scripts": { 
    "test": "make test" 
  }, 
  "repository": { 
    "type": "Git", 
    "url": "git+https://github.com/lurongtao/gp19-npm.git" 
  }, 
  "keywords": [ 
    "demo" 
  ], 
  "author": "Felixlu", 
  "license": "ISC", 
  "bugs": { 
    "url": "https://github.com/lurongtao/gp19-npm/issues" 
  }, 
  "homepage": "https://github.com/lurongtao/gp19-npm#readme", 
}

```



##### 2.8.3 注册npm仓库账号

- npm adduser

```
https://www.npmjs.com 上面的账号
felix_lurt/qqmko09ijn
$ npm adduser

```



##### 2.8.4 上传包

- npm publish



坑：403 Forbidden

```
查看npm源：npm config get registry
切换npm源方法一：npm config set registry http://registry.npmjs.org
切换npm源方法二：nrm use npm

```



##### 2.8.5 安装包

- npm install gp19-npm



##### 2.8.6 卸载包

```
查看当前项目引用了哪些包 ：
npm ls
卸载包：
npm unpublish --force

```



##### 2.8.7 使用引入包

```
var hello = require('gp19-npm')
hello.sayHello()

```



#### 2.9 npm脚本

Node开发离不开npm，而脚本功能是npm最强大、最常用的功能之一。



##### 2.9.1 什么是npm脚本？

npm允许在package.json文件里面，使用scripts字段定义脚本命令。

```json
{
	// ...
    "scripts": {
		"builds": "node build.js"
    }
}

```



##### 2.9.2 执行顺序

如果npm脚本里面需要执行多个任务，那么需要明确它们的执行顺序。

> scripts1.js

```js
var x = 0
console.log(x)

```



> scripts2.js

```js
var y = 0
console.log(y)

```



```json
"scripts" : {
	"script1": "node script1.js",
    "script2": "ndoe script2.js"
}

```



如果是并行执行（即同时的平行执行），可以使用`&`符号。

```
$ npm run script1 & npm run script2

```



如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用`&&`符号。

```
$ npm run script1 && npm run script2

```



##### 2.9.3 简写形式

常用的npm脚本简写形式

```
npm start 是 npm run start

```





##### 2.9.4 变量

npm脚本有一个非常强大功能，就是可以使用npm的内部变量。

首先，通过`npm _package_`前缀，npm脚本可以拿到package.json里面的字段。比如，下面是一个package.json。



> 注意：一定要在npm脚本中运行（如：npm run view）才可以，直接在命令行中运行JS（如：node view.js）是拿不到的

```json
{
	"name": "foo",
    "version": "1.2.5",
    "scripts": {
		"view": "node view.js"
    }
}

```

那么，变量`npm_package_name`返回foo，变量`npm_package_version`返回1.2.5。

```js
// view.js
console.log(process.env.npm_package_name); // foo
console.log(process.env.npm_package_version); // 1.2.5

```

上面代码中，我们通过环境变量`process.env`对象，拿到`package.json`的字段值。如果是Bash脚本，可以用`$npm_package_name`和 `$npm_package_version`取到这两个值。



npmpackage前缀也支持嵌套的package.json字段。

```json
"repository": {
	"type": 'git',
    "url": "xxx"
},
"scripts": {
	"view": "echo $npm_package_repository_type"
}

```

上面代码中，repository字段的type属性，可以通过`npm_package_repository_type`取到。



下面是另外一个例子。

```json
"scripts": {
	"install": "foo.js"
}

```

上面代码中，`npm_package_scripts_install`变量的值等于foo.js。

然后，npm脚本还可以通过`npmconfig`前缀，拿到npm的配置变量，即`npm config get xxx`命令返回的值。比如，当前模块的发型标签，可以通过`npm_config_tag`取到。

```json
"view": "echo $npm_config_tag",

```



注意，package.json里面的config对象，可以被环境变量覆盖。

```json
{
	"name": "foo",
    "config": {"port": "8080"},
    "scripts": {"start": "node server.js"}
}

```

上面代码中，`npm_package_config_port`变量返回的是8080。这个值可以用下面的方法覆盖。

```
$ npm config set foo:port 80

```

最后，env命令可以列出所有环境变量。

"env":"env" 





#### 2.10 npm安装git上发布的包

```
# 这样适合安装公司内部的git服务器上的项目
npm install git+https://git@github.com:lurongtao/gp-project.git

# 或者以ssh的方式
npm install git+ssh://git@github.com:lurongtao/gp-project.git

```



#### 2.11 cross-env使用

##### 2.11.1 cross-env是什么

运行跨平台设置和使用环境变量的脚本



##### 2.11.2 出现原因

当您使用`NODE_ENV=production`，来设置环境变量时，大多数Windows命令提示将会阻塞（报错）。（异常是Windows上的Bash，它使用本机Bash。）换言之，Windows不支持`NOE_ENV=production`的设置方式。



##### 2.11.3 解决

cross-env使得您可以使用单个命令，而不必担心为平台正确设置或使用环境变量。这个迷你的包（cross-env）能够提供一个设置环境变量的scripts，让你能够以Unix方式设置环境变量，然后在Windows上也能兼容运行。



##### 2.11.4 安装

```
npm install --save-dev cross-env

```



##### 2.11.5 使用

```json
{
	"scripts": {
		"build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
    }
}

```

`NODE_ENV`环境变量将由`cross-env`设置，打印`process.env.NODE_ENV === 'production'`



### 3、NRM：npm registry manager

#### 3.1 手工切换源

##### 3.1.1 查看当前源

```
npm config get registry

```



##### 3.1.2 切换淘宝源

```
npm config set registry https://registry.npm.taobao.org

```



#### 3.2 NRM管理源

NRM是npm的镜像源管理工具，有时候国外资源太慢，使用这个就可以快速地在npm源间切换。



##### 3.2.1 安装nrm

在命令行执行命令，`npm install -g nrm`，全局安装nrm。



##### 3.2.2 使用nrm

执行命令`nrm ls`查看可选的源。其中，带`*`的是当前使用的源，上面输出表明当前源是官方源。



##### 3.2.3 切换nrm

如果要切换到taobao源，执行命令`nrm use taobao`。



##### 3.2.4 测试速度

你还可以通过`nrm test`测试相应源的响应时间。



### 4、 NPX：npm package extention

npm从5.2版开始，增加了npx命令。它有很多用处，本文介绍该命令的主要使用场景。

Node自带npm模块，所以可以直接使用npx命令。万一不能用，就要手动安装一下。

```
$ npm install -g npx

```



#### 4.1 调用项目安装的模块

npx想要解决的主要问题，就是调用项目内部安装的模块。比如，项目内部安装了Mocha。

```
$ npm install -D mocha

```



一般来说，调用Mocha，只能在项目脚本和package.json的scripts字段里面，如果想在命令行下调用，必须像下面这样。

```
# 项目的根目录下执行
$ node-modules/.bin/mocha --version

```

npx就是想解决这个问题，让项目内部安装的模块用起来更方便，只要像下面这样调用就行了。

```
$ npx mocha --version

```

npx的原理很简单，就是运行的时候，会倒`node_modules/.bin`路径和环境变量`$PATH`里面，检查命令是否存在。



由于npx会检查环境变量$PATH，所以系统命令也可以调用。

```
# 等同于 ls
$ npx ls

```

注意，Bash内置的命令不在$PATH里面，所以不能用。比如cd 是Bash命令，因此就不能用npx cd。



#### 4.2 避免全局安装模块

除了调用项目内部模块，npx还能避免全局安装的模块。比如，create-react-app这个模块是全局安装，npx可以运行它，而且不进行全局安装。

```
$ npx create-react-app my-react-app

```

上面代码运行时，npx将create-react-app下载到一个临时目录，使用以后再删除。所以，以后再次执行上面的命令，会重新下载create-react-app。



注意，只要npx后面的模块无法在本地发现，就会下载同名模块。比如，本地没有安装http-server模块，下面的命令会自动下载该模块，在当前目录启动一个Web服务。

```
$ npx http-server

```



#### 4.3 --no--install参数和--ignore-existing参数

如果想让npx强制使用本地模块，不下载远程模块，可以使用--no-install参数。如果本地不存在该模块，就会报错。

```
$ npx --no-install http-server

```

反过来，如果忽略本地的同名模块，强制安装使用远程模块，可以使用--ignore-existing参数。比如，本地已经安装了http-server，但还是想使用远程模块，就用这个参数。

```
$ npx --ignore-existing http-server

```



### 5、node的浏览端调试

- `node --inspect --inspect-brk server.js`



### 6、node进程管理工具

- supervisor

- nodemon

  ```
  npm install nodemon
  nodemon server.js
  
  ```

  

- forever

- pm2



## 三、模块/包与CommonJS

### 1、模块/包分类

Node.js有三类模块，即内置的模块、第三方的模块、自定义的模块。



#### 1.1 内置的模块

Node.js内置模块又叫核心模块，Node.js安装完成可直接使用。如：

```js
const path = require('path')
var extname = path.extname('index.html')
console.log(extname)

```



#### 1.2 第三方的Node.js模块

第三方的Node.js模块指的是为了实现某些功能，发布的npmjs.org上的模块，按照一定的开源协议供社群使用。如：

```
npm install chalk

```

```js
const chalk = require('chalk')
console.log(chalk.blue('Hello world!'))

```



#### 1.3 自定义的Node.js模块

自定义的Node.js模块，也叫文件模块，是我们自己写的供自己使用的模块。同时，这类模块发布到npmjs.org上就成了开源的第三方模块。



自定义模块是在运行时动态加载，需要完整的路径分析、文件定位、编译执行过程、速度相比核心模块稍微慢一些，但是用的非常多。



##### 1.3.1 模块定义、接口暴露和引用接口

我们可以把公共的功能抽离成为一个单独的js文件作为一个模块，默认情况下面这个模块里面的方法或者属性，外面是没法访问的。如果要让外部可以访问模块里面的方法或者属性，就必须在模块里面通过`exports`或者`module.exports`暴露属性或者方法。



> m1.js

```js
const name = 'gp19'

const sayName = () => {
	console.log(name)
}

console.log('module 1')

// 接口暴露方法一：
module.exports = {
	say: sayName
}

// 接口暴露方法二：
exports.say = sayName

// 错误！
exports = {
	say: sayName
}

```



> main.js

```js
const m1 = require('./m1')
m1.say()

```



##### 1.3.2 模块的循环引用

由于exports使用方式不对，会在两个不同js循环引用的情况下，导致其中一个js无法获取另外一个js的方法，从而导致执行报错。如：

- a.js

```js
exports.done = false
const b = require('./b.js')
console.log('in a, b.done = %j', b.done)
exports.done = true
console.log('a done')

```



- b.js

```js
console.log('b starting')
exports.done = false
const a = require('./a.js')
console.log('in b, a.done= %j', a.done)
exports.done = true
console.log('b done')

```



- main.js

```js
console.log('main starting')
const a = require('./a.js')
const b = require('./b.js')
console.log('in main, a.done = %j, b.done = %j', a.done, b.done)

```

`main.js`首先会load `a.js`，此时执行到`const b = require('./b.js');`的时候，程序会转去load`b.js`, 在`b.js`中执行到`const a = require('./a.js');` 为了防止无限循环，将`a.js`exports的未完成副本返回到`b.js`模块。然后`b.js`完成加载，并将其导出对象提供给`a.js`模块。

我们知道nodeJs的对每个js文件进行了一层包装称为`module`，`module`中有一个属性`exports`，当调用`require('a.js')`的时候其实返回的是`module.exports`对象，`module.exports`初始化为一个`{}`空的object，所以在上面的例子中，执行到`b.js`中`const a = require('./a.js');`时不会load新的`a module`, 而是将已经load但是还未完成的`a module`的`exports`属性返回给`b module`，所以`b.js`拿到的是`a module`的`exports`对象，即：`{done:false}`, 虽然在`a.js`中`exports.done`被修改成了`true`，但是由于此时`a.js`未`load`完成，所以在`b.js`输出的`a module`的属性`done`为`false`，而在`main.js`中输出的`a module`的属性`done`为`true`. Nodejs通过上面这种返回未完成exports对象来解决循环引用的问题。



## 四、常用内置模块

这里介绍几个常用的内置模块：url，querystring，http，events，fs，stream，readline，crypto，zlib



### 1、url



#### 1.1 parse

> 要解析的内容，是否查询字符串

`url.parse(urlString[,parseQueryString[,slashesDenoteHost]])`

```js
const url = require('url')
const urlString = 'https://www.baidu.com:443/ad/index.html?id=8&name=mouse#tag=110'
const parsedStr = url.parse(urlString)
console.log(parsedStr)

=>

Url {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com:443',
  port: '443',
  hostname: 'www.baidu.com',
  hash: '#tag=110',
  search: '?id=8&name=mouse',
  query: [Object: null prototype] { id: '8', name: 'mouse' },
  pathname: '/ad/index.html',
  path: '/ad/index.html?id=8&name=mouse',
  href: 'https://www.baidu.com:443/ad/index.html?id=8&name=mouse#tag=110'
}

```



#### 1.2 format

> 将一个解析后的URL对象、转成、一个格式化的URL字符串。

`url.format(urlObject)`

```js
const url = require('url')
const urlObject = {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com:443',
  port: '443',
  hostname: 'www.baidu.com',
  hash: '#tag=110',
  search: '?id=8&name=mouse',
  query: { id: '8', name: 'mouse' },
  pathname: '/ad/index.html',
  path: '/ad/index.html?id=8&name=mouse',
  href: 'https://www.baidu.com:443/ad/index.html?id=8&name=mouse#tag=110'
}
const parsedObj = url.format(urlObject)
console.log(parsedObj)

=> 
'https://www.baidu.com:443/ad/index.html?id=8&name=mouse#tag=110'

```



#### 1.3 resolve

> 用于拼接URL（替换 域名后面第一个`/`后的内容,如果出现`.`就向上返回一级之后再拼接，两个`..`就向上反两级再拼接）

`url.resolve(from, to)`

```
const url = require('url')
var a = url.resolve('/one/two/three', 'four') 
var b = url.resolve('http://example.com/', '/one')
var c = url.resolve('http://example.com/one', '/two');
var d = url.resolve('http://example.com/one/ddd/ddd/ddd', './two');
var e = url.resolve('http://example.com/one/ddd/ddd/ddd', '../two');
var f = url.resolve('http://example.com/one/ddd/ddd/ddd', '.../two');
console.log(a +","+ b +","+ c+','+d+','+e+','+f);

=>
/one/two/four,
http://example.com/one,
http://example.com/two,
http://example.com/one/ddd/ddd/two,
http://example.com/one/ddd/two
http://example.com/one/ddd/ddd/.../two

```



### 2、querystring

#### 2.1 parse

> 将字符串转成对象。说白了其实就是把url上带的参数串转成数组对象。

`querystring.parse(str[, sep[, eq[, options]]])`

- `str`：欲转换的字符串
- `sep`：设置分隔符，默认为`&`
- `eq`：设置赋值符，默认为`=`
- `[options]`maxKeys：可接受字符串的最大长度，默认为1000

```js
const querystring = require('querystring')
var qs = 'foo=bar&baz=qux&baz=quux&corge'
var parsed = querystring.parse(qs)
console.log(parsed)

=>

{ foo: 'bar', baz: ['qux', 'quux'], corge: '' }

```



#### 2.2 stringify

> 将对象转换成字符串，字符串里多个参数将用 ‘&' 分隔，将用 ‘=' 赋值。

`querystring.stringify(obj[, sep[, eq[, options]]])`

- `obj`：欲转换的对象
- `sep`：设置分隔符，默认为`&`
- `eq`：设置赋值符，默认为`=`

```js
const querystring = require('querystring')
var qo = { foo: 'bar', baz: ['qux', 'quux'], corge: '' }
var parsed = querystring.stringify(qo)
console.log(parsed)

==>
'foo=bar&baz=qux&baz=quux&corge='
 

const querystring = require('querystring')
var qo = {foo: 'bar', baz: 'qux'}
var parsed =querystring.stringify(qo, ';', ':')
console.log(parsed)

==>
'foo:bar;baz:qux'

```



#### 2.3 escape/unescape

> 以针对网址查询字符串的特定要求优化的方式对给定的 `str` 执行网址百分比编码
>
> `querystring.escape()` 方法被 `querystring.stringify()` 使用，通常不会被直接使用。 导出它主要是为了允许应用程序代码在必要时通过将 `querystring.escape` 分配给替代函数来提供替换的百分比编码实现。

`querystring.escape(str)`

```js
const querystring = require('querystring')
var str = 'id=3&city=北京&url=https://www.baidu.com'
var escaped = querystring.escape(str)
console.log(escaped)

==> 'id%3D3%26city%3D%E5%8C%97%E4%BA%AC%26url%3Dhttps%3A%2F%2Fwww.baidu.com'

```



> 在给定的 `str` 上执行网址百分比编码字符的解码。
>
> `querystring.unescape()` 方法被 `querystring.parse()` 使用，通常不会被直接使用。 导出它主要是为了允许应用程序代码在必要时通过将 `querystring.unescape` 分配给替代函数来提供替代的解码实现。
>
> 默认情况下，`querystring.unescape()` 方法将尝试使用 JavaScript 内置的 `decodeURIComponent()` 方法进行解码。 如果失败，则将使用更安全的不会因格式错误的网址而抛出错误的同类方法。

`querystring.unescape(str)`

```js
const querystring = require('querystring')
var str = 'id%3D3%26city%3D%E5%8C%97%E4%BA%AC%26url%3Dhttps%3A%2F%2Fwww.baidu.com'
var unescaped = querystring.unescape(str)
console.log(unescaped)

==>
'id=3&city=北京&url=https://www.baidu.com'

```



### 3、http/https

#### 3.1 get

```js
var http = require('http')
var https = require('https')

// 1、接口 2、跨域
const server = http.createServer((request, response) => {
  var url = request.url.substr(1)

  var data = ''

  
  response.writeHeader(200, {
    'content-type': 'application/json;charset=utf-8',
    'Access-Control-Allow-Origin': '*'
  })

  //response.write('<div>hello</div>')
  //response.end()
  // 或
  //response.end('<div>hello</div>')
    
  https.get(`https://m.lagou.com/listmore.json${url}`, (res) => {

    res.on('data', (chunk) => {
      data += chunk
    })

    res.on('end', () => {
      response.end(JSON.stringify({
        ret: true,
        data
      }))
    })
  })

})

server.listen(8080, () => {
  console.log('localhost:8080')
})

```



#### 3.2 post:服务器提交（攻击）

```js
const https = require('https')
const querystring = require('querystring')

const postData = querystring.stringify({
  province: '上海',
  city: '上海',
  district: '宝山区',
  address: '同济支路199号智慧七立方3号楼2-4层',
  latitude: 43.0,
  longitude: 160.0,
  message: '求购一条小鱼',
  contact: '13666666',
  type: 'sell',
  time: 1571217561
})

const options = {
  protocol: 'http:',
  hostname: 'localhost',
  method: 'POST',
  port: 3000,
  path: '/index.php/trade/add_item',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
    'Content-Length': Buffer.byteLength(postData)
  }
}

const server = http.createServer((req, res) => {
	const request = http.request(options, result => {
		
    })
    req.write(postData)
    req.end()
    
    res.end()
})
server.listen(8080, ()=> {
	console.log('localhost:8080')
})

//function doPost() {
//  let data

//  let req = https.request(options, (res) => {
//    res.on('data', chunk => data += chunk)
//    res.on('end', () => {
//      console.log(data)
//    })
//  })

//  req.write(postData)
//  req.end()
//}

// setInterval(() => {
//   doPost()
// }, 1000)

```



#### 3.3 跨域：JSONP

```js
const http = require('http')
const url = require('url')

const app = http.createServer((req, res) => {
  let urlObj = url.parse(req.url, true)

  switch (urlObj.pathname) {
    case '/api/user':
      res.end(`${urlObj.query.cb}({"name": "gp145"})`)
      break
    default:
      res.end('404.')
      break
  }
})

app.listen(8080, () => {
  console.log('localhost:8080')
})

```



#### 3.4 跨域：CORS

```js
const http = require('http')
const url = require('url')
const querystring = require('querystring')

const app = http.createServer((req, res) => {
  let data = ''
  let urlObj = url.parse(req.url, true)

  res.writeHead(200, {
    'content-type': 'application/json;charset=utf-8',
    'Access-Control-Allow-Origin': '*'
  })

  req.on('data', (chunk) => {
    data += chunk
  })

  req.on('end', () => {
    responseResult(querystring.parse(data))
  })

  function responseResult(data) {
    switch (urlObj.pathname) {
      case '/api/login':
        res.end(JSON.stringify({
          message: data
        }))
        break
      default:
        res.end('404.')
        break
    }
  }
})

app.listen(8080, () => {
  console.log('localhost:8080')
})

```



#### 3.5 跨域：middleware(http-proxy-middware)

```js
const http = require('http')
const proxy = require('http-proxy-middleware')

http.createServer((req, res) => {
  let url = req.url

  res.writeHead(200, {
    'Access-Control-Allow-Origin': '*'
  })

  if (/^\/api/.test(url)) {
    let apiProxy = proxy('/api', { 
      target: 'https://m.lagou.com',
      changeOrigin: true,
      pathRewrite: {
        '^/api': ''
      }
    })

    // http-proy-middleware 在Node.js中使用的方法
    apiProxy(req, res)
  } else {
    switch (url) {
      case '/index.html':
        res.end('index.html')
        break
      case '/search.html':
        res.end('search.html')
        break
      default:
        res.end('[404]page not found.')
    }
  }
}).listen(8080)

```



#### 3.6 爬虫

```js
const https = require('https')
const http = require('http')
const cheerio = require('cheerio')

http.createServer((request, response) => {
  response.writeHead(200, {
    'content-type': 'application/json;charset=utf-8'
  })

  const options = {
    protocol: 'https:',
    hostname: 'maoyan.com',
    port: 443,
    path: '/',
    method: 'GET'
  }

  const req = https.request(options, (res) => {
    let data = ''
    res.on('data', (chunk) => {
      data += chunk
    })

    res.on('end', () => {
      filterData(data)
    })
  })

  function filterData(data) {
    let $ = cheerio.load(data)
    let $movieList = $('.movie-item')
    let movies = []
    $movieList.each((index, value) => {
      movies.push({
        title: $(value).find('.movie-title').attr('title'),
        score: $(value).find('.movie-score i').text(),
      })
    })

    response.end(JSON.stringify(movies))
  }

  req.end()
}).listen(9000)

```



### 4、Events

```js
const EventEmitter = require('events')

class MyEventEmitter extends EventEmitter {}

const event = new MyEventEmitter()

event.on('play', (movie) => {
  console.log(movie)
})

event.emit('play', '我和我的祖国')
event.emit('play', '中国机长')

```



### 5、File System

```js
const fs = require('fs')
const fsP = require('fs').promises

// 创建文件夹
fs.mkdir('./logs', (err) => {
  console.log('done.')
})

// 文件夹改名
fs.rename('./logs', './log', () => {
  console.log('done')
})

// 删除文件夹
fs.rmdir('./log', () => {
  console.log('done.')
})

// 写内容到文件里
fs.writeFile(
  './logs/log1.txt',
  'hello',
  // 错误优先的回调函数
  (err) => {
    if (err) {
      console.log(err.message)
    } else {
      console.log('文件创建成功')
    }
  }
)

// 给文件追加内容
fs.appendFile('./logs/log1.txt', '\nworld', () => {
  console.log('done.')
})

// 读取文件内容
fs.readFile('./logs/log1.txt', 'utf-8', (err, data) => {
  console.log(data)
})

// 删除文件
fs.unlink('./logs/log1.txt', (err) => {
  console.log('done.')
})

// 批量写文件
for (var i = 0; i < 10; i++) {
  fs.writeFile(`./logs/log-${i}.txt`, `log-${i}`, (err) => {
    console.log('done.')
  })
}

// 读取文件/目录信息
fs.readdir('./', (err, data) => {
  data.forEach((value, index) => {
    fs.stat(`./${value}`, (err, stats) => {
      // console.log(value + ':' + stats.size)
      console.log(value + ' is ' + (stats.isDirectory() ? 'directory' : 'file'))
    })
  })
})

// 同步读取文件
try {
  const content = fs.readFileSync('./logs/log-1.txt', 'utf-8')
  console.log(content)
  console.log(0)
} catch (e) {
  console.log(e.message)
}

console.log(1)

// 异步读取文件：方法一
fs.readFile('./logs/log-0.txt', 'utf-8', (err, content) => {
  console.log(content)
  console.log(0)
})
console.log(1)

// 异步读取文件：方法二
fs.readFile('./logs/log-0.txt', 'utf-8').then(result => {
  console.log(result)
})

// 异步读取文件：方法三
function getFile() {
  return new Promise((resolve) => {
    fs.readFile('./logs/log-0.txt', 'utf-8', (err, data) => {
      resolve(data)
    })
  })
}

;(async () => {
  console.log(await getFile())
})()

// 异步读取文件：方法四
const fsp = fsP.readFile('./logs/log-1.txt', 'utf-8').then((result) => {
  console.log(result)
})

console.log(fsP)

// watch 监测文件变化
fs.watch('./logs/log-0.txt', () => {
  console.log(0)
})

```



### 6、Stream

```js
const fs = require('fs')

const readstream = fs.createReadStream('./note.txt')
const writestream = fs.createWriteStream('./note2.txt')

writestream.write(readstream)

```



### 7、Zlib

```js
const fs = require('fs')
const zlib = require('zlib')

const gzip = zlib.createGzip()

const readstream = fs.createReadStream('./note.txt')
const writestream = fs.createWriteStream('./note2.txt')

readstream
  .pipe(gzip)
  .pipe(writestream)

writestream.write(readstream)

```



### 8、ReadLine

```js
const readline = require('readline')

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
})

rl.question('What do you think of Node.js? ', (answer) => {
  // TODO: Log the answer in a database
  console.log(`Thank you for your valuable feedback: ${answer}`)

  rl.close()
})

```



### 9、Crypto

```js
const crypto = require('crypto')

const secret = 'abcdefg'
const hash = crypto.createHmac('sha256', secret)
                   .update('I love you')
                   .digest('hex')
console.log(hash)

```



## 五、路由

```js
var http = require('http')
var fs = require('fs')

http.createServer( function ( req, res ) {

  switch ( req.url ) {
    case '/home':
      res.write('home')
      res.end()
      break
    case '/mine':
      res.write('mine')
      res.end()
      break
    case '/login': 
      fs.readFile( './static/login.html',function ( error , data ) {
        if ( error ) throw error  
        res.write( data )
        res.end()
      })
      break
    case '/fulian.jpg':
      fs.readFile( './static/fulian.jpg', 'binary', function( error , data ) {
        if( error ) throw error 
        res.write( data, 'binary' )
        res.end()
      })
      break
    default: 
      break
   }

 }).listen( 8000, 'localhost', function () {
   console.log( '服务器运行在： http://localhost:8000' )
 })

```



## 六、静态资源服务

### 6.1 readStaticFile

`/modules/readStaticFile.js`

```js
// 引入依赖的模块
var path = require('path')
var fs = require('fs')
var mime = require('mime')

function readStaticFile(res, filePathname) {

  var ext = path.parse(filePathname).ext
  var mimeType = mime.getType(ext)

  // 判断路径是否有后缀, 有的话则说明客户端要请求的是一个文件 
  if (ext) {
    // 根据传入的目标文件路径来读取对应文件
    fs.readFile(filePathname, (err, data) => {
    // 错误处理
      if (err) {
        res.writeHead(404, { "Content-Type": "text/plain" })
        res.write("404 - NOT FOUND")
        res.end()
      } else {
        res.writeHead(200, { "Content-Type": mimeType })
        res.write(data)
        res.end()
      }
    });
    // 返回 true 表示, 客户端想要的 是 静态文件
    return true
  } else {
    // 返回 false 表示, 客户端想要的 不是 静态文件
    return false
  }
}

// 导出函数
module.exports = readStaticFile

```



### 6.2 server

`/server.js`

```js
// 引入相关模块
var http = require('http');
var url = require('url');
var path = require('path');
var readStaticFile = require('./modules/readStaticFile');

// 搭建 HTTP 服务器
var server = http.createServer(function(req, res) {
  var urlObj = url.parse(req.url);
  var urlPathname = urlObj.pathname;
  var filePathname = path.join(__dirname, "/public", urlPathname);

  // 读取静态文件
  readStaticFile(res, filePathname);
});

// 在 3000 端口监听请求
server.listen(3000, function() {
  console.log("服务器运行中.");
  console.log("正在监听 3000 端口:")
})

```



### 6.3 最终目录结构

![](https://lurongtao.gitee.io/felixbooks-gp19-node.js/images/dir.jpg)





# 02-Express

基于Node.js平台，快速、开放、极简的web开发框架。

```
$ npm install express --save

```



## 一、特色

### 1、Web应用

Express是一个基于Node.js平台的极简、灵活的web应用开发框架，它提供一系列强大的特性，帮助你创建各种Web和移动设备应用。



### 2、API

丰富的HTTP快捷方法和任意排列组合的Connect中间件，让你创建健壮、友好的API变得既快速又简单。



### 3、性能

Express不对Node.js已有的特性进行二次抽象，我们只是在它之上扩展了Web应用所需的基本性能。



## 二、安装

首先假定你已经安装了Node.js，接下来为你的应用创建一个目录，然后进入此目录并将其作为当前工作目录。

```
$ mkdir myapp
$ cd myapp

```

通过`npm init`命令为你的应用创建一个`package.json`文件。欲了解 package.json 是如何起作用的，请参考 Specifics of npm’s package.json handling。

```
$ npm init

```

此命令将要求你输入几个参数，例如此应用的名称和版本。你可以直接按“回车”键接受默认设置即可，下面这个除外：

```
entry point: (index.js)

```

键入app.js或者你所希望的名称，这是当前应用的入口文件。如果你希望采用默认的`index.js`文件名，只需按“回车”键即可。



接下来安装Express并将其保存到依赖列表中：

```
$ npm install express --save

```



如果只是临时安装Express，不想将它添加到依赖列表中，只需略去--save参数即可：

```
$ npm install express

```



> 安装Node模块时，如果指定了` --save` 参数，那么此模块将被添加到`package.json`文件中`dependencies`依赖列表中。然后通过`npm install` 命令即可



## 三、Hello World实例

接下来，我们一起创建一个基本的Express应用。

注意：这里所创建的是一个最最简单的Express应用，并且仅仅只有一个文件 - 和通过Express应用生成器所创建的应用完全不一样，Express应用生成器所创建的应用框架包含多JavaScript文件、Jade模板和针对不同用途的子目录。



进入myapp目录，创建一个名为app.js的文件，然后将下列代码复制进去：

```js
var express = require('express');
var app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});

var server = app.listen(3000, function () {
  var host = server.address().address;
  var port = server.address().port;

  console.log('Example app listening at http://%s:%s', host, port);
});

```

上面的代码启动一个服务并监听从3000端口进入的所有连接请求。他将对所有(/)URL或 路由返回“Hello World！”字符串。对于其他所有路径全部返回 404 Not Found。

> req (请求) 和 res (响应) 与 Node 提供的对象完全一致，因此，你可以调用 req.pipe()、req.on('data', callback) 以及任何 Node 提供的方法。



通过如下命令启动此应用：

```
$ node app.js

```

然后在浏览器中打开 <http://localhost:3000/> 并查看输出结果。



## 四、路由

路由是指如何定义应用的端点（URls）以及如何响应客户端的请求。



路由是由一个URI、HTTP请求（GET、POST等）和若干个句柄组成，它的结构如下：app.METHOD(path, [callback..], callback)， app 是 express 对象的一个实例，METHOD是一个 HTTP 请求方法，path 是服务器上的路径， callback 是当路由匹配时要执行的函数。



下面是一个基本的路由示例：

```js
var express = require('express');
var app = express();

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function(req, res) {
  res.send('hello world');
});

```



### 1、路由方法

路由方法源于 HTTP 请求方法，和 express 实例相关联。

下面这个例子展示了为应用跟路径定义的 GET 和 POST 请求：

```js
// GET method route
// 对网站首页的访问返回 "Hello World!" 字样
app.get('/', function (req, res) {
  res.send('Hello World!')
})

// 网站首页接受 POST 请求
app.post('/', function (req, res) {
  res.send('Got a POST request')
})

// /user 节点接受 PUT 请求
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})

// /user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})

```

Express 定义了如下和 HTTP 请求对应的路由方法： get, post, put, head, delete, options, trace, copy, lock, mkcol, move, purge, propfind, proppatch, unlock, report, mkactivity, checkout, merge, m-search, notify, subscribe, unsubscribe, patch, search, 和 connect。

> 有些路由方法名不是合规的 JavaScript 变量名，此时使用括号记法，比如：`app['m-search']('/', function ...)`



app.all()是一个特殊的路由方法，没有任何HTTP方法与其对应，它的作用是对于一个路径上的所有请求加载中间件。



在下面的例子中，来自"/secret"的请求，不管使用 GET、POST、PUT、DELETE或其他任何http模块支持的HTTP请求，句柄都会得到执行。

```js
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next(); // pass control to the next handler
})

```



### 2、路由路径

路由路径和请求方法一起定义了请求的端点，它可以是字符串、字符串模式或者正则表达式。



Express使用`path-to-regexp`匹配路由路径，请参考文档查阅所有定义路由路径的方法。Express Route Tester是测试基本Express路径的好工具，但不支持模式匹配。

> 查询字符串不是路由路径的一部分。

使用字符串的路由路径示例：

```js
// 匹配根路径的请求
app.get('/', function (req, res) {
  res.send('root');
});

// 匹配 /about 路径的请求
app.get('/about', function (req, res) {
  res.send('about');
});

// 匹配 /random.text 路径的请求
app.get('/random.text', function (req, res) {
  res.send('random.text');
});

```



使用字符串模式的路由路径示例：

```js
// 匹配 acd 和 abcd
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});

// 匹配 abcd、abbcd、abbbcd等
app.get('/ab+cd', function(req, res) {
  res.send('ab+cd');
});

// 匹配 abcd、abxcd、abRABDOMcd、ab123cd等
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});

// 匹配 /abe 和 /abcde
app.get('/ab(cd)?e', function(req, res) {
 res.send('ab(cd)?e');
});

```

> 字符 ?、+、* 和 () 是正则表达式的子集，- 和 . 在基于字符串的路径中按照字面值解释。



使用正则表达式的路由路径示例：

```js
// 匹配任何路径中含有 a 的路径：
app.get(/a/, function(req, res) {
  res.send('/a/');
});

// 匹配 butterfly、dragonfly，不匹配 butterflyman、dragonfly man等
app.get(/.*fly$/, function(req, res) {
  res.send('/.*fly$/');
});

```



### 3、路由句柄

可以为请求处理提供多个回调函数，其行为类似中间件。唯一的区别是这些回调函数有可能调用`next('route')`方法而略过其他路由回调函数。可以利用该机制为路由定义前提条件，如果在现有路径上继续执行没有意义，则可将控制权交给剩下的路径。



路由句柄有多种形式，可以是一个函数、一个函数数组、或者是两者混合，如下所示。



使用一个回调函数处理路由：

```js
app.get('/example/a', function(req, res) {
	res.send('Hello from A!')
})

```



使用多个回调函数处理路由（记得指定next对象）：

```js
app.get('/example/b', function (req, res, next) {
  console.log('response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});

```



使用回调函数数组处理路由：

```js
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

var cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])

```



混合使用函数和函数数组处理路由：

```js
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from D!')
})

```



### 4、响应方法

下表中响应对象(res)的方法向客户端返回响应，终结请求响应的循环。如果在路由句柄中一个方法也不调用，来自客户端的请求会一直挂起。

|       方法       |                            描述                            |
| :--------------: | :--------------------------------------------------------: |
|  res.download()  |                        提示下载文件                        |
|    res.end()     |                      终结响应处理流程                      |
|    res.json()    |                   发送一个JSON格式的响应                   |
|   res.jsonp()    |             发送一个支持JSONP的JSON格式的响应              |
|   res.direct()   |                         重定向请求                         |
|   res.render()   |                        渲染视图模板                        |
|    res.send()    |                     发送各种类型的响应                     |
|  res.sendFile()  |                 以八位字节流的形式发送文件                 |
| res.sendStatus() | 设置响应状态代码，并将其以字符串形式作为响应体的一部分发送 |



### 5、app.route()

可使用app.route()创建路由路径的链式路由句柄。由于路径在一个地方指定，这样做有助于创建模块化的路由，而且减少了代码冗余和拼写错误。



下面这个示例程序使用app.route()定义了链式路由句柄。

```js
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });

```



### 6、express.Router

可使用express.Router类创建模块化、可挂载的路由句柄。Router实例是一个完整的中间件和路由系统，因此常称其为一个'mini-app'。



下面的实例程序创建了一个路由模块，并加载 了一个中间件，定义了一些路由，并且将它们挂载至应用的路径上。



在app目录下创建名为bird.js的文件，内容如下：

```js
var express = require('express');
var router = express.Router();

// 该路由使用的中间件
router.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});
// 定义网站主页的路由
router.get('/', function(req, res) {
  res.send('Birds home page');
});
// 定义 about 页面的路由
router.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = router;

```



然后在应用中加载路由模块：

```js
var birds = require('./birds')
...
app.use('/birds', birds)

```

应用即可处理发自 /birds 和 /birds/about 的请求，并且调用为该路由指定的 timeLog 中间件。



## 五、利用Express托管静态文件

通过Express内置的`express.static`可以方便地托管静态文件，例如图片、CSS、JavaScript文件等。



将静态资源文件所在的目录作为参数传递给`express.static`中间件就可以提供静态资源文件的访问了。例如，假设在public目录放置了图片、CSS和JavaScript文件，你就可以：

```js
app.use(express.static('public'))

```

现在，public目录下面的文件就可以访问了。

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html

```

> 所有文件的路径都是相对于存放目录的，因此，存放静态文件的目录名不会出现在URL中。



如果你的静态资源存放在多个目录下面，你可以多次调用`express.static`中间件：

```js
app.use(express.static('public'))
app.use(express.static('files'))

```



访问静态资源文件时，`express.static`中间件会根据目录添加的顺序查找所需的文件。



如果你希望所有通过`express.static`访问的文件都存放在一个”虚拟(virtual)“目录（即目录根本不存在）下面，可以通过为静态资源目录指定一个挂载路径的方式来实现，如下所示：

```js
app.use('/static', express.static('public'))

```

现在，你就可以通过带有”/static“前缀的地址来访问public目录下面的文件了。

```js
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html

```



## 六、使用中间件

Express是一个自身功能极简，完全是由路由和中间件构成一个的web开发框架：从本质上来说，一个Express应用就是在调用各种中间件。



中间件（Middleware）是一个函数，它可以访问请求对象（request object(req)），响应对象(response object(res))，和web应用中处于请求-响应循环流程中的中间件，一般被命名为next的变量。



中间件的功能包括：

- 执行任何代码
- 修改请求和响应对象
- 终结请求-响应循环
- 调用堆栈中的下一个中间件



如果当前中间件没有终结请求-响应循环，则必须调用`next()`方法将控制权交给下一个中间件，否则请求就会挂起。



Express应用可使用如下几种中间件：

- 应用级中间件
- 路由级中间件
- 错误处理中间件
- 内置中间件
- 第三方中间件



使用可选则挂载路径，可在应用级别或路由级别装载中间件。另外，你还可以同时装在一系列中间件函数，从而在一个挂载点上创建一个子中间件栈。



### 1、应用级中间件

应用级中间件绑定到app对象，使用`app.use()`和`app.METHOD()`，其中，`METHOD`是需要处理的HTTP请求的方法，例如GET,PUT,POST等等，全部小写。例如：

```js
var app = express()

// 没有挂载路径的中间件，应用的每个请求都会执行该中间件
app.use(function (req, res, next) {
	console.log('Time:', Date.now())
    next()
})

// 挂载至 /user/:id 的中间件，任何指向 /user/:id 的请求都会执行它
app.use('/user/:id', function(req, res, next) {
	console.log('Request Type:', req.method)
    next()
})

// 路由和句柄函数(中间件系统)，处理指向 /user/:id 的 GET 请求
app.get('/user/:id', function(req, res, next) {
	res.send('USER')
})

```



下面这个例子展示了在一个挂载点装载一组中间件。

```js
// 一个中间件栈，对任何指向 /user/:id 的 HTTP 请求打印出相关信息
app.use('/user/:id', function(req, res, next) {
	console.log('Request URL:', req.originalUrl)
    next()
}, function(req, res, next) {
	console.log('Request Type:', req.method)
    next()
})

```

作为中间件系统的路由句柄，使得为路径定义多个路由成为可能。在下面的例子中，为指向 /user/:id 的 GET 请求定义了两个路由。 第二个路由虽然不会带来任何问题，但却永远不会被调用，因为第一个路由已经终止了请求-响应循环。

```js
// 一个中间件栈，处理指向 /user/:id 的 GET请求
app.get('/user/:id', function(req, res, next) {
	console.log('ID:', req.params.id)
    next()
}, function(req, res, next) {
	res.send('User Info')
})

// 处理 /user/:id, 打印出用户id
app.get('/user/:id', function(req, res, next) {
	res.end(req.params.id)
})

```



如果需要在中间件栈中跳过剩余中间件，调用 `next('route')`方法将控制权交给下一个路由。注意：`next('route')`只对使用 `app.VERB()`  或 `router.VERB()`加载的中间件有效。

```js
// 一个中间件栈，处理指向 /user/:id 的 GET 请求
app.get('/user/:id', function(req, res, next) {
	// 如果 user id 为0，跳到下一个路由
    if (req.params.id === 0) next('route')
    // 否则将控制权交给栈中下一个中间件
    else next()
}, function(req, res, next) {
	// 渲染常规页面
    res.render('regular')
})

// 处理 /user/:id, 渲染一个特殊页面
app.get('/user/:id', function(req, res, next) {
	res.render('special')
})

```



### 2、路由级中间件

路由级中间件和应用级中间件一样，只是它绑定的对象为 `express.Router()`。

```
var router = express.Router()

```

路由级使用`router.use()`或`router.VERB()`加载。

 上述在应用级创建的中间件系统，可通过如下代码改写为路由级。

```js
var app = express()
var router = express.Router()

// 没有挂载路径的中间件，通过该路由的每个请求都会执行该中间件
router.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})

// 一个中间件栈，显示任何指向 /user/:id 的 HTTP 请求的信息
router.use('/user/:id', function(req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})

// 一个中间件栈，处理指向 /user/:id 的 GET 请求
router.get('/user/:id', function (req, res, next) {
  // 如果 user id 为 0, 跳到下一个路由
  if (req.params.id == 0) next('route')
  // 负责将控制权交给栈中下一个中间件
  else next() //
}, function (req, res, next) {
  // 渲染常规页面
  res.render('regular')
})

// 处理 /user/:id， 渲染一个特殊页面
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id)
  res.render('special')
})

// 将路由挂载至应用
app.use('/', router)

```



### 3、错误处理中间件

> 错误处理中间件有4个参数，定义错误处理中间件时必须使用这4个参数。即使不需要next对象，也必须在签名中声明它，否则中间件会被识别为一个常规中间件，不能处理错误。

错误处理中间件和其他中间件定义类似，只是要使用4个参数，而不是3个，其签名如下：(err,req,res,next)。

```js
app.use(function(err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})

```



### 4、内置中间件

从4.x版本开始，Express已经不再依赖Connect了。除了express.static，Express以前内置的中间件现在已经全部单独作为模块安装使用了。请参考 中间件列表。



`express.static(root, [options])`

express.static 是 Express 唯一内置的中间件。它基于 serve-static，负责在Express应用中提托管静态资源。

参数 root 指提供静态资源的根目录。

可选的 options 参数拥有如下属性。

| 属性         | 描述                                                         | 类型     | 缺省值       |
| ------------ | ------------------------------------------------------------ | -------- | ------------ |
| dotfiles     | 是否对外输出文件名以点（.）开头的文件。可选值为”allow“，”deny“和”ignore“。 | String   | ”ignore“     |
| etag         | 是否启用etag生成                                             | Boolean  | true         |
| extensions   | 设置文件扩展名备份选项                                       | Array    | []           |
| index        | 发送目录索引文件，设置为false禁用目录索引                    | Mixed    | "index.html" |
| lastModified | 设置Last-Modified头为文件在操作系统上的最后修改日期。可能值为true或false | Boolean  | true         |
| maxAge       | 以毫秒或者其字符串格式设置Cache-Control头的max-age属性。     | Number   | 0            |
| redirect     | 当路径为目录时，重定向至”/“                                  | Boolean  | true         |
| setHeaders   | 设置HTTP头以提供文件的函数                                   | Function |              |

下面的例子使用了 express.static 中间件，其中的 options 对象经过了精心的设计。

```js
var options = {
  dotfiles: 'ignore',
  etag: false,
  extensions: ['htm', 'html'],
  index: false,
  maxAge: '1d',
  redirect: false,
  setHeaders: function (res, path, stat) {
    res.set('x-timestamp', Date.now())
  }
}

app.use(express.static('public', options))

```

每个应用可有多个静态目录。

```js
app.use(express.static('public'))
app.use(express.static('uploads'))
app.use(express.static('files'))

```



### 5、第三方中间件

通过使用第三方中间件从而为 Express 应用增加更多功能。

安装所需功能的 node 模块，并在应用中加载，可以在应用级加载，也可以在路由级加载。

下面的例子安装并加载了一个解析 cookie 的中间件： cookie-parser

```
$ npm install cookie-parser

```

```js
var express = require('express')
var app = express()
var cookieParser = require('cookie-parser')

// 加载用于解析 cookie 的中间件
app.use(cookieParser())

```



## 七、在Express中使用模板引擎

需要在应用中进行如下设置才能让Express渲染模板文件：

- `views`，放模板文件的目录，比如：`app.set('views', './views')`
- `view engine`，模板引擎，比如：`app.set('view engine', 'ejs')`



**art-template**

art-template for express 4.x.

- 1、Install

  ```
  npm install --save art-template
  npm install --save express-art-template
  
  ```

  

- 2、Example

  ```js
  var express = require('express')
  var app = express()
  
  // view engine setup
  app.engine('art', require('express-art-template'))
  app.set('view', {
      debug: process.env.NODE_ENV !== 'production'
  })
  app.set('views', path.join(__dirname, 'views'))
  app.set('view engine', 'art')
  
  // routes
  app.get('/', function (req, res) {
      res.render('index.art', {
          user: {
              name: 'aui',
              tags: ['art', 'template', 'nodejs']
          }
      })
  })
  
  ```

  

# 03-Koa2

## 一、koa2快速开始

因为node.js v7.6.0开始完全支持async/await，不需要加flag，所以node.js环境都要7.6.0以上node.js环境 版本v7.6以上 npm 版本3.x以上
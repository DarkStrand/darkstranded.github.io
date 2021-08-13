---
title: canvas
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: darkstranded.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-08-13 16：01：30
comments: true
tags: 
 - web
keywords: canvas
description: canvas
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/3.jpeg

---




## 一、canvas简介

<kbd>canvas</kbd>是HTML5新增的，一个可以使用脚本（通常为JavaScript）在其中绘制图像的HTML元素。它可以用来制作照片或者制作简单（也不是那么简单）的动画，甚至可以进行实时视频处理和渲染。

它最初由苹果内部使用自己MacOS X Webkit推出，供应用程序使用像仪表盘的构件和Safari浏览器使用。后来，有人通过Gecko内核的浏览器（尤其是Mozilla和Firefox），Opera和Chrome和超文本网络应用技术工作组建议为下一代的网络技术使用该元素。

Canvas是由HTML代码配合高度和宽度属性而定义出的可绘制区域。JavaScript代码可以访问该区域，类似于其他通用的二维API，通过一套完整的绘图函数来动态生成图形。

Mozilla程序从Geko1.8（Firefox1.5）开始支持<kbd>canvas</kbd>，Internet Explorer从IE9开始<kbd>canvas</kbd>。Chrome和Opera9+也支持<kbd>canvas</kbd>。



## 二、Canvas基本使用

### 2.1canvas元素

<kbd>canvas</kbd>看起来和<kbd>`<img>`</kbd>标签一样，只是<kbd>canvas</kbd>只有两个可选的属性width、height属性，而没有src、alt属性。

如果不给<kbd>canvas</kbd>设置width、height属性时，则默认width为300、height为150，单位都是px。也可以使用css属性来设置宽高，但是如宽高属性和初始比例不一致，他会出现扭曲。所以，建议永远不要使用css属性来设置<kbd>canvas</kbd>的宽高。

***替换内容***

由于某些较老的浏览器（尤其是IE9之前的IE浏览器）或者浏览器不支持HTML元素<kbd>canvas</kbd>，在这些浏览器上你应该总是能展示替代内容。

支持<kbd>canvas</kbd>的浏览器会只渲染<kbd>canvas</kbd>标签，而忽略其中的替代内容。不支持<kbd></kbd>canvas</kbd>的浏览器会直接渲染替代内容。

用文本替换：

```html
<canvas>
    你的浏览器不支持canvas，请升级你的浏览器。
</canvas>
```

用<kbd>`<img>`</kbd>替换：

```html
<canvas>
	<img src="./美女.jgp" alt="">
</canvas>
```

***结束标签</canvas>不可省略***

与<kbd>`<img>`</kbd>元素不同，<kbd>canvas</kbd>元素需要结束标签（</canvas>）。如果结束标签不存在，则文档的其余部分会被认为是替代内容，将不会显示出来。

##2.2渲染上下文（Thre Rending Context）

<kbd>canvas</kbd>会创建一个固定大小的画布，会公开一个或多个**渲染上下文**(画笔)，使用**渲染上下文**来绘制和处理要展示的内容。

我们重点研究2D渲染上下文。其他的上下文我们暂不研究，比如，WebGL使用了基于OpenGLES的3D上下文。

```javascript
var canvas = document.getElementById('tutorial');
//获得2d上下文对象
var ctx = canvas.getContext('2d')
```



### 2.3检测支持性

```javascript
var canvas = document.getElementById('tutorial');
if(canvas.getContext) {
	var ctx = canvas.getContext('2d')
    // drawing code here
} else {
	// canvas-unsupported code here
}
```



### 2.4代码模板

```html
<canvas id="tutorial" width="300" height="300"></canvas>
<script type="text/javascript">
	function draw() {
		var canvas = document.getElementById('tutorial');
        if(!canvas.getContext) return
        var ctx = canvas.getContext('2d')
        //开始写代码
    }
</script>
```



### 2.5一个简单的例子

以下实例绘制两个长方形

```html
<canvas id="tutorial" width="300" height="300"></canvas>
<script type="text/javascript">
	function draw() {
		var canvas = document.getElementById('tutorial');
        if(!canvas.getContext) return
        var ctx = canvas.getContext('2d')
        ctx.fillStyle = "rgb(200,0,0)"
        //绘制矩形
        ctx.fillRect (10,10,55,50)
        ctx.fillStyle = "rgb(0,0,200,0.5)"
        ctx.fillRect (30,30,55,50)
    }
    draw()
</script>
```



## 三、绘制形状

### 3.1栅格(grid)和坐标空间

如下图所示，canvas元素默认被网格所覆盖。通常来说网格中的一个单元 相当于canvas元素中的一像素。栅格的起点为左上角，坐标为(0,0)。所有元素的位置都相对于原点来定位。所以图中蓝色方形左上角的坐标为距离左边（X轴）x像素，距离上边（Y轴）y像素，坐标为(x,y)。

后面我们会涉及到坐标原点的平移、网格的旋转以及缩放等。

![img](https://www.runoob.com/wp-content/uploads/2018/12/Canvas_default_grid.png)

### 3.2绘制矩形

<kbd>canvas</kbd>只支持一种原生的图形绘制：***矩形***。所有其他图形都至少需要生成一种路径(path)。不过，我们拥有众多路径生成的方法让复杂图形的绘制成为了可能。

canvas提供了三种方法绘制矩形：

* fillRect(x, y, width, height)：绘制一个填充的矩形。
* strokeRect(x, y, width, height)：绘制一个矩形的边框。
* clearRect(x, y, width, height)：清除指定的矩形区域，然后这块区域会变的完全透明。

> 说明：这3个方法具有相同的参数。
>
> * x,y：指的是矩形的左上角的坐标。(相对于canvas的坐标原点)
> * width,height：指的是绘制的矩形的宽和高。

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.fillRect(10,10,100,50) //绘制矩形，填充的默认颜色为黑色
    ctx.strokeRect(10, 70, 100 ,50) //绘制矩形边框
}
draw()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/2400420933-5b74dd8f80306_articlex.png)

```javascript
ctx.clearRect(15, 15, 50, 25)
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/2347163070-5b74dd8f813a6_articlex.png)

## 四、绘制路径(path)

图形的基本元素是路径。

路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。

一个路径，甚至一个子路径，都是闭合的。

***使用路径绘制图形需要一些额外的步骤：***

> 1. 创建路径起始点
> 2. 调用绘制方法去绘制出路径
> 3. 把路径封闭
> 4. 一旦路径生成，通过描边或填充路径区域来渲染图形

***下面是需要用到的方法：***

> 1. beginPath()
>
> ​       新建一条路径，路径一旦创建成功，图形绘制命令被指向到路径上生成路径
>
> 2. moveTo(x, y)
>
>    把画笔移动到指定的坐标(x, y)。相当于设置路径的起始点坐标。
>
> 3. closePath()
>
>    闭合路径之后，图形绘制命令又重新指向到上下文中
>
> 4. stroke()
>
>    通过线条来绘制图形轮廓
>
> 5. fill()
>
>    通过填充路径的内容区域生成实心的图形



## 4.1绘制线段

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath() //新建一条path
    ctx.moveTo(50,50) // 把画笔移动到指定的坐标
    ctx.lineTo(200,50) //绘制一条从当前位置到指定坐标(200,50)的直线
    ctx.closePath() //闭合路径。会拉一条从当前点到path起始点的直线。如果当前点与起始点重合，则什么都不做
    ctx.stroke() //绘制路径
}
draw()
```

## 4.2绘制三角形边框

```javascript
function draw() {
	var canvas = document.getElmentById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath()
    ctx.moveTo(50,50)
    ctx.lineTo(200,50)
    ctx.lineTo(200,200)
    ctx.closePath() //虽然我们只绘制了两条线段，但是closePath会closePath，仍然是一个三角形
    ctx.stroke() //描边。stroke不会自动closePath()
}
draw()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/2106846415-5b74dd8f67000_articlex.png)



## 4.3填充三角形

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath()
    ctx.moveTo(50,50)
    ctx.lineTo(200,50)
    ctx.lineTo(200,200)
    ctx.fill() //填充闭合区域。如果path没有闭合，则fill()会自动闭合路径
}
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/3457015746-5b74dd8f72860_articlex.png)



## 4.4绘制圆弧

有两个方法可以绘制圆弧：

1. arc(x, y, r, startAngle, endAngle, anticlockwise): 

   以(x,y) 为圆心，以r为半径，从startAngle弧度开始到endAngle弧度结束。anticlosewise是布尔值，true表示逆时针，false表示顺时针(默认是顺时针)。

   >1. 这里的度数都是弧度。
   >
   >2. 0弧度是指的x轴正方向。
   >
   >   >radians = (Math.PI/180)*degrees //角度转换成弧度

2. arcTo(x1,y1,x2,y2,radius):

   根据给定的控制点和半径画一段圆弧，最后再以直线连接两个控制点。

```javascript
//圆弧案例1
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath()
    ctx.arc(50,50,40,0,Math.PI /2, false)
    ctx.stroke()
}
draw()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/3832141455-5b74dd8f658df_articlex.png)

```javascript
//圆弧案例2
funcion draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath()
    ctx.arc(50,50,50,0,Math.PI/2,false)
    ctx.stroke()
    
    ctx.beginPath()
    ctx.arc(150,50,40,0,-Math.PI/2,true)
    ctx.closePath()
    ctx.stroke()
    
    ctx.beginPath()
    ctx.arc(50,150,40,-Math.PI/2,Math.PI/2,false)
    ctx.fill()
    
    ctx.beginPath()
    ctx.arc(150,150,40,0,Math.PI,false)
    ctx.fill()
}
draw()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/2218794221-5b74dd8f43f98_articlex.png)

```javascript
//圆弧案例3
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath()
    ctx.moveTo(50,50)
    //参数1、2：控制点1坐标
    //参数3、4：控制点2坐标
    //参数5：圆弧半径
    ctx.arcTo(200,50,200,200,100)
    ctx.lineTo(200,200)
    ctx.stroke()
    
    ctx.beginPath()
    ctx.rect(50,50,10,10)
    ctx.rect(200,50,10,10)
    ctx.rect(200,200,10,10)
    ctx.fill()
}
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/3556678928-5b74dd8f1bd2a_articlex.png)

> arcTo方法的说明：
>
> 这个方法可以这样理解。绘制的弧形是由两条切线所决定。
>
> 第1条切线：起始点和控制点1决定的直线。
>
> 第2条切线：控制点1和控制点2决定的直线。
>
> 其实绘制的圆弧就是与这两条直线相切的圆弧。



## 4.5绘制贝塞尔曲线

### 4.5.1什么是贝塞尔曲线

贝塞尔曲线(Bézier curve)，又称贝兹曲线或贝济埃曲线，是应用于二维图形应用程序的数学曲线。

一般的矢量图形软件通过它来精确画出曲线，贝兹曲线是由线段与节点组成，节点是可拖动的支点，线段像可伸缩的皮筋，我们在绘图工具上看到的钢笔工具就是来做这种矢量曲线的。

贝塞尔曲线是计算机图形学中相当重要的参数曲线，在一些比较成熟的位图软件中也有贝塞尔曲线工具入PhotoShop等。在Flash4中还没有完整的曲线工具，而在Flash5里面已经提供出贝塞尔曲线工具。

贝塞尔曲线于1962，由法国工程师皮埃尔·贝塞尔（Pierre Bézier）所广泛发表，他运用贝塞尔曲线来为汽车的主体进行设计。贝塞尔曲线最初由Paul de Casteljau 于 1959 年运用 de Casteljau 演算法开发，以稳定数值的方法求出贝兹曲线。

***一次贝塞尔曲线其实是一条直线***

![img](https://www.runoob.com/wp-content/uploads/2018/12/240px-b_1_big.gif)

***二次贝塞尔曲线***

![img](https://www.runoob.com/wp-content/uploads/2018/12/b_2_big.gif)

![img](https://www.runoob.com/wp-content/uploads/2018/12/1544764428-5713-240px-BC3A9zier-2-big.svg-.png)

三次贝塞尔曲线

![img](https://www.runoob.com/wp-content/uploads/2018/12/b_3_big.gif)

![img](https://www.runoob.com/wp-content/uploads/2018/12/1544764428-2467-240px-BC3A9zier-3-big.svg-.png)



### 4.5.2绘制贝塞尔曲线

1. 绘制二次贝塞尔曲线：/kwɒ'drætɪk/

```javascript
quadraticCurveTo(cp1x,cp1y,x,y)
```

> 说明：
>
> * 参数1和2：控制点坐标
> * 参数3和4：结束点坐标

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.beginPath()
    ctx.moveTo(10,200) //起始点
    var cp1x = 40, cp1y = 100 //控制点
    var x = 200, y = 200 //结束点
    //绘制二次贝塞尔曲线
    ctx.quadraticCurveTo(cp1x, cp1y, x, y)
    ctx.stroke()
    
    ctx.beginPath()
    ctx.rect(10,200,10,10)
    ctx.rect(cp1x,cp1y,10,10)
    ctx.rect(x,y,10,10)
    ctx.fill()
}
draw()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/274915666-5b74dd8ecb2e2_articlex.png)

2. 绘制三次贝塞尔曲线:

   ```javascript
   bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
   ```

   > 说明：
   >
   > * 参数1和2：控制点1的坐标
   > * 参数3和4：控制点2的坐标
   > * 参数5和6：结束点的坐标

   

   ```javascript
   function draw() {
   	var canvas = document.getElementById('tutorial')
       if(!canvas.getContext) return
       var ctx = canvas.getContext('2d')
       ctx.beginPath()
       ctx.moveTo(40,200) //起始点
       var cp1x = 20, cp1y = 100 //控制点1
       var cp2x = 100, cp2y = 120 //控制点2
       var x = 200, y = 200 //结束点
       //绘制三次贝塞尔曲线
       ctx.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)
       ctx.stroke()
       
       ctx.beginPath();
       ctx.rect(40, 200, 10, 10);
       ctx.rect(cp1x, cp1y, 10, 10);
       ctx.rect(cp2x, cp2y, 10, 10);
       ctx.rect(x, y, 10, 10);
       ctx.fill();
   }
   draw()
   ```

   

   ![img](https://www.runoob.com/wp-content/uploads/2018/12/3947786617-5b74dd8ec8678_articlex.png)



# 五、添加样式和颜色

在前面的绘制矩形章节中，只用到了默认的线条和颜色。

如果想要给图形上色，有两个重要的属性可以做到。

1. fillStyle = color 设置图形的填充颜色
2. strokeStyle = color 设置图形轮廓的颜色

> 备注：
>
> 1. color可以是表达css颜色值的字符串、渐变对象或者图案对象
> 2. 默认情况下，线条和填充颜色都是黑色
> 3. 一旦您设置了strokeStyle或者fillStyle的值，那么这个新值就会成为新绘制的图形的默认值。如果你要给每个图形上不同的颜色，你需要重新设置fillStyle或strokeStyle的值

##     fillStyle



```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    for(var i = 0; i < 6; i++) {
		for(var j = 0; j < 6; j++) {
			ctx.fillStyle = 'rgb(' + Math.floor(255 - 42.5 * i) + ',' + Math.floor(255 - 42.5 * j) + ',0)'
            ctx.fillRect(j * 50, i * 50, 50, 50)
        }
    }
}
draw()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/2505008676-5b74dd8ebad41_articlex.png)



## strokeStyle



```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    for(var i = 0; i < 6; i++) {
		for(var j = 0;j < 6;j++) {
			ctx.strokeStyle = `rgb(${randomInt(0,255)}, ${randomInt(0,255)}, ${randomInt(0,255)})`
            ctx.strokeRect(j*50,i*50,40,40)
        }
    }
}
draw()

//返回随机的[from,to]之间的整数(包括from，也包括to)
function randomInt(from,to) {
	return parseInt(Math.random() * (to-from +1) + from)
}
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/3288535670-5b74dd8ea12d9_articlex.png)

## Transparency(透明度)



globalAlpha = transparencyValue:这个属性影响到canvas里所有图形的透明度，有效的值范围是0.0（完全透明）到1.0（完全不透明），默认是1.0

**globalAlpha**属性在需要绘制大量拥有相同透明度的图形时候相当高效。不过，我认为使用rgba()设置透明度更好一些。

**1. line style**

线宽。只能是正值。默认是1.0

起始点和终点的连线为中心，上下各占线宽的一半。

```javascript
ctx.beginPath()
ctx.moveTo(10,10)
ctx.lineTo(100,10)
ctx.lineWidth = 10
ctx.stroke()

ctx.beginPath()
ctx.moveTo(110,10)
ctx.lineTo(160,10)
ctx.lineWidth = 20
ctx.stroke()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/3410060825-5b74dd8ea12d9_articlex.png)

**2. lineCap = type**

线条末端样式。

> 共有三个值：
>
> 1. butt： 线段末端以方形结束
> 2. round：线段末端以圆形结束
> 3. square：线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一般的矩形区域

```javascript
var lineCaps = ['butt', 'round', 'square']
for(var i = 0;i<3;i++) {
	ctx.beginPath()
    ctx.moveTo(20+30*i,30)
    ctx.lineTo(20+30*i,100)
    ctx.lineWidth = 20
    ctx.lineCap = lineCaps[i]
    ctx.stroke()
}

ctx.beginPath()
ctx.moveTo(0,30)
ctx.lineTo(300,30)

ctx.moveTo(0,100)
ctx.lineTo(300,100)

ctx.strokeStyle = 'red'
ctx.lineWidth = 1
ctx.stroke()
```

![img](https://www.runoob.com/wp-content/uploads/2018/12/3380216230-5b74dd8e97e85_articlex.png)

**3. lineJoin = type**

同一个path内，设定线条与线条间接合处的样式。

共有三个值round,bevel和miter：

1. round通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状。圆角的半径是线段的宽度。
2. bevel在相连部分的末端填充一个额外的以三角形为底的区域，每个部分都有各自独立的矩形拐角。
3. miter(默认)通过延伸相连部分的外边缘，使其相交于一点，形成一个额外的菱形区域。

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    
    var lineJoin = ['round','bevel', 'miter']
    ctx.lineWidth = 20
    for(var i =0; i< lineJoin.length;i++) {
		ctx.lineJoin = lineJoin[i]
        ctx.beginPath()
        ctx.moveTo(50,50+i*50)
        ctx.lineTo(100,100+i*50)
        ctx.lineTo(150,50+i*50)
        ctx.lineTo(200,100+i*50)
        ctx.lineTo(250,50+i*50)
        ctx.stroke()
    }
}
draw()
```

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/1584506777-5b74dd8e82768_articlex.png)



**4. 虚线**

用setLineDash方法和lineDashOffset属性来指定虚线样式。

setLineDash方法接收一个数组，来指定线段与间隙的交替；

lineDashOffset属性设置起始偏移量。

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.setLineDash([20,5]) //[实线长度，间隙长度]
    ctx.lineDashOffset = -0
    ctx.strokeRect(50,50,210,210)
}
draw()
```

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/2805401035-5b74dd8e6833c_articlex.png)

> 备注：getLineDash()返回一个包含当前虚线样式，长度为非偶数的数组。



# 六、绘制文本

***绘制文本的两个方法***

canvas提供了两种方法来渲染文本：

1. fillText(text, x, y, [, maxWidth])在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的。

2. strokeText(text, x, y, [, maxWidth])在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的。

   ```javascript
   var ctx;
   function draw() {
   	var canvas = document.getELementById('tutorial')
       if(!canvas.getContext) return
       ctx = canvas.getContext('2d')
       ctx.font = '100px sans-serif'
       ctx.fillText('天若有情',10,100)
       ctx.strokeText('天若有情',10,200)
   }
   draw()
   ```

   1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/404304980-5b74dd8e7499e_articlex.png)



***给文本添加样式***

1. font = value 当前我们用来绘制文本的样式。这个字符串使用和css font属性相同的语法。**默认的字体是10px sans-serif。**
2. textAlign = value 文本对齐选项。可选的值包括：start，end，left，right，center。**默认是start。**
3. textBaseline = value 基线对齐选项，可选的值包括：top，hanging，middle，alphabetic，ideographic，bottom。**默认值是alphabetic。**
4. direction = value 文本方向。可能的值包括：ltr，rtl，inherit。**默认值是inherit。**



# 七、绘制图片

我们也可以在canvas上直接绘制图片。

## 7.1由零开始创建图片

```javascript
var img = new Image() //创建一个<img>元素
img.src = 'myImage.png' //设置图片原地址
```

**绘制img**

```javascript
//参数1：要绘制的img
//参数2、3：绘制的img在canvas中的坐标
ctx.drawImage(img,0,0)
```

> 注意：考虑到图片是从网络加载，如果drawImage的时候图片还没有完全加载完成，则什么都不做，个别浏览器会抛异常。所以我们应该保证在img绘制完成之后再drawImage。

```javascript
var img = new Image() //创建img元素
img.onload = function() {
	ctx.drawImage(img, 0, 0)
}
img.src = 'myImage.png' //设置图片c地址
```



## 7.2 绘制img标签元素中的图片

img可以new，也可以来源于我们页面中的<img>标签。

```html
<img src="./美女.jpg" alt="" width="300"><br>
<canvas id="tutorial" width="600" height="400"></canvas>
function draw() {
	var canvas = document.getElementById('tutorial)
	if(!canvas.getContext) return
	var ctx = canvas.getContext('2d')
	var img = document.querySelector('img')
	ctx.drawImg(img,0,0)
}
document.querySelector('img').onclick = function() {
	draw()
}
```

1. 1. 第一张图片就是页面中的 `<img>` 标签：

2. ![img](https://www.runoob.com/wp-content/uploads/2018/12/2255709523-5b74dd8eb033e_articlex.png)



## 7.3 缩放图片

drawImage()可以再添加两个参数：

```javascript
drawImage(image,x,y,width,height)
```

这个方法多了两个参数：width和height，这两个参数用来控制 当像canvas画入时应该缩放的大小。

```javascript
ctx.drawImage(img,0,0,400,200)
```



##7.4 切片(slice)

```javascript
drawImage(image,sx,sy,sWidth,sHeight,dx,dy,dWidth,dHeight)
```

第一个参数和其他的是相同的，都是一个图像或者另一个canvas的引用。

其他8个参数：前4个事定义图像源的切片位置和大小，后四个则是定义切片的目标显示位置和大小。

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/2106688680-54566fa3d81dc_articlex.jpeg)



# 八、状态的保存和恢复

Saving and restoring state是绘制复杂图形时必不可少的操作。

save()和restore()

save和restore方法是用来保存和恢复canvas状态的，都没有参数。

Canvas的状态就是当前画面应用的所有样式和变形的一个快照。

## 8.1 关于save()

> Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。

一个绘画状态包括:

* 当前应用的变形（即移动，旋转和缩放）

* strokeStyle,fillStyle,globalAlpha,lineWidth,lineCap,lineJoin,miterLimit,shadowOffsetX,shadowOffsetY,shadowBlur,shadowColor,globalCompositeOperation的值

* 当前的裁切路径（clipping path)

  

  ***可以调用任意多次save方法（类似数组的push()）***

## 8.2关于resotre()

> 每一次调用restore方法，上一个保存的状态就从栈中弹出，所有设定都恢复（类似数组pop()）

```javascript
var ctx;
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    
    ctx.fillRect(0,0,150,150) //使用默认设置绘制一个矩形
    ctx.save() //保存默认状态
    
    ctx.fillStyle = 'red' //在原有配置基础上对颜色做改变
    ctx.fillRect(15,15,120,120) //使用心得设置绘制一个矩形
    
    ctx.save() //保存当前状态
	ctx.fillStyle = '#FFF' //再次改变颜色状态
    ctx.fillRect(30,30,90,90) //使用心得配置绘制一个矩形
    
    ctx.restore() //重新加载之前的颜色状态
    ctx.fillRect(45,45,60,60) //使用上一次的配置绘制一个矩形
    
    ctx.restore() //加载默认颜色配置
    ctx.fillRect(60,60,30,30) //使用加载的配置绘制一个矩形
}
draw()
```



# 九、变形

## 9.1translate

translate(x,y)

用来移动canvas的原点到指定的位置

translate方法接收两个参数。x是左右偏移量，y是上下偏移量，如图所示。

在做变形之前先保存状态是一个良好的习惯。大多数情况下，调用restore方法比手动恢复原形的状态要简单得多。又如果你是在一个循环中做位移但没有保存和恢复canvas的状态，很可能到最后会发现怎么有些东西不见了，那是因为它很可能已经超出canvas范围以外了。注意：translate移动的是canvas的坐标原点(坐标变换)。

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/829832336-5b74dd8e3ad9a_articlex.png)

```javascript
var ctx;
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.save() //保存坐标原点平移之前的状态
    ctx.translate(100,100)
    ctx.strokeRect(0,0,100,100)
    ctx.restore()
    ctx.translate(220,200)
    ctx.fillRect(0,0,100,100)
}
draw()
```

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/1230266743-5b74dd8e3b0ce_articlex.png)



## 9.2 rotate

rotate(angle)

旋转坐标轴。

这个方法只接受一个参数：旋转的角度（angle），它是顺时针方向的，以弧度为单位的值。旋转的中心是坐标原点。

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/3322150878-5b74dd8e2b6a4_articlex.png)

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvs.getContext) return
    var ctx = canvas.getContext('2d')
    
    ctx.fillStyle = 'red'
    ctx.save()
    
    ctx.translate(100,100)
    ctx.rotate(Math.PI/180*45)
    ctx.fillStyle = 'blue'
    ctx.fillRect(0,0,100,100)
    ctx.restore()
    
    ctx.save()
    ctx.translate(0,0)
    ctx.fillRect(0,0,50,50)
    ctx.restore()
}
draw()
```

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/1819968878-5b74dd8e1e770_articlex.png)



## 9.3 scale

scale(x,y)

我们用它来增减图形在canvas中的像素数目，对形状、位图进行缩小或者放大。

scale方法接受两个参数。x,y分别是横轴和纵轴的缩放因子，它们都必须是正值。值比1.0小表示缩小，比1.0大表示放大，值为1.0时什么效果都没有。

默认情况下，canvas的1单位就是1个像素。举例说，如果我们设置缩放因子是0.5，1个单位就变成对应0.5个像素，这样绘制出来的形状就会是原先的一半。同理，设置为2.0时，1个单位就对应变成了2像素，绘制的结果就是图形放大了2倍。



## 9.4 transform(变形矩阵)

transform(a,b,c,d,e,f)

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/2958376259-5b74dd8e15192_articlex.png)

* a (m11): Horizontal scaling.
* b (m12): Horizontal skewing.
* c (m21): Vertical skewing.
* d (m22): Vertical scaling.
* e (dx): Horizontal moving.
* f (dy): Vertical moving.

```javascript
function draw() {
	var canvas = document.getElementById('tutorial')
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.transform(1,1,0,1,0,0)
    ctx.fillRect(0,0,100,100)
}
draw()
```

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/489430190-5b74dd8e17ad2_articlex.png)



## 十、合成

在前面的所有例子中，我们总是将一个图形画在另一个之上，对于其他更多的情况，仅仅这样是远远不够的。比如，对合成的图形来说，绘制顺序会有限制。不过，我们可以利用globalCompositeOperation属性来改变这种情况。

```javascript
globalCompositeOperation = type
```

```javascript
var ctx;
function draw() {
	var canvas = document.getElementById('tutorial');
    if(!canvas.getContext) return
    var ctx = canvas.getContext('2d')
    ctx.fillStyle = 'blue'
    ctx.fillRect(0,0,200,200)
    
    ctx.globalCompositeOperation = 'source-over' //全局合成操作
    ctx.fillStyle = 'red'
    ctx.fillRect(100,100,200,200)
}
```

> 注：下面的展示中，蓝色是原有的，红色是新的。
>
> type是下面13中字符串值之一:

1. 这是默认设置，新图像会覆盖在原有图像。

1. ![img](https://www.runoob.com/wp-content/uploads/2018/12/1858023544-5b74dd8e0813d.png)


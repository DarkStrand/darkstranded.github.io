---


title: middle-front
date: 2021-09-18
categories:
  - front
tags:
  - 前端
sidebar: auto
---

# 1、HTML篇

## 1.1 HTML5语义化
[HTML5语义化](https://rainylog.com/post/ife-note-1/)

### 1.1.1 为什么需要语义化

* 易修改、易维护

* 无障碍阅读支持

* 搜索引擎友好、利于SEO

* 面向未来的HTML、浏览器在未来可能提供更丰富的支持

  

### 1.1.2 结构语义化
语义元素均有一个共同特点--他们均不做任何事情。换句话说，语义元素仅仅是页面结构的规范化，并不会对内容有本质的影响。

下图展示了一个典型的页面结构。
![](https://rainylog-1256215078.cos.ap-shanghai.myqcloud.com/images/page.png)



### 1.1.3 头部
`<header>`元素有两种用法，第一是标注内容的标题，第二是标注网页的页眉，如上图你看到的那样。除非必要（内容标题附带其他信息的情况下：发布时间、作者等），一般不在内容中使用`<header>`。因而，网页中可以包含多个`<header>`元素。按照HTML5的规定，`<header>`都应包含某个级别的标题，所以应隐式或显式地包含标题，通常将不希望显示的标题设置为`display: none;`，一方面遵守规范，另一方面则提供了无障碍阅读而不至于影响到页面设计。



### 1.1.4 导航栏

导航栏使用`<nav>`看起来是理所当然的，进一步，它也用于一组文章的链接。一个页面可以包含多个`<nav>`元素，但通常仅仅在页面的主要导航部分使用它。



《HTML5：The Missing Manual》中指出了在侧栏使用`<nav>`标签的两个案例：

```html
<!-- 案例一 -->
<nav>
    <!-- 此处是链接 -->
    <aside></aside>
    <aside></aside>
</nav>

<!-- 案例二 -->
<aside>
	<nav>
    	<!-- 此处是链接 -->
    </nav>
    <section></section>
    <div></div>
</aside>
```

如果侧栏中包含其它不同于链接的其它区块，那么，使用第二种方案显然更为合适。



导航通常包含一组链接，普遍认为，链接使用列表来组织。

```html
<nav>
	<ul>
        <li><a href="#" title="链接">链接</a></li>
        <li><a href="#" title="链接">链接</a></li>
        <li><a href="#" title="链接">链接</a></li>
    </ul>
</nav>
```



### 1.1.5 附注

`<aside>`元素并不仅仅是侧栏，它表示与它周围文本没有密切关系的内容。文章中同样可以使用`<aside>`元素，来说明文章的附加内容、解释说明某个观点、相关内容链接等等。



当`<aside>`用于侧栏时，其表示整个网页的附加内容。通常的广告区域、搜索、分享链接则位于侧栏。侧栏中的`<section>`元素规定了一个区域，通常是带有标题的内容。



`<section>`标签适合标记的内容区块：

* 与页面主体并列显示的小内容块。
* 独立性内容，清单、表单等。
* 分组内容，如CMS系统中的文章分类区块。
* 比较长文档的一部分，可能仅仅是为了正确规定页面大纲。



`<div>`标签依然是可用的，当你觉得使用其它标签都不太合适的时候。新的语义元素出现之前，我们总是这么干的！



### 1.1.6 页脚

同可”包罗万象“的`<header>`元素不同，标准规定`<footer>`标签仅仅可以包含版权、来源信息、法律限制等等之类的文本或链接信息。如果想要在页脚中包含其它内容，可以使用熟悉的`<div>`来帮忙。

```html
<div>
    <aside>
    <!-- 其它内容 -->
    </aside>
    
    <footer>
    <!-- 法律、版权、来源、联系信息等 -->
    </footer>
</div>
```



### 1.1.7 主要内容

在早先的HTML5版本中并没有规定页面主体的标签，相关的书中经常会说：除去头部、尾部、侧栏等其它部分，剩下的自然是主体部分。



然而，HTML5.1中规定了一个`<main>`标签来标识主体内容。`<main>`标签不能包含在页面其它区块元素中，通常是`<body>`的子标签，或者是全局`<div>`的子标签。`<main>`标签可以帮助屏幕阅读工具识别页面的主体部分，从而让访问者迅速得到有用的信息。



### 1.1.8 文章

`<article>`表示一个完整的、自成一体的内容块。如文章或新闻报道。`<article>`应包含完整的标题、文章署名、发布时间、正文。当语义与表现发生冲突，例如有时需要将文章分多个页面显示，那么需要把每个页面的文章区域都用`<article>`标记。



文章中包含插图时，使用新的语义元素`<figure>`标签。

```html
<article>
    <h1>标题</h1>
    <p>
        <!-- 内容 -->
    </p>
    <figure>
    	<img src="#" alt="插图">
        <figcaption>这是一个插图</figcaption>
    </figure>
</article>
```

上述情况下，`<figcaption>`包含了关于插图的详细解释，则`<img>`的`alt`属性可以略去。



### 1.1.9 IFE任务

#### 1.1.9.1 任务描述

参考示例图（[点击查看](http://7xrp04.com1.z0.glb.clouddn.com/task_1_1_1.jpg)），完成一个HTML页面代码编写（不写CSS，不需要关注样式，只关注文档结构）。



#### 1.1.9.2 范例

用语义化来构建该页面的结构：[点击查看](https://github.com/geekrainy/ife/blob/master/task/task-1/index.html)



# 2、CSS篇

## 2.1 CSS常见面试题

[45道CSS经典面试题](https://segmentfault.com/a/1190000013325778)

### 2.1.1 介绍一下标准的CSS的盒子模型？与低版本IE的盒子模型有什么不同的？

* `标准盒子模型`：宽度 = 内容的宽度（content）+ border + padding + margin
* `低版本IE盒子模型`：宽度 = 内容宽度（content + border + padding）+ margin



### 2.1.2 box-sizing属性？

用来控制元素的盒子模型的解析模式，默认为`content-box`。

* `content-box`：W3C的标准盒子模型，设置元素的`height/width`属性指的是`content`部分的高/宽。
* `border-box`：IE传统盒子模型。设置元素的`height/width`属性指的是`border + padding + content`部分的高/宽。



### 2.1.3 CSS选择器有哪些？哪些属性可以继承？

* `CSS选择符`：id选择器(#myid)、类选择器(.myclassname)、标签选择器(div,h1,p)、相邻选择器(h1 + p)、子选择器(ul > li)、后代选择器(li a)、通配符选择器(*)、属性选择器(a[rel="external"])、伪类选择器(a:hover, li:nth-child)
* `可继承的属性`：font-size，font-family，color
* `不可继承的样式`：border，padding，margin，width，height
* `优先级（就近原则）`：!important > [id > class > tag]，!important比内联优先级高



### 2.1.4 CSS优先级算法如何计算？

* 元素选择符：1
* class选择符：10
* id选择符：100
* 元素标签：1000



1. !important声明的样式优先级最高，如果冲突再进行计算。
2. 如果优先级相同，则选择最后出现的样式。
3. 继承得到的样式的优先级最低。



### 2.1.5 CSS3新增伪类有哪些？

* `p:first-of-type`：选择属于其父元素的首个元素
* `p:last-of-type`：选择属于其父元素的最后元素
* `p:only-of-type`：选择属于其父元素唯一的元素
* `p:only-child`：选择属于其父元素的唯一子元素
* `p:nth-child(2)`：选择属于其父元素的第二个子元素
* `:enabled, :disabled`：表单控件的禁用状态
* `:checked`：单选框或复选框被选中



### 2.1.6 如何居中div？如何居中一个浮动元素？如何让绝对定位的div居中？

> div

```css
border: 1px solid red;
margin: 0 auto;
height: 50px;
width: 80px;
```



> 浮动元素的上下左右居中

```css
border: 1px solid red;
float: left;
position: absolute;
width: 200px;
height: 100px;
left: 50%;
top: 50%;
margin: -50px 0 0 -100px;
```



> 绝对定位的左右居中

```css
border: 1px solid black;
position: absolute;
width: 200px;
height: 100px;
margin: 0 auto;
left: 0;
right: 0;
```



### 2.1.7 display有哪些值？说明他们的作用？

* `inline`（默认）：内联
* `none`：隐藏
* `block`：块显示
* `table`：表格显示
* `list-item`：项目列表
* `inline-block`



### 2.1.8 position的值？

* `static`（默认）:按照正常文档流进行排列
* `relative`（相对定位）：不脱离文档流，参考自身静态位置通过top，bottom，left，right定位
* `absolute`（绝对定位）：参考距其最近一个不为static的父级元素通过top,bottom,left,right定位
* `fixed`（固定定位）：所固定的参照对象是可视窗口



### 2.1.9 CSS3有哪些新特性？

1. RGBA和透明度
2. background-image  background-origin(content-box/padding-box/border-box)   background-size   background-repeat
3. word-wrap（对长的不可分割单词换行）word-wrap:break-word
4. 文字阴影：text-shadow: 5px 5px 5px #FF0000; （水平阴影，垂直阴影，模糊距离，阴影颜色）
5. font-face属性：定义自己的字体
6. 圆角（边框半径）：border-radius 属性用于创建圆角
7. 边框图片：border-image: url(border.png) 30 30 round;
8. 盒阴影：box-shadow: 10px 10px 5px #888888;
9. 媒体查询：定义两套css，当浏览器的尺寸变化时会采用不同的属性



### 2.1.10 请解释一下CSS3的flexbox（弹性盒布局模型），以及使用场景？

该布局模型的目的是提供一种更加高效的方式来对容器中的条目进行布局、对齐和分配空间。在传统的布局方式中，`block`布局是把块在垂直方向从上到下依次排列的；而`inline`布局则是在水平方向来排列。弹性盒布局并没有这样内在的方向限制，可以由开发人员自由操作。



使用场景：弹性布局适合于移动前端开发，在Android和ios上也完美支持。



### 2.1.11 用纯CSS创建一个三角形的原理是什么？

首先，需要把元素的宽度、高度设为0.然后设置边框样式。

```css
width: 0;
height: 0;
border-top: 40px solid transparent;
border-left: 40px solid transparent;
border-right: 40px solid transparent;
border-bottom: 40px solid #ff0000;
```



### 2.1.12 一个满屏品字布局如何设计？

第一种真正的品字：

1. 三块高宽是确定的；
2. 上面那块用`margin: 0 auto;`居中；
3. 下面两块用`float`或者`inline-block`不换行；
4. 用`margin`调整位置使他们居中。



第二种全屏的品字布局：

上面的div设置成100%，下面的div分别宽50%，然后使用float或者inline使其不换行



### 2.1.13 常见的兼容性问题？

1. 不同浏览器的标签默认的`margin`和`padding`不一样。

   ```css
   * {
   	margin: 0;
       padding: 0;
   }
   ```

   

2. IE6双边距bug：块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大。

   ```css
   hack: diaplay: inline; // 将其转化为行内属性。
   ```

3. 渐进识别的方式，从总体中逐渐排除局部。首先，巧妙的使用"9"这一标记，将IE浏览器从所有情况中分离出来。接着，再次使用”+“将IE8和IE7、IE6分离开来，这样IE8已经独立识别。

   ```css
   {
   	background-color: #f1ee18; /*所有识别*/
       .background-color: #00deff\9; /*IE6、7、8识别*/
       +background-color: #a200ff; /*IE6、7识别*/
       _background-color: #1e0bd1; /*IE6识别*/
   }
   ```

   

4. 设置较小高度标签（一般小于10px），在IE6，IE7中高度超出自己设置高度。hack：给超出高度的标签设置overflow:hidden; 或者设置行高line-height小于你设置的高度。

5. IE下，可以使用获取常规属性的方法来获取自定义属性，也可以使用`getAttribute()`获取自定义属性；Firefox下，只能使用`getAttribute()`获取自定义属性。解决方法：统一通过`getAttribute()`获取自定义属性。

6. Chrome中文界面下默认会将小于12px的文本强制按照12px显示，可以通过加入CSS属性 `-webkit-text-size-adjust:none;`解决

7. 超链接访问过后hover样式就不出现了，被点击访问过的超链接样式不再具有`hover`和`active`了。解决方法是改变CSS属性的排列顺序：L-V-H-A(love hate):  a:link{}  a:visited{}  a:hover{}  a:active{}

   

   
### 2.1.14 为什么要初始化CSS样式？

因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。



### 2.1.15 absolute的containing block计算方式跟正常流有什么不同？

无论属于哪种，都要先找到其祖先元素中最近的`position`值不为`static`的元素，然后再判断：

1. 若此元素为`inline`元素，则`containing block`为能够包含这个元素生成的第一个和最后一个`inline box`的`padding box`（除margin，border外的区域）的最小矩形
2. 否则，则由这个祖先元素的`padding box`构成。

如果都找不到，则为`intial containing block`。



> 补充：
>
> 1. static（默认的）/relative：简单说就是它的父元素的内容框（即去掉padding的部分）
> 2. absolute：向上找最近的定位为absolute/relative的元素
> 3. fixed：它的containing block一律为根元素（html/body）



### 2.1.16 CSS里的visibility属性有个collapse属性值？在不同浏览器下有什么区别？

当一个元素的`visibility`属性被设置成`collapse`值后，对于一般的元素，它的表现跟`hidden`是一样的。

1. chrome中，使用`collapse`值和使用`hidden`没有区别。
2. firefox，opera和IE，使用`collapse`和使用`display:none`没有什么区别。



### 2.1.17 display:none与visibility:hidden的区别？

* `display:none`不显示对应的元素，在文档布局中不再分配空间（回流+重绘）
* `visibility:hidden`隐藏对应元素，在文档布局中仍保留原来的空间（重绘）



### 2.1.18 position跟display、overflow、float这些特性相互叠加后会怎么样？

display属性规定元素应该生成的框的类型；position属性规定元素的定位类型；float属性是一种布局方式，定义元素在哪个方向浮动。



类似于优先级机制：position:absolute/fixed优先级最高，有他们在时，float不起作用，display值需要调整。float或者absolute定位的元素，只能是块元素或表格。



### 2.1.19 对BFC规范（块级格式化上下文：block formatting context）的理解？

BFC规定了内部的Block Box如何布局。

定位方案：

 	1. 内部的Box会在垂直方向上一个接一个放置。
 	2. Box垂直方向的距离由margin决定，属于同一个BFC的两个相邻Box的margin会发生重叠。
 	3. 每个元素的margin box的左边，与包含块border box的左边相接触。
 	4. BFC的区域不会与float box重叠。
 	5. BFC是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
 	6. 计算BFC的高度时，浮动元素也会参与计算。



满足下列条件之一就可触发BFC

1. 根元素，html
2. float的值不为none（默认）
3. overflow的值不为visible（默认）
4. display的值为inline-block、table-cell、table-caption
5. position的值为absolute或fixed



### 2.1.20 为什么会出现浮动和什么时候需要清除浮动？清除浮动的方式？

浮动元素碰到包含它的边框或者浮动元素的边框停留。由于浮动元素不在文档流中，所以文档流的块框表现得就像浮动框不存在一样。浮动元素会漂浮在文档流的块框上。



浮动带来的问题：

 	1. 父元素的高度无法被撑开，影响与父元素同级的元素
 	2. 与浮动元素同级的非浮动元素（内联元素）会跟谁其后
 	3. 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构。



清除浮动的方式：

 	1. 父级div定义height
 	2. 最后一个浮动元素后加空div标签，并添加样式clear:both
 	3. 包含浮动元素的父标签添加样式overflow为hidden或auto
 	4. 父级div定义zoom



### 2.1.21 上下margin重合的问题

在重合元素外包裹一层容器，并触发该容器生成一个BFC。

例子：

```html
<div class="aside"></div>
<div class="text">
    <div class="main"></div>
</div>
<!--下面是css代码-->
.aside {
    margin-bottom: 100px;  
    width: 100px;
    height: 150px;
    background: #f66;
}
.main {
    margin-top: 100px;
    height: 200px;
    background: #fcc;
}
.text{
    /*盒子main的外面包一个div，通过改变此div的属性使两个盒子分属于两个不同的BFC，以此来阻止margin重叠*/
    overflow: hidden;  //此时已经触发了BFC属性。
}

```



### 2.1.22 设置元素浮动后，该元素的display值是多少？

自动变成display:block



### 2.1.23 移动端的布局用过媒体查询吗？

通过媒体查询可以为不同大小和尺寸的媒体定义不同的css，适应相应的设备的显示。

```html
// <head>里边
<link rel="stylesheet" type="text/css" href="xxx.css" media="only screen and (max-device-width:480px)">
    
//css
@media only screen and (max-device-width:480px) {
	//css样式
}
```



### 2.1.24 使用css预处理器吗？

less sass



### 2.1.25 CSS优化、提高性能的方法有哪些？

1. 避免过度约束
2. 避免后代选择符
3. 避免链式选择符
4. 使用紧凑的语法
5. 避免不必要的命名空间
6. 避免不必要的重复
7. 最好使用表示语义的名字。一个好的类名应该是描述他是什么而不是像什么
8. 避免!important，可以选择其他选择器
9. 尽可能的精简规则，你可以合并不同类里的重复规则



### 2.1.26 浏览器是怎样解析CSS选择器的？

CSS选择器的解析是从右向左解析的。若从左向右的匹配，发现不符合规则，需要进行回溯，会损失很多性能。若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。两种匹配规则的性能差别很大，是因为从右向左的匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都浪费在了失败的查找上面。

而在CSS解析完毕后，需要将解析的结果与DOM Tree的内容一起进行分析建立一颗Render Tree，最终用来进行绘图。在建立Render Tree时（Webkit中的「Attachment」过程），浏览器就要为每个DOM Tree中的元素根据CSS的解析结果（Style Rules）来确定生成怎样的Render Tree。



### 2.1.27 在网页中应该使用奇数还是偶数的字体？为什么呢？

使用偶数字体。偶数字号相对更容易和Web设计的其他部分构成比例关系。Windows自带的点阵宋体（中易宋体）从Vista开始只提供12、14、16px这三个大小的点阵，而13、15、17px时用的是小一号的点。（即每个字占的空间大了1px，但点阵没变），于是略显稀疏。



### 2.1.28 margin和padding分别适合什么场景使用？

* 何时使用margin：
  * 需要在border外侧添加空白
  * 空白处不需要背景色
  * 上下相连的两个盒子之间的空白，需要相互抵消时。
* 何时使用padding：
  * 需要在border内添加空白
  * 空白处需要北京颜色
  * 上下相连的两个盒子的空白，希望为两者之和
* 兼容性的问题：在IE5和IE6中，为float的盒子指定margin时，左侧的margin可能会变成两倍的宽度。通过改变padding或者指定盒子的display:inline解决



### 2.1.29 元素竖向的百分比设定是相对于容器的高度吗？

当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如padding-top，padding-bottom，margin-top，margin-bottom等，当按百分比设定它们时，依据的也是父容器的宽度，而不是高度。



### 2.1.30 全屏滚动的原理是什么？用到了CSS的哪些属性？

1. 原理：有点类似于轮播，整体的元素一直排列下去，假设有5个需要展示的全屏页面，那么高度是500%，只是展示100%，剩下的可以通过transform进行y轴定位，也可以通过margin-top实现
2. overflow:hidden;transition: all 1000ms ease;



### 2.1.31 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？

响应式网站设计（Responsive Web design）是一个网站能够兼容多个终端，而不是为每一个终端做一个特定的版本。

基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理。

页面头部必须有meta声明的viewport。

```html
<meta name=’viewport’ content=”width=device-width, initial-scale=1. maximum-scale=1,user-scalable=no”>
```



### 2.1.32 视觉滚动效果？

视差滚动（Parallax Scrolling）通过在网页向下滚动的时候，控制背景的移动速度比前景的移动速度慢来创建出令人惊叹的3D效果。

1. CSS3实现

   * 优点：开发时间短、性能和开发效率比较好
   * 缺点：不能兼容到低版本的浏览器

2. jQuery实现

   通过控制不同层滚动速度，计算每一层的时间，控制滚动效果。

   * 优点：能兼容到各个版本的，效果可控性好
   * 缺点：开发起来对制作者要求高

3. 插件实现方式

   例如：parallax-scrolling，兼容性十分好



### 2.1.33 ::before和:after中双冒号和单冒号有什么区别？解释一下这两个伪元素的作用

1. 单冒号（:）用于CSS3伪类，双冒号（::）用于CSS3伪元素
2. ::before就是以一个子元素的存在，定义在元素主体内容之前的一个伪元素。并不存在于dom之中，只存在在页面之中。



:before和:after这两个伪元素，是在CSS2.1里新出现的。起初，伪元素的前缀使用的是单冒号语法，但随着Web的进化，在CSS3的规范里，伪元素的语法被修改成使用双冒号，成为::before和::after



### 2.1.34 你对line-height是如何理解的？

行高是指一行文字的高度，具体说是两行文字间基线的距离。CSS中起高度作用的是height和line-height，没有定义height属性，最终其表现作用一定是line-height。

单行文本垂直居中：把line-height值设置为height一样大小的值可以实现单行文字的垂直居中，其实也可以把height删除。

多行文本垂直居中：需要设置display属性为inline-block。



### 2.1.35 怎么让Chrome支持小于12px的文字？

```css
p{font-size:10px;-webkit-transform:scale(0.8);} //0.8是缩放比例
```



### 2.1.36 让页面里的字体变清晰，变细用CSS怎么做？

-webkit-font-smoothing在window系统下没有起作用，但是在IOS设备上起作用`-webkit-font-smoothing:antialiased`是最佳的，灰度平滑。



### 2.1.37 position:fixed;在android下无效怎么处理？

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no"/>
```



### 2.1.38 如果需要手动写动画，你认为最小时间间隔是多久，为什么？

多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60*1000ms = 16.7ms。



### 2.1.39 li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法？

行框的排列会受到中间空白（回车空格）等的影响，因为空白也属于字符，这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了。

> 解决办法：

1. 可以将`<li>`代码全部写在一排
2. 浮动li中`float:left`
3. 在`ul`中用`font-size:0`（谷歌不支持）；可以使用`letter-space:-3px`



### 2.1.40 display:inline-block什么时候会显示间隙？

1. 有空格时候会有间隙

   解决：移除空格

2. margin正值的时候

   解决：margin使用负值

3. 使用font-size时候

   解决：font-size:0、letter-spacing、word-spacing



### 2.1.41 有一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度

外层div使用`position:relative;`高度要求自适应的div使用`position:absolute;top:100px;bottom:0;left:0`



### 2.1.42 png、jpg、gif这些图片格式解释一下，分别什么时候用。有没有了解过webp？

1. png是便携式网络图片（Portable Network Graphics）是一种无损数据压缩位图文件格式。优点是：压缩比高，色彩好。大多数地方都可以用。
2. jpg是一种针对相片使用的一种失真压缩方法，是一种破坏性的压缩，在色调及颜色平滑变化做的不错。在www上，被用来储存和传输照片的格式。
3. gif是一种位图文件格式，以8位色重现真色彩的图像。可以实现动画效果。
4. webp格式是谷歌在2010年推出的图片格式，压缩率只有jpg的2/3，大小比png小了45%。缺点是压缩的时间更久了，兼容性不好，目前谷歌和opera支持。



### 2.1.43 style标签写在body后与body前有什么区别？

页面加载自上而下，当然是先加载样式。

写在body标签后由于浏览器以逐行方式对HTML文档进行解析，当解析到写在尾部的样式表（外联或写在style标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染，在windows的IE下可能会出现FOUC现象（即样式失效导致的页面闪烁问题）



### 2.1.44 CSS属性overflow属性定义溢出元素内容区的内容会如何处理？

* 参数是scroll的时候，必会出现滚动条
* 参数是auto的时候，子元素内容大于父元素时出现滚动条
* 参数是visible的时候，溢出的内容出现在父元素之外
* 参数是hidden的时候，溢出隐藏



### 2.1.45 阐述一下CSS Sprites

将一个页面涉及到的所有图片都包含到一张大图中去，然后利用CSS的`background-image`，`background-repeat`，`background-position`的组合进行背景定位。利用CSS Sprites能很好地减少网页的http请求，从而大大提高页面的性能，CSS Sprites能减少页面的字节。



## 2.2 能不能讲一讲Flex布局，以及常用的属性？

[阮一峰的flex系列](https://link.juejin.cn/?target=https%3A%2F%2Fwww.ruanyifeng.com%2Fblog%2F2015%2F07%2Fflex-grammar.html)

### 2.2.1 flex布局是什么？

Flex是Flexible Box的缩写，意为”弹性布局“，用来为盒装模型提供最大的灵活性。

任何一个容器都可以指定为Flex布局。

```css
.box {
	display: flex;
}
```



行内元素也可以使用Flex布局。

```css
.box {
	display: inline-flex;
}
```



Webkit内核的浏览器，必须加上`-webkit`前缀。

```css
.box {
	display: -webkit-flex; /* Safari */
    display: flex;
}
```



> 注意，设为Flex布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。



### 2.2.2 基本概念

采用Flex布局的元素，称为Flex容器（flex container），简称”容器“。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称”项目“。

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。



项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`。



### 2.2.3 容器的属性

以下6个属性设置在容器上。

* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content



#### 2.2.3.1 flex-direction属性

`flex-direction`属性决定主轴的方向（即项目的排列方向）。

```css
.box {
	flex-direction: row | row-reverse | column | column-reverse;
}
```

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)



它可能有四个值。

* row（默认值）：主轴为水平方向，起点在左端。
* row-reverse：主轴为水平方向，起点在右端。
* column：主轴为垂直方向，起点在上沿。
* column-reverse：主轴为垂直方向，起点在下沿。



#### 2.2.3.2 flex-wrap属性

默认情况下，项目都排在一条线（又称“轴线”）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071006.png)

```css
.box {
	flex-wrap: nowrap | wrap | wrap-reverse; 
}
```



它可能取三个值。

1. `nowrap`（默认）：不换行。

   ![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071007.png)

2. `wrap`：换行，第一行在上方。

   ![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071008.jpg)

3. `wrap-reverse`：换行，第一行在下方。

   ![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071009.jpg)



#### 2.2.3.3 flex-flow

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

```css
.box {
	flex-flow: <flex-direction> || <flex-wrap>;
}
```



#### 2.2.3.4 justify-content属性

`justify-content`属性定义了项目在主轴上的对齐方式。

```css
.box {
	justify-content: flex-start | flex-end | center | space-between
}
```

![justify-content](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

它可能取五个值，具体对齐方式与轴的方向有关。下面假设主轴为从左到右。

* flex-start(默认值)：左对齐
* flex-end：右对齐
* center：居中
* space-between：两端对齐，项目之间的间隔都相等
* space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。



#### 2.2.3.5 align-items属性

`align-items`属性定义项目在交叉轴上如何对齐。

```css
.box {
	align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![align-items](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。

* flex-start：交叉轴的起点对齐
* flex-end：交叉轴的终点对齐
* center：交叉轴的中点对齐
* baseline：项目的第一行文字的基线对齐
* stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度



#### 2.2.3.6 align-content属性

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.box {
	align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png)

该属性可能取6个值。

* flex-start：与交叉轴的起点对齐
* flex-end：与交叉轴的终点对齐
* center：与交叉轴的中点对齐
* space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
* space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
* stretch（默认值）：轴线占满整个交叉轴



### 2.2.4 项目的属性

以下6个属性设置在项目上。

* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self



#### 2.2.4.1 order属性

`order`属性定义项目的排列顺序。数值越小，排列越靠前，默认为0

```css
.item {
	order: <integer>;
}
```

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071013.png)



#### 2.2.4.2 flex-grow属性

`flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

```css
.item {
	flex-grow: <number>; /* default 0 */
}
```

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071014.png)

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。



#### 2.2.4.3 flex-shrink属性

`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

```css
.item {
	flex-shrink: <number>; /* default 1 */
}
```

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071015.jpg)

如果所有项目的`flex-shrink`属性都为 1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。



#### 2.2.4.4 flex-basis属性

`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

```css
.item {
	flex-basis: <length> | auto; /* default auto */
}
```

它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间。



#### 2.2.4.5 flex属性

`flex`属性是`flex-grow`，`flex-shrink`和`flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

```css
.item {
	flex: none | [<'flex-grow'><'flex-shrink'><'flex-basis'>]
}
```

该属性有两个快捷键：`auto`（`1 1 auto`）和`none`（`0 0 auto`）。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。



#### 2.2.4.6 align-self属性

`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png)

该属性可能去6个值，除了auto，其他都与align-items属性完全一致。



# 3、JS篇

## 3.1 从原型到原型链

### 3.1.2 构造函数创建对象

我们先使用构造函数创建一个对象：

```js
function Person() {

}
var person = new Person();
person.name = 'Kevin';
console.log(person.name) // Kevin
```

在这个例子中，`Person`就是一个构造函数，我们使用`new`创建了一个实例对象`person`。

很简单吧，接下来进入正题：



### 3.1.3 prototype

每个函数都有一个`prototype`属性，就是我们经常在各种例子中看到的那个`prototype`，比如：

```js
function Person() {
}
// 虽然写在注释里，但是你要注意：
// prototype是函数才会有的属性
Person.prototype.name = 'Kevin';
var person1 = new Person();
var person2 = new Person();
console.log(person1.name) // Kevin
console.log(person2.name) // Kevin
```

那这个函数的`prototype`属性到底指向的是什么呢？是这个函数的原型吗？

其实，函数的`prototype`属性指向了一个对象，这个对象正是调用该构造函数而创建的实例的原型，也就是这个例子中的`person1`和`person2`的原型。

那什么是原型呢？你可以这样理解：每一个JavaScript对象（null除外）在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型“继承”属性。

让我们用一张图表示构造函数和实例原型之间的关系：

![prototype](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/prototype/prototype.png))



在这张图中我们用`Object.prototype`表示实例原型。

那么我们该怎么表示实例与实例原型，也就是`person`和`Person.prototype`之间的关系呢，这时候我们就要讲到第二个属性：



### 3.1.4 `__proto__`

这是每一个JavaScript对象（除了null）都具有的一个属性，叫`__proto__`，这个属性会指向该对象的原型。

为了证明这一点，我们可以在火狐或者谷歌中输入：

```js
function Person(){
    
}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

于是我们更新下关系图：

![__proto__](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/prototype/__proto__.png)

既然实例对象和构造函数都可以指向原型，那么原型是否有属性指向构造函数或者实例呢？



### 3.1.5 constructor

指向实例倒是没有，因为一个构造函数可以生成多个实例，但是原型指向构造函数倒是有的，这就要讲到第三个属性：`constructor`，每个原型都有一个`constructor`属性指向关联的构造函数。

为了验证这一点，我们可以尝试：

```js
function Person() {
}
console.log(Person === Person.prototype.constructor); // true
```

所以再更新下关系图：

![constructor](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/prototype/constructor.png)

综上我们已经得出：

```js
function Person() {
    
}
var person = new Person();

console.log(person.__proto__ == Person.prototype) // true
console.log(Person.prototype.constructor == Person) // true
// 顺便学习一个ES5的方法，可以获得对象的原型
console.log(Object.getPrototypeOf(person) === Person.prototype) // true
```



了解了构造函数、实力原型、和实例之间的关系，接下来我们讲讲实例和原型的关系：



### 3.1.6 实例与原型

当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。



举个例子：

```js
function Person() {
}
Person.prototype.name = 'Kevin';

var person = new Person();
person.name = 'Daisy';
console.log(person.name) // Daisy

delete person.name;
console.log(person.name) // Kevin
```

在这个例子中，我们给实例对象`person`添加了`name`属性，当我们打印`person.name`的时候，结果自然为`Daisy`。

但是当我们删除了`person`的`name`属性时，读取`person.name`，从`person`对象中找不到`name`属性就会从`person`的原型也就是`person.__ptoto__`，也就是`Person.prototype`中查找，幸运的是我们找到了`name`属性，结果为`Kevin`。



但是万一还没有找到呢？原型的原型又是什么呢？



### 3.1.7 原型的原型

在前面，我们已经讲了原型也是一个对象，既然是对象，我们就可以用最原始的方式创建它，那就是：

```js
var obj = new Object();
obj.name = 'Kevin'
console.log(obj.name) // Kevin
```

其实原型对象就是通过`Object`构造函数生成的，结合之前所讲，实例的`__proto__`指向构造函数的`prototype`，所以我们再更新下关系图：

![原型的原型](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/prototype/原型的原型.png)



### 3.1.8 原型链

那 Object.prototype 的原型呢？

null，我们可以打印：

```js
console.log(Object.prototype.__proto__ === null) // true
```



然而`null`究竟代表了什么呢？

引用阮一峰的《[undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)》就是：

> null表示“没有对象”，即该处不应该有值。

所以`Object.prototype.__proto__`的值为`null`跟`Object.prototype`没有原型，其实表达了一个意思。

所以查找到属性的时候查到`Object.prototype`就可以停止查找了。

最后一张关系图也可以更新为：

![原型链](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/prototype/原型链.png)

顺便还要说一下，图中由相互关联的原型组成的链状结构就是原型链，也就是蓝色的这条线。





> 补充

### 3.1.9 constructor

首先是`constructor`属性，我们看个例子：

```js
function Person() {
}
var person = new Person();
console.log(person.constructor === Person); // true
```

当获取`person.constructor`时，其实`person`中并没有`constructor`属性，当不能读取到`constructor`属性时，会从`person`的原型也就是`Person.prototype`中读取，正好原型中有该属性，所以：

```js
person.constructor === Person.prototype.constructor
```



### 3.1.10 `__proto__`

其次是`__proto__`，绝大部分浏览器都支持这个非标准的方法访问原型，然而它并不存在于`Person.prototype`中，实际上，它是来自于`Object.prototype`，与其说是一个属性，不如说是一个`getter/setter`，当使用`obj.__proto__`时，可以理解成返回了`Object.getPrototypeOf(obj)`。



### 3.1.11 真的是继承吗？

最后是关于继承，前面我们讲到“每一个对象都会从原型‘继承’‘属性’”，实际上，继承是一个十分具有迷惑性的说法，引用《你不知道的JavaScript》中的话，就是：



继承意味着复制操作，然而JavaScript默认并不会复制对象的属性，相反，JavaScript只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些。





## 3.2 JavaScript深入之词法作用域和动态作用域

### 3.2.1 作用域

作用域是指程序源代码中定义变量的区域。

作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

JavaScript采用词法作用域（lexical scoping），也就是静态作用域。



### 3.2.2 静态作用域与动态作用域

因为JavaScript采用的是词法作用域，函数的作用域在函数定义的时候就决定了。

而与词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定的。

让我们认真看个例子就能明白之间的区别：

```js
var vaule = 1;
function foo() {
	console.log(value);
}
function bar() {
	var value = 2;
    foo();
}
bar();

// 结果是？？？
```

假设JavaScript采用静态作用域，让我们分析下执行过程：



执行`foo`函数，先从`foo`函数内部查找是否有局部变量`value`，如果没有，就根据书写的位置，查找上面一层的代码，也就是`value`等于1，所以结果会打印1。



假设JavaScript采用动态作用域，让我们分析下执行过程：



执行`foo`函数，依然是从`foo`函数内部查找是否有局部变量`value`。如果没有，就从调用函数的作用域，也就是`bar`函数内部查找`value`变量，所以结果会打印2。



前面我们已经说了，JavaScript采用的是静态作用域，所以这个例子的结果是1。



### 3.2.3 动态作用域

也许你会好奇什么语言是动态作用域？

`bash`就是动态作用域，不信的话，把下面的脚本存成例如`scope.bash`，然后进行相应的目录，用命令行执行`bash ./scope.bash`，看看打印的值是多少。

```js
value = 1
function foo() {
	echo $value;
}
function bar() {
	local value = 2;
    foo;
}
bar
```



### 3.2.4 思考题

最后，让我们看一个《JavaScript权威指南》中的例子：

```js
var scope = 'global scope';
function checkscope() {
	var scope = 'local scope';
    function f() {
		return scope;
    }
    return f();
}
checkscope();
```

```js
var scope = 'global scope';
function checkscope() {
	var scope = 'local scope';
    function f() {
		return scope;
    }
    return f;
}
checkscope()();
```

两段代码都会打印：`local scope`。

原因也很简单，因为JavaScript采用的是词法作用域，函数的作用域基于函数创建的位置。

而引用《JavaScript权威指南》的回答就是：

JavaScript函数的执行用到了作用域链，这个作用域链是在函数定义的时候创建的。嵌套的函数`f()`定义在这个作用域链里，其中的变量scope一定是局部变量，不管何时何地执行`f()`，这种绑定在执行`f()`时依然有效。



但是在这里真正想让大家思考的是：

虽然两端代码执行的结果一样，但是两段代码究竟有哪些不同呢？

如果要回答这个问题，就要牵涉到很多的内容，词法作用域只是其中的一小部分，让我们期待下文。



## 3.3 JavaScript深入之执行上下文栈

### 3.3.1 顺序执行？

如果要问到JavaScript代码执行顺序的话，想必写过JavaScript的开发者都会有个直观的印象，那就是顺序执行，毕竟：

```js
var foo = function() {
	console.log('foo1');
}
foo(); // foo1

var foo = function() {
	console.log('foo2');
}
foo(); // foo2
```



然而去看这段代码：

```js
function foo() {
	console.log('foo1');
}
foo(); // foo2
function foo() {
	console.log('foo2');
}
foo(); // foo2
```

打印的结果却是两个`foo2`。



刷过面试题的都知道这是因为JavaScript引擎并非一行一行地分析和执行程序，而是一段一段地分析执行。当执行一段代码的时候，会进行一个“准备工作”，比如第一个例子中的变量提升，和第二个例子中的函数提升。



但是本文真正想让大家思考的是：这个“一段一段”中的“段”究竟是怎么划分的呢？



到底JavaScript引擎遇到一段怎样的代码时才会做“准备工作”呢？



### 3.3.2 可执行代码




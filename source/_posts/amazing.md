---
title: 新鲜玩意
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
keywords: 新奇
description: 各种新奇玩意
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg
---

## 页面浮动多边形跟随鼠标移动

![页面浮动多边形跟随鼠标移动](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e635cd6c5a0d4086ad1c2edefeb44a4e~tplv-k3u1fbpfcp-zoom-1.image)
<!-- ![页面浮动多边形跟随鼠标移动][img1] -->

```javascript
//鼠标绘制多边形
!(function() {
  //封装方法，压缩之后减少文件大小
  function get_attribute(node, attr, default_value) {
    return node.getAttribute(attr) || default_value
  }
  //封装方法，压缩之后减少文件大小
  function get_by_tagname(name) {
    return document.getElementsByTagName(name)
  }
  //获取配置参数
  function get_config_option() {
    var scripts = get_by_tagname("script"),
      script_len = scripts.length,
      script = scripts[script_len - 1] //当前加载的script
    return {
      l: script_len, //长度，用于生成id用
      z: get_attribute(script, "zIndex", -1), //z-index
      o: get_attribute(script, "opacity", 0.5), //opacity
      c: get_attribute(script, "color", "0,0,0"), //color
      n: get_attribute(script, "count", 99), //count
    }
  }
  //设置canvas的高宽
  function set_canvas_size() {
    ;(canvas_width = the_canvas.width =
      window.innerWidth ||
      document.documentElement.clientWidth ||
      document.body.clientWidth),
      (canvas_height = the_canvas.height =
        window.innerHeight ||
        document.documentElement.clientHeight ||
        document.body.clientHeight)
  }

  //绘制过程
  function draw_canvas() {
    context.clearRect(0, 0, canvas_width, canvas_height)
    //随机的线条和当前位置联合数组
    var e, i, d, x_dist, y_dist, dist //临时节点
    //遍历处理每一个点
    random_points.forEach(function(r, idx) {
      ;(r.x += r.xa),
        (r.y += r.ya), //移动
        (r.xa *= r.x > canvas_width || r.x < 0 ? -1 : 1),
        (r.ya *= r.y > canvas_height || r.y < 0 ? -1 : 1), //碰到边界，反向反弹
        context.fillRect(r.x - 0.5, r.y - 0.5, 1, 1) //绘制一个宽高为1的点
      //从下一个点开始
      for (i = idx + 1; i < all_array.length; i++) {
        e = all_array[i]
        // 当前点存在
        if (null !== e.x && null !== e.y) {
          x_dist = r.x - e.x //x轴距离 l
          y_dist = r.y - e.y //y轴距离 n
          dist = x_dist * x_dist + y_dist * y_dist //总距离, m

          dist < e.max &&
            (e === current_point &&
              dist >= e.max / 2 &&
              ((r.x -= 0.03 * x_dist), (r.y -= 0.03 * y_dist)), //靠近的时候加速
            (d = (e.max - dist) / e.max),
            context.beginPath(),
            (context.lineWidth = d / 2),
            (context.strokeStyle = "rgba(" + config.c + "," + (d + 0.2) + ")"),
            context.moveTo(r.x, r.y),
            context.lineTo(e.x, e.y),
            context.stroke())
        }
      }
    }),
      frame_func(draw_canvas)
  }
  //创建画布，并添加到body中
  var the_canvas = document.createElement("canvas"), //画布
    config = get_config_option(), //配置
    canvas_id = "c_n" + config.l, //canvas id
    context = the_canvas.getContext("2d"),
    canvas_width,
    canvas_height,
    frame_func =
      window.requestAnimationFrame ||
      window.webkitRequestAnimationFrame ||
      window.mozRequestAnimationFrame ||
      window.oRequestAnimationFrame ||
      window.msRequestAnimationFrame ||
      function(func) {
        window.setTimeout(func, 1000 / 45)
      },
    random = Math.random,
    current_point = {
      x: null, //当前鼠标x
      y: null, //当前鼠标y
      max: 20000, // 圈半径的平方
    },
    all_array
  the_canvas.id = canvas_id
  the_canvas.style.cssText =
    "position:fixed;top:0;left:0;z-index:" + config.z + ";opacity:" + config.o
  get_by_tagname("body")[0].appendChild(the_canvas)

  //初始化画布大小
  set_canvas_size()
  window.onresize = set_canvas_size
  //当时鼠标位置存储，离开的时候，释放当前位置信息
  ;(window.onmousemove = function(e) {
    e = e || window.event
    current_point.x = e.clientX
    current_point.y = e.clientY
  }),
    (window.onmouseout = function() {
      current_point.x = null
      current_point.y = null
    })
  //随机生成config.n条线位置信息
  for (var random_points = [], i = 0; config.n > i; i++) {
    var x = random() * canvas_width, //随机位置
      y = random() * canvas_height,
      xa = 2 * random() - 1, //随机运动方向
      ya = 2 * random() - 1
    // 随机点
    random_points.push({
      x: x,
      y: y,
      xa: xa,
      ya: ya,
      max: 6000, //沾附距离
    })
  }
  all_array = random_points.concat([current_point])
  //0.1秒后绘制
  setTimeout(function() {
    draw_canvas()
  }, 100)
})()
```

## 每日一言功能

![每日一言功能](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b7a8b1a5b3e4577b1b67efee35b54ba~tplv-k3u1fbpfcp-zoom-1.image)

```html
<body>
  <strong><p id="hitokoto">每日一言获取中...</p></strong>
  <script>
    //每日一言
    $(function() {
      var xhr = new XMLHttpRequest()
      xhr.open("get", "https://v1.hitokoto.cn")
      xhr.onreadystatechange = function() {
        if (xhr.readyState === 4) {
          var data = JSON.parse(xhr.responseText)
          var hitokoto = document.getElementById("hitokoto")
          hitokoto.innerText = data.hitokoto
        }
      }
      xhr.send()
    })
  </script>
</body>
```

## 鼠标点击出现爱心特效

![鼠标点击出现不同颜色爱心](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/da17b3c0f8374b11bf92938a7cb32031~tplv-k3u1fbpfcp-zoom-1.image)

```javascript
//鼠标点击爱心
!(function(e, t, a) {
  function r() {
    for (var e = 0; e < s.length; e++)
      s[e].alpha <= 0
        ? (t.body.removeChild(s[e].el), s.splice(e, 1))
        : (s[e].y--,
          (s[e].scale += 0.004),
          (s[e].alpha -= 0.013),
          (s[e].el.style.cssText =
            "left:" +
            s[e].x +
            "px;top:" +
            s[e].y +
            "px;opacity:" +
            s[e].alpha +
            ";transform:scale(" +
            s[e].scale +
            "," +
            s[e].scale +
            ") rotate(45deg);background:" +
            s[e].color +
            ";z-index:99999"))
    requestAnimationFrame(r)
  }
  function n() {
    var t = "function" == typeof e.onclick && e.onclick
    e.onclick = function(e) {
      t && t(), o(e)
    }
  }
  function o(e) {
    var a = t.createElement("div")
    ;(a.className = "heart"),
      s.push({
        el: a,
        x: e.clientX - 5,
        y: e.clientY - 5,
        scale: 1,
        alpha: 1,
        color: c(),
      }),
      t.body.appendChild(a)
  }
  function i(e) {
    var a = t.createElement("style")
    a.type = "text/css"
    try {
      a.appendChild(t.createTextNode(e))
    } catch (t) {
      a.styleSheet.cssText = e
    }
    t.getElementsByTagName("head")[0].appendChild(a)
  }
  function c() {
    return (
      "rgb(" +
      ~~(255 * Math.random()) +
      "," +
      ~~(255 * Math.random()) +
      "," +
      ~~(255 * Math.random()) +
      ")"
    )
  }
  var s = []
  ;(e.requestAnimationFrame =
    e.requestAnimationFrame ||
    e.webkitRequestAnimationFrame ||
    e.mozRequestAnimationFrame ||
    e.oRequestAnimationFrame ||
    e.msRequestAnimationFrame ||
    function(e) {
      setTimeout(e, 1e3 / 60)
    }),
    i(
      ".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"
    ),
    n(),
    r()
})(window, document)
```

![鼠标点击出现‘富强明主’字体](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5015800b668b4f329c0c414283c79d5a~tplv-k3u1fbpfcp-zoom-1.image)

```javascript

<script>
    //定义获取词语下标
var a_idx = 0;
jQuery(document).ready(function($) {
        //点击body时触发事件
    $("body").click(function(e) {
    //需要显示的词语
    var a = new Array("富强","民主", "文明", "和谐","自由", "平等", "公正","法治", "爱国", "敬业","诚信", "友善");
    //设置词语给span标签
    var $i = $("<span/>").text(a[a_idx]);
    //下标等于原来下标+1  余 词语总数
    a_idx = (a_idx + 1)% a.length;
    //获取鼠标指针的位置，分别相对于文档的左和右边缘。
    //获取x和y的指针坐标
    var x = e.pageX, y = e.pageY;
    //在鼠标的指针的位置给$i定义的span标签添加css样式
    $i.css({"z-index" : 999999,
        "top" : y - 20,
        "left" : x,
        "position" : "absolute",
        "font-weight" : "bold",
        "color" : "#ff6651"
        });
    //在body添加这个标签
    $("body").append($i);
        //animate() 方法执行 CSS 属性集的自定义动画。
        //该方法通过CSS样式将元素从一个状态改变为另一个状态。CSS属性值是逐渐改变的，这样就可以创建动画效果。
        //详情请看http://www.w3school.com.cn/jquery/effect_animate.asp
        $i.animate({
        //将原来的位置向上移动180
            "top" : y - 180,
                "opacity" : 0
         //1500动画的速度
        }, 1500, function() {
        //时间到了自动删除
            $i.remove();
        });
    });
});

</script>

```

## macOS Dock 效果

![macOS中Dock效果](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bdcbc5783ecd4331b40b1326ebc0143f~tplv-k3u1fbpfcp-watermark.image)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      html {
        font-size: 15px;
      }

      body {
        margin: 0;
        padding: 0;
        display: flex;
        width: 100%;
        min-height: 100vh;
        overflow: hidden;
        align-items: flex-end;
        background-image: linear-gradient(
          109.6deg,
          rgba(25, 170, 209, 1) 11.3%,
          rgba(21, 65, 249, 1) 69.9%
        );
      }

      .glass {
        width: 100%;
        height: 8rem;
        background: rgba(255, 255, 255, 0.25);
        box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
        backdrop-filter: blur(4px);
        -webkit-backdrop-filter: blur(4px);
        border: 1px solid rgba(255, 255, 255, 0.18);
        display: flex;
        justify-content: center;
      }

      .dock {
        --scale: 1;

        list-style: none;
        margin: 0;
        padding: 0;
        display: flex;
        justify-content: center;
        align-items: center;
      }

      .dock li {
        font-size: calc(6rem * var(--scale));
        padding: 0 0.5rem;
        cursor: default;

        position: relative;
        top: calc((6rem * var(--scale) - 6rem) / 2 * -1);

        transition: 15ms all ease-out;
      }

      .dock li.loading {
        animation: 1s loading ease-in infinite;
      }

      @keyframes loading {
        0%,
        100% {
          transform: translateY(0px);
        }
        60% {
          transform: translateY(-40px);
        }
      }
    </style>
  </head>
  <body>
    <div class="glass">
      <ul class="dock">
        <li>😃</li>
        <li>😊</li>
        <li>😜</li>
        <li>😍</li>
        <li>🤩</li>
        <li>🥳</li>
        <li>🥶</li>
      </ul>
    </div>

    <script>
      document.querySelectorAll(".dock li").forEach((li) => {
        li.addEventListener("click", (e) => {
          e.currentTarget.classList.add("loading")
        })

        li.addEventListener("mousemove", (e) => {
          let item = e.target
          let itemRect = item.getBoundingClientRect()
          let offset = Math.abs(e.clientX - itemRect.left) / itemRect.width

          let prev = item.previousElementSibling || null
          let next = item.nextElementSibling || null

          let scale = 0.6

          resetScale()

          if (prev) {
            prev.style.setProperty("--scale", 1 + scale * Math.abs(offset - 1))
          }

          item.style.setProperty("--scale", 1 + scale)

          if (next) {
            next.style.setProperty("--scale", 1 + scale * offset)
          }
        })
      })

      document.querySelector(".dock").addEventListener("mouseleave", (e) => {
        resetScale()
      })

      function resetScale() {
        document.querySelectorAll(".dock li").forEach((li) => {
          li.style.setProperty("--scale", 1)
        })
      }
    </script>
  </body>
</html>
```

## canvas 实现水印

```javascript
var watermark = {}

function setWatermark(args) {
  //声明一个怪异一点的变量，确保id的唯一性
  var id = "111.222.333.456"
  var xIndex = 15 //绘制文本的 x 坐标位置
  var yIndex = 65 //绘制文本的 y 坐标位置
  var xInterval = 25 //有多个参数时的行间间隔
  if (document.getElementById(id) !== null) {
    document.body.removeChild(document.getElementById(id))
  }
  //利用canvas绘制水印信息
  var can = document.createElement("canvas")
  can.width = 250
  can.height = 150
  var cans = can.getContext("2d")
  cans.rotate((-20 * Math.PI) / 180)
  cans.font = "17px Vedana"
  // ziti yanse
  cans.fillStyle = "rgba(200, 200, 200, 0.30)"
  cans.textAlign = "left"
  cans.textBaseline = "Middle"
  for (let i = 0; i < args.length; i++) {
    cans.fillText(args[i], xIndex, yIndex) //绘制水印文案
    yIndex += xInterval //设置每行间隔
  }
  //创建div用于显示
  var div = document.createElement("div")
  div.id = id
  div.style.pointerEvents = "none"
  div.style.top = "70px"
  div.style.left = "90px"
  div.style.position = "fixed"
  div.style.zIndex = "100000"
  div.style.width = document.documentElement.clientWidth - 50 + "px"
  div.style.height = document.documentElement.clientHeight - 50 + "px"
  //div承载水印显示
  div.style.background =
    "url(" + can.toDataURL("image/png") + ") left top repeat"
  document.body.appendChild(div)
  return id
}

watermark.set = function() {
  let args = Array.prototype.slice.apply(arguments)
  let id = setWatermark(args)
  // 检测如果水印被去掉了，自动给加上
  setInterval(function() {
    if (document.getElementById(id) === null) {
      id = setWatermark(args)
    }
  }, 500)
  //在窗口大小改变之后,自动触发加水印事件
  window.onresize = function() {
    setWatermark(args)
  }
}
window.watermark = watermark

watermark.set("绝密档案", "严禁外泄")
```


## canvas实现验证码
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Document</title>
</head>

<body>
  <canvas id="canvas" width="120" height="40"></canvas>
  <a href="#" id="changeImg">看不清，换一张</a>
  <input type="text" id="text">
  <script>
    // 随机数
    function randomNum(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    }
    /**生成一个随机色**/
    function randomColor(min, max) {
      var r = randomNum(min, max);
      var g = randomNum(min, max);
      var b = randomNum(min, max);
      return "rgb(" + r + "," + g + "," + b + ")";
    }
    drawPic();
    document.getElementById("changeImg").onclick = function (e) {
      e.preventDefault();
      drawPic();
    }

    var Vcode = ''

    /**绘制验证码图片**/
    function drawPic() {
      var canvas = document.getElementById("canvas");
      var width = canvas.width;
      var height = canvas.height;
      var ctx = canvas.getContext('2d');
      ctx.textBaseline = 'bottom';

      /**绘制背景色**/
      ctx.fillStyle = randomColor(180, 240); //颜色若太深可能导致看不清
      ctx.fillRect(0, 0, width, height);
      /**绘制文字**/
      var str = 'ABCEFGHJKLMNPQRSTWXY123456789';
      vCode = ''
      for (var i = 0; i < 4; i++) {
        var txt = str[randomNum(0, str.length)];    // 每次随机生成的数
        vCode += txt
        ctx.fillStyle = randomColor(50, 160);  //随机生成字体颜色
        ctx.font = randomNum(15, 40) + 'px SimHei'; //随机生成字体大小
        var x = 10 + i * 25;
        var y = randomNum(25, 45);
        var deg = randomNum(-45, 45);
        //修改坐标原点和旋转角度
        ctx.translate(x, y);
        ctx.rotate(deg * Math.PI / 180);
        ctx.fillText(txt, 0, 0);
        //恢复坐标原点和旋转角度
        ctx.rotate(-deg * Math.PI / 180);
        ctx.translate(-x, -y);
      }
      /* *绘制干扰线* */
      for (var i = 0; i < 4; i++) {
        ctx.strokeStyle = randomColor(40, 180);
        ctx.beginPath();
        ctx.moveTo(randomNum(0, width), randomNum(0, height));
        ctx.lineTo(randomNum(0, width), randomNum(0, height));
        ctx.stroke();
      }
      /**绘制干扰点**/
      for (var i = 0; i < 20; i++) {
        ctx.fillStyle = randomColor(0, 255);
        ctx.beginPath();
        ctx.arc(randomNum(0, width), randomNum(0, height), 1, 0, 2 * Math.PI);
        ctx.fill();
      }
      console.log("随机生成的验证码是:::", vCode);
    }
    let text = document.getElementById('text')
    text.onblur = function(e) {
      console.log(text.value,'value')
      if(text.value == vCode) {

      } else {
        alert('请输入正确的验证码')
      }
    }
  </script>
</body>

</html>
```


## 纯css实现霓虹灯效果
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .light {
      position: relative;
      padding: 25px 30px;
      color: #03e9f4;
      font-size: 24px;
      text-transform: uppercase;
      transition: 0.5s;
      letter-spacing: 4px;
      cursor: pointer;
      overflow: hidden;
      width: 200px;
      height: 100px;
    }
    .light:hover {
      background-color: #03e9f4;
      color: #050801;
      box-shadow: 0 0 5px #03e9f4,
                  0 0 25px #03e9f4,
                  0 0 50px #03e9f4,
                  0 0 200px #03e9f4;
    }
    .light div {
      position: absolute;
    }
    .light div:nth-child(1){
      width: 100%;
      height: 2px;
      top: 0;
      left: -100%;
      background: linear-gradient(to right,transparent,#03e9f4);
      animation: animate1 2s linear infinite;
    }
    .light div:nth-child(2){
      width: 2px;
      height: 100%;
      top: -100%;
      right: 0;
      background: linear-gradient(to bottom,transparent,#03e9f4);
      animation: animate2 2s linear infinite;
      animation-delay: 0.5s;
    }
    .light div:nth-child(3){
      width: 100%;
      height: 2px;
      bottom: 0;
      right: -100%;
      background: linear-gradient(to left,transparent,#03e9f4);
      animation: animate3 2s linear infinite;
      animation-delay: 1s;
    }
    .light div:nth-child(4){
      width: 2px;
      height: 100%;
      bottom: -100%;
      left: 0;
      background: linear-gradient(to top,transparent,#03e9f4);
      animation: animate4 2s linear infinite;
      animation-delay: 1.5s;
    }
    @keyframes animate1 {
      0% {
        left: -100%;
      }
      50%,100% {
        left: 100%;
      }
    }
    @keyframes animate2 {
      0% {
        top: -100%;
      }
      50%,100% {
        top: 100%;
      }
    }
    @keyframes animate3 {
      0% {
        right: -100%;
      }
      50%,100% {
        right: 100%;
      }
    }
    @keyframes animate4 {
      0% {
        bottom: -100%;
      }
      50%,100% {
        bottom: 100%;
      }
    }
  </style>
</head>
<body>
  <div class="light">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    Neon Button
  </div>
</body>
</html>
```


## canvas实现刮刮乐
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <img src="./男1.jpg" width="400" id="img" alt="" />
    <input type="text" name="" id="txt" />
    <!-- <canvas></canvas> -->
    <script>
      const oImg = document.getElementById("img")
      const txt = document.getElementById("txt")
      //oImg.readyState 图片加载状态
      if (oImg.readyState === "complete") {
        draw() //true表示已经加载完成 执行draw()方法
      } else {
        //图片加载完成执行draw方法
        oImg.onload = draw
      }

      function draw() {
        //等图片加载完成后再添加canvas画布在上面
        let can = document.createElement("canvas") //创建一个canvas画布
        can.width = oImg.width //等于图片的宽高
        can.height = oImg.height
        can.style.position = "absolute" //canvas画布设置浮动会漂浮在图片上
        can.style.left = oImg.offsetLeft + "px" //保存与画布位置一致
        can.style.top = oImg.offsetTop + "px"
        //找到图片的父级：parentNode  在oImg子元素前面添加canvas标签：insertBefore
        oImg.parentNode.insertBefore(can, oImg) //在img前面去插入canvas标签
        let ctx = can.getContext("2d")
        ctx.fillStyle = "#bbb" //刮刮乐的颜色
        ctx.fillRect(0, 0, oImg.width, oImg.height) //填充宽度

        //合成:处理合成图片的透明样式；
        //拖拽的时候，canvas图层显示透明；destination-out：新图形与原图形重叠部分透明
        ctx.globalCompositeOperation = "destination-out"
        ctx.strokeStyle = "#eee" //触笔的颜色 随便  因为它终究变成透明
        ctx.lineWidth = 30 //拖动时开始画线的线宽
        ctx.lineCap = "round" //这两步是把画笔变成圆形

        //按下，移动，抬起事件
        can.onmousedown = function (e) {
          e = e || window.event //兼容低版本IE浏览器
          //e.pageX距离文档右边缘； offsetLeft：canvas画布距离文档的右边距离
          let x = e.pageX - can.offsetLeft //得到的x是在canvas上的坐标值
          let y = e.pageY - can.offsetTop
          ctx.beginPath()
          // ctx.moveTo(  x,y )//从哪里开始来画
          ctx.arc(x, y, 15, 0, 6.3, false) //点第一下是画一个圆
          ctx.fill()
          //按下后拖拽
          can.onmousemove = function (e) {
            //拖动时一直执行下面
            e = e || window.event //兼容低版本IE浏览器
            ctx.beginPath() //拖动时开始画线
            ctx.moveTo(x, y) //起始点
            ctx.lineTo(e.pageX - can.offsetLeft, e.pageY - can.offsetTop) //移动的过程

            //每次移动的时候，样式所在的坐标；
            x = e.pageX - can.offsetLeft //第二次渲染刮图片效果的起始点应该在上一次的终止点
            y = e.pageY - can.offsetTop
            ctx.stroke() //弹出图形并恢复画布
          }
          document.onmouseup = function () {
            //抬起后将事件注销
            can.onmousemove = null
            this.onmouseup = null
            check() //完成后通过像素计算刮过的的百分比
          }
        }
        function check() {
          //获取画布的像素列表
          let data = ctx.getImageData(0, 0, can.width, can.height).data
          let n = 0 //计算透明像素的个数
          for (let i = 0; i < data.length; i += 4) {
            //感觉这一步比较消耗性能
            //RGBA
            if (
              data[i] == 0 &&
              data[i + 1] == 0 &&
              data[i + 2] == 0 &&
              data[i + 3] == 0
            ) {
              n++
            }
          }
          let f = (n * 100) / (can.width * can.height) //算出所刮的面积的占比；
          txt.value = `刮开面积:${f.toFixed(2)}%`
          //刮开面积的比例
          if (f > 30) {
            //如果所刮的面积大于30%   则将canvas画布整体清除fillRect
            ctx.beginPath()
            ctx.fillRect(0, 0, can.width, can.height)
            txt.value = "刮开面积大于30%，全部显示"
          }
        }
      }
    </script>
  </body>
</html>
```

## swiper轮播图组件
> 拖拽、回弹物料效果是参照开源项目swiper.js做的
```javascript
/**
 * 轮播组件
 * @param {object} params 配置传参
 * @param {string} params.el 组件节点 class|id|<label>
 * @param {number} params.moveTime 过渡时间（毫秒）默认 300
 * @param {number} params.interval 自动播放间隔（毫秒）默认 3000
 * @param {boolean} params.loop 是否需要回路
 * @param {boolean} params.vertical 是否垂直滚动
 * @param {boolean} params.autoPaly 是否需要自动播放
 * @param {boolean} params.pagination 是否需要底部圆点
 * @param {(index: number) => void} params.slideCallback 滑动/切换结束回调
 * @author https://github.com/Hansen-hjs
 * @description 
 * 移动端`swiper`组件，如果需要兼容`pc`自行修改对应的`touch`到`mouse`事件即可。现成效果预览：https://huangjingsheng.gitee.io/hjs/cv/demo/face/
 */
function swiper(params) {
    /**
     * css class 命名列表
     * @dec ["滑动列表","滑动item","圆点容器","底部圆点","圆点高亮"]
     */
    const classNames = [".swiper_list", ".swiper_item", ".swiper_pagination", ".swiper_dot", ".swiper_dot_active"];
    /** 滑动结束函数 */
    const slideEnd = params.slideCallback || function() {};
    /**
     * 组件节点
     * @type {HTMLElement}
     */
    let node = null;
    /**
     * item列表容器
     * @type {HTMLElement}
     */
    let nodeItem = null;
    /**
     * item节点列表
     * @type {Array<HTMLElement>}
     */
    let nodeItems = [];
    /**
     * 圆点容器
     * @type {HTMLElement}
     */
    let nodePagination = null;
    /**
     * 圆点节点列表
     * @type {Array<HTMLElement>}
     */
    let nodePaginationItems = [];
    /** 是否需要底部圆点 */
    let pagination = false;
    /** 是否需要回路 */
    let isLoop = false;
    /** 方向 `X => true` | `Y => false` */
    let direction = false;
    /** 是否需要自动播放 */
    let autoPaly = false;
    /** 自动播放间隔（毫秒）默认 3000 */
    let interval = 3000;
    /** 过渡时间（毫秒）默认 300 */
    let moveTime = 300;

    /** 设置动画 */
    function startAnimation() {
        nodeItem.style.transition = `${moveTime / 1000}s all`; 
    }

    /** 关闭动画 */
    function stopAnimation() {
        nodeItem.style.transition = "0s all";
    }

    /**
     * 属性样式滑动
     * @param {number} n 移动的距离
     */
    function slideStyle(n) {
        let x = 0, y = 0;
        if (direction) {
            y = n;
        } else {
            x = n;
        }
        nodeItem.style.transform = `translate3d(${x}px, ${y}px, 0px)`;
    }

    /**
     * 事件开始
     * @param {number} width 滚动容器的宽度
     * @param {number} height 滚动容器的高度
     */
    function main(width, height) {
        /**
         * 动画帧
         * @type {requestAnimationFrame}
         */
        const animation = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;
        /** 触摸开始时间 */
        let startTime = 0;
        /** 触摸结束时间 */
        let endTime = 0;
        /** 开始的距离 */
        let startDistance = 0;
        /** 结束的距离 */
        let endDistance = 0;
        /** 结束距离状态 */
        let endState = 0;
        /** 移动的距离 */
        let moveDistance = 0;
        /** 圆点位置 && 当前 item 索引 */
        let index = 0;
        /** 动画帧计数 */
        let count = 0;
        /** loop 帧计数 */
        let loopCount = 0;
        /** 移动范围 */
        let range = direction ? height : width;

        /** 获取拖动距离 */
        function getDragDistance() {
            /** 拖动距离 */
            let dragDistance = 0;
            // 默认这个公式
            dragDistance = moveDistance + (endDistance - startDistance);
            // 判断最大正负值
            if ((endDistance - startDistance) >= range) {
                dragDistance = moveDistance + range;
            } else if ((endDistance - startDistance) <= -range) {
                dragDistance = moveDistance - range;
            }
            // 没有 loop 的时候惯性拖拽
            if (!isLoop) {
                if ((endDistance - startDistance) > 0 && index === 0) {
                    // console.log("到达最初");
                    dragDistance = moveDistance + ((endDistance - startDistance) - ((endDistance - startDistance) * 0.6));
                } else if ((endDistance - startDistance) < 0 && index === nodeItems.length - 1) {
                    // console.log("到达最后");
                    dragDistance = moveDistance + ((endDistance - startDistance) - ((endDistance - startDistance) * 0.6));
                }
            }
            return dragDistance;
        }

        /**
         * 判断触摸处理函数 
         * @param {number} slideDistance 滑动的距离
         */
        function judgeTouch(slideDistance) {
            //	这里我设置了200毫秒的有效拖拽间隔
            if ((endTime - startTime) < 200) return true;
            // 这里判断方向（正值和负值）
            if (slideDistance < 0) {
                if ((endDistance - startDistance) < (slideDistance / 2)) return true;
                return false;
            } else {
                if ((endDistance - startDistance) > (slideDistance / 2)) return true;
                return false;
            }
        }

        /** 返回原来位置 */
        function backLocation() {
            startAnimation();
            slideStyle(moveDistance);
        }

        /**
         * 滑动
         * @param {number} slideDistance 滑动的距离
         */
        function slideMove(slideDistance) {
            startAnimation();
            slideStyle(slideDistance);
            loopCount = 0;
            // 判断 loop 时回到第一张或最后一张
            if (isLoop && index < 0) {
                // 我这里是想让滑块过渡完之后再重置位置所以加的延迟 (之前用setTimeout，快速滑动有问题，然后换成 requestAnimationFrame解决了这类问题)
                function loopMoveMin() {
                    loopCount += 1;
                    if (loopCount < moveTime / 1000 * 60) return animation(loopMoveMin);
                    stopAnimation();
                    slideStyle(range * -(nodeItems.length - 3));
                    // 重置一下位置
                    moveDistance = range * -(nodeItems.length - 3);
                }
                loopMoveMin();
                index = nodeItems.length - 3;
            } else if (isLoop && index > nodeItems.length - 3) {
                function loopMoveMax() {
                    loopCount += 1;
                    if (loopCount < moveTime / 1000 * 60) return animation(loopMoveMax);
                    stopAnimation();
                    slideStyle(0);
                    moveDistance = 0;
                }
                loopMoveMax();
                index = 0;
            }
            // console.log(`第${ index+1 }张`);	// 这里可以做滑动结束回调
            if (pagination) {
                nodePagination.querySelector(classNames[4]).className = classNames[3].slice(1);
                nodePaginationItems[index].classList.add(classNames[4].slice(1));
            }
        }

        /** 判断移动 */
        function judgeMove() {
            // 判断是否需要执行过渡
            if (endDistance < startDistance) {
                // 往上滑动 or 向左滑动
                if (judgeTouch(-range)) {
                    // 判断有loop的时候不需要执行下面的事件
                    if (!isLoop && moveDistance === (-(nodeItems.length - 1) * range)) return backLocation();
                    index += 1;
                    slideMove(moveDistance - range);
                    moveDistance -= range;
                    slideEnd(index);
                } else {
                    backLocation();
                }
            } else {
                // 往下滑动 or 向右滑动
                if (judgeTouch(range)) {
                    if (!isLoop && moveDistance === 0) return backLocation();
                    index -= 1;
                    slideMove(moveDistance + range);
                    moveDistance += range;
                    slideEnd(index)
                } else {
                    backLocation();
                }
            }
        }

        /** 自动播放移动 */
        function autoMove() {
            // 这里判断 loop 的自动播放
            if (isLoop) {
                index += 1;
                slideMove(moveDistance - range);
                moveDistance -= range;
            } else {
                if (index >= nodeItems.length - 1) {
                    index = 0;
                    slideMove(0);
                    moveDistance = 0;
                } else {
                    index += 1;
                    slideMove(moveDistance - range);
                    moveDistance -= range;
                }
            }
            slideEnd(index);
        }

        /** 开始自动播放 */
        function startAuto() {
            count += 1;
            if (count < interval / 1000 * 60) return animation(startAuto);
            count = 0;
            autoMove();
            startAuto();
        }

        // 判断是否需要开启自动播放
        if (autoPaly && nodeItems.length > 1) startAuto();

        // 开始触摸
        nodeItem.addEventListener("touchstart", ev => {
            startTime = Date.now();
            count = 0;
            loopCount = moveTime / 1000 * 60;
            stopAnimation();
            startDistance = direction ? ev.touches[0].clientY : ev.touches[0].clientX;
        });

        // 触摸移动
        nodeItem.addEventListener("touchmove", ev => {
            ev.preventDefault();
            count = 0;
            endDistance = direction ? ev.touches[0].clientY : ev.touches[0].clientX;
            slideStyle(getDragDistance());
        });

        // 触摸离开
        nodeItem.addEventListener("touchend", () => {
            endTime = Date.now();
            // 判断是否点击
            if (endState !== endDistance) {
                judgeMove();
            } else {
                backLocation();
            }
            // 更新位置 
            endState = endDistance;
            // 重新打开自动播
            count = 0;
        });
    }

    /**
     * 输出回路：如果要回路的话前后增加元素
     * @param {number} width 滚动容器的宽度
     * @param {number} height 滚动容器的高度
     */
    function outputLoop(width, height) {
        const first = nodeItems[0].cloneNode(true), last = nodeItems[nodeItems.length - 1].cloneNode(true);
        nodeItem.insertBefore(last, nodeItems[0]);
        nodeItem.appendChild(first);
        nodeItems.unshift(last);
        nodeItems.push(first);
        if (direction) {
            nodeItem.style.top = `${-height}px`;
        } else {
            nodeItem.style.left = `${-width}px`;
        }
    }

    /**
     * 输出动态布局
     * @param {number} width 滚动容器的宽度
     * @param {number} height 滚动容器的高度
     */
    function outputLayout(width, height) {
        if (direction) {
            for (let i = 0; i < nodeItems.length; i++) {
                nodeItems[i].style.height = `${height}px`;
            }
        } else {
            nodeItem.style.width = `${width * nodeItems.length}px`;
            for (let i = 0; i < nodeItems.length; i++) {
                nodeItems[i].style.width = `${width}px`;
            }
        }
    }

    /** 输出底部圆点 */
    function outputPagination() {
        let paginations = "";
        nodePagination = node.querySelector(classNames[2]);
        // 如果没有找到对应节点则创建一个
        if (!nodePagination) {
            nodePagination = document.createElement("div");
            nodePagination.className = classNames[2].slice(1);
            node.appendChild(nodePagination);
        }
        for (let i = 0; i < nodeItems.length; i++) {
            paginations += `<div class="${classNames[3].slice(1)}"></div>`;
        }
        nodePagination.innerHTML = paginations;
        nodePaginationItems = [...nodePagination.querySelectorAll(classNames[3])];
        nodePagination.querySelector(classNames[3]).classList.add(classNames[4].slice(1));
    }

    /** 初始化动态布局 */
    function initLayout() {
        node = document.querySelector(params.el);
        if (!node) return console.warn("没有可执行的节点！");
        nodeItem = node.querySelector(classNames[0]);
        if (!nodeItem) return console.warn(`缺少"${classNames[0]}"节点！`);
        nodeItems = [...node.querySelectorAll(classNames[1])];
        if (nodeItems.length == 0) return console.warn("滑动节点个数必须大于0！");
        const moveWidth = node.offsetWidth, moveHeight = node.offsetHeight;
        if (pagination) outputPagination();
        if (isLoop) outputLoop(moveWidth, moveHeight);
        outputLayout(moveWidth, moveHeight);
        main(moveWidth, moveHeight);
    }

    /** 初始化参数 */
    function initParams() {
        if (typeof params !== "object") return console.warn("传参有误");
        pagination = params.pagination || false;
        direction = params.vertical || false;
        autoPaly = params.autoPaly || false;
        isLoop = params.loop || false;
        moveTime = params.moveTime || 300;
        interval = params.interval || 3000;
        initLayout();
    }
    initParams();
}

```

## 图片懒加载
>非传统实现方式，性能最优
```javascript
/**
 * 懒加载
 * @description 可加载`<img>`、`<video>`、`<audio>`等一些引用资源路径的标签
 * @param {object} params 传参对象
 * @param {string?} params.lazyAttr 自定义加载的属性（可选）
 * @param {"src"|"background"} params.loadType 加载的类型（默认为`src`）
 * @param {string?} params.errorPath 加载失败时显示的资源路径，仅在`loadType`设置为`src`中可用（可选）
 */
function lazyLoad(params) {
    const attr = params.lazyAttr || "lazy";
    const type = params.loadType || "src";

    /** 更新整个文档的懒加载节点 */
    function update() {
        const els = document.querySelectorAll(`[${attr}]`);
        for (let i = 0; i < els.length; i++) {
            const el = els[i];
            observer.observe(el);
        }
    }

    /**
     * 加载图片
     * @param {HTMLImageElement} el 图片节点
     */
    function loadImage(el) {
        const cache = el.src; // 缓存当前`src`加载失败时候用
        el.src = el.getAttribute(attr);
        el.onerror = function () {
            el.src = params.errorPath || cache;
        }
    }

    /**
     * 加载单个节点
     * @param {HTMLElement} el 
     */
    function loadElement(el) {
        switch (type) {
            case "src":
                loadImage(el);
                break;
            case "background":
                el.style.backgroundImage = `url(${el.getAttribute(attr)})`;
                break;
        }
        el.removeAttribute(attr);
        observer.unobserve(el);
    }

    /** 
     * 监听器 
     * [MDN说明](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)
    */
    const observer = new IntersectionObserver(function(entries) {
        for (let i = 0; i < entries.length; i++) {
            const item = entries[i];
            if (item.isIntersecting) {
                loadElement(item.target);
            }
        }
    })

    update();

    return {
        observer,
        update
    }
}

```

在vue中使用指令去使用
```javascript
import Vue from "vue";

/** 添加一个加载`src`的指令 */
const lazySrc = lazyLoad({
    lazyAttr: "vlazy",
    errorPath: "./img/error.jpg"
})

Vue.directive("v-lazy", {
    inserted(el, binding) {
        el.setAttribute("vlazy", binding.value); // 跟上面的对应
        lazySrc.observer.observe(el);
    }
})

/** 添加一个加载`background`的指令 */
const lazyBg = lazyLoad({
    lazyAttr: "vlazybg",
    loadType: "background"
})

Vue.directive("v-lazybg", {
    inserted(el, binding) {
        el.setAttribute("vlazybg", binding.value); // 跟上面的对应
        lazyBg.observer.observe(el);
    }
})


```

## 上传图片
```html
<!-- 先准备好一个input标签，然后设置type="file"，最后挂载一个onchange事件 -->
<input class="upload-input" type="file" name="picture" onchange="upLoadImage(this)">
```

```javascript
/**
 * input上传图片
 * @param {HTMLInputElement} el 
 */
function upLoadImage(el) {
    /** 上传文件 */
    const file = el.files[0];
    /** 上传类型数组 */
    const types = ["image/jpg", "image/png", "image/jpeg", "image/gif"];
    // 判断文件类型
    if (types.indexOf(file.type) < 0) {
        file.value = null; // 这里一定要清空当前错误的内容
        return alert("文件格式只支持：jpg 和 png");
    }
    // 判断大小
    if (file.size > 2 * 1024 * 1024) {
        file.value = null;
        return alert("上传的文件不能大于2M");
    }
    
    const formData = new FormData();    // 这个是传给后台的数据
    formData.append("img", file);       // 这里`img`是跟后台约定好的`key`字段
    console.log(formData, file);
    // 最后POST给后台，这里我用上面的方法
    ajax({
        url: "http://xxx.com/uploadImg",
        method: "POST",
        data: {},
        formData: formData,
        overtime: 5000,
        success(res) {
            console.log("上传成功", res);
        },
        fail(err) {
            console.log("上传失败", err);
        },
        timeout() {
            console.warn("XMLHttpRequest 请求超时 !!!");
        }
    });
}

```

[base64转换和静态预览](https://github.com/Hansen-hjs/my-note/blob/master/JavaScript/upload-img.html)
## 下拉刷新组件
> 拖拽效果参考上面swiper的实现方式，下拉中的效果是可以自己定义的
```javascript
// 这里我做的不是用 window 的滚动事件，而是用最外层的绑定触摸下拉事件去实现
// 好处是我用在Vue这类单页应用的时候，组件销毁时不用去解绑 window 的 scroll 事件
// 但是滑动到底部事件就必须要用 window 的 scroll 事件，这点需要注意

/**
 * 下拉刷新组件
 * @param {object} option 配置
 * @param {HTMLElement} option.el 下拉元素（必选）
 * @param {number} option.distance 下拉距离[px]（可选）
 * @param {number} option.deviation 顶部往下偏移量[px]（可选）
 * @param {string} option.loadIcon 下拉中的 icon html（可选）
 */
function dropDownRefresh(option) {
    const doc = document;
    /** 整体节点 */
    const page = option.el;
    /** 下拉距离 */
    const distance = option.distance || 88;
    /** 顶部往下偏移量 */
    const deviation = option.deviation || 0;
    /** 顶层节点 */
    const topNode = doc.createElement("div");
    /** 下拉时遮罩 */
    const maskNode = doc.createElement("div");

    topNode.innerHTML = `<div refresh-icon style="transition: .2s all;"><svg style="transform: rotate(90deg); display: block;" t="1570593064555" viewBox="0 0 1575 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="26089" width="48" height="48"><path d="M1013.76 0v339.968H484.115692V679.778462h529.644308v339.968l529.644308-485.612308v-48.600616L1013.76 0zM243.396923 679.857231h144.462769V339.968H243.396923V679.778462z m-240.797538 0h144.462769V339.968H2.599385V679.778462z" fill="#000000" fill-opacity=".203" p-id="26090"></path></svg></div><div refresh-loading style="display: none; animation: refresh-loading 1s linear infinite;">${option.loadIcon || '<p style="font-size: 15px; color: #666;">loading...</p>'}</div>`;
    topNode.style.cssText = `width: 100%; height: ${distance}px; position: fixed; top: ${-distance + deviation}px; left: 0; z-index: 10; display: flex; flex-wrap: wrap; align-items: center; justify-content: center; box-sizing: border-box; margin: 0; padding: 0;`;
    maskNode.style.cssText = "position: fixed; top: 0; left: 0; width: 100%; height: 100vh; box-sizing: border-box; margin: 0; padding: 0; background-color: rgba(0,0,0,0); z-index: 999;";
    page.parentNode.insertBefore(topNode, page);

    /**
     * 设置动画时间
     * @param {number} n 秒数 
     */
    function setAnimation(n) {
        page.style.transition = topNode.style.transition = n + "s all";
    }

    /**
     * 设置滑动距离
     * @param {number} n 滑动的距离（像素）
     */
    function setSlide(n) {
        page.style.transform = topNode.style.transform = `translate3d(0px, ${n}px, 0px)`;
    }
    
    /** 下拉提示 icon */
    const icon = topNode.querySelector("[refresh-icon]");
    /** 下拉 loading 动画 */
    const loading = topNode.querySelector("[refresh-loading]");

    return {
        /**
         * 监听开始刷新
         * @param {Function} callback 下拉结束回调
         * @param {(n: number) => void} rangeCallback 下拉状态回调
         */
        onRefresh(callback, rangeCallback = null) {
            /** 顶部距离 */
            let scrollTop = 0;
            /** 开始距离 */
            let startDistance = 0;
            /** 结束距离 */
            let endDistance = 0;
            /** 最后移动的距离 */
            let range = 0;

            // 触摸开始
            page.addEventListener("touchstart", function (e) {
                startDistance = e.touches[0].pageY;
                scrollTop = 1;
                setAnimation(0);
            });

            // 触摸移动
            page.addEventListener("touchmove", function (e) {
                scrollTop = doc.documentElement.scrollTop === 0 ? doc.body.scrollTop : doc.documentElement.scrollTop;
                // 没到达顶部就停止
                if (scrollTop != 0) return;
                endDistance = e.touches[0].pageY;
                range = Math.floor(endDistance - startDistance);
                // 判断如果是下滑才执行
                if (range > 0) {
                    // 阻止浏览自带的下拉效果
                    e.preventDefault();
                    // 物理回弹公式计算距离
                    range = range - (range * 0.5);
                    // 下拉时icon旋转
                    if (range > distance) {
                        icon.style.transform = "rotate(180deg)";
                    } else {
                        icon.style.transform = "rotate(0deg)";
                    }
                    setSlide(range);
                    // 回调距离函数 如果有需要
                    if (typeof rangeCallback === "function") rangeCallback(range);
                }
            });

            // 触摸结束
            page.addEventListener("touchend", function () {
                setAnimation(0.3);
                // console.log(`移动的距离：${range}, 最大距离：${distance}`);
                if (range > distance && range > 1 && scrollTop === 0) {
                    setSlide(distance);
                    doc.body.appendChild(maskNode);
                    // 阻止往上滑动
                    maskNode.ontouchmove = e => e.preventDefault();
                    // 回调成功下拉到最大距离并松开函数
                    if (typeof callback === "function") callback();
                    icon.style.display = "none";
                    loading.style.display = "block";
                } else {
                    setSlide(0);
                }
            });

        },
        /** 结束下拉 */
        end() {
            maskNode.parentNode.removeChild(maskNode);
            setAnimation(0.3);
            setSlide(0);
            icon.style.display = "block";
            loading.style.display = "none";
        }
    }
}

```

## 监听滚动到底部
```javascript
/**
 * 监听滚动到底部
 * @param {object} options 传参对象
 * @param {number} options.distance 距离底部多少像素触发（px）
 * @param {boolean} options.once 是否为一次性（防止重复用）
 * @param {() => void} options.callback 到达底部回调函数
 */
function onScrollToBottom(options) {
    const { distance = 0, once = false, callback = null } = options;
    const doc = document;
    /** 滚动事件 */
    function onScroll() {
        /** 滚动的高度 */
        let scrollTop = doc.documentElement.scrollTop === 0 ? doc.body.scrollTop : doc.documentElement.scrollTop;
        /** 滚动条高度 */
        let scrollHeight = doc.documentElement.scrollTop === 0 ? doc.body.scrollHeight : doc.documentElement.scrollHeight;
        if (scrollHeight - scrollTop - distance <= window.innerHeight) {
            if (typeof callback === "function") callback();
            if (once) window.removeEventListener("scroll", onScroll);
        }
    }
    window.addEventListener("scroll", onScroll);
    // 必要时先执行一次
    // onScroll(); 
}

```

## 音频播放组件
```javascript
/**
 * `AudioContext`音频组件 
 * [资料参考](https://www.cnblogs.com/Wayou/p/html5_audio_api_visualizer.html)
 * @description 解决在移动端网页上标签播放音频延迟的方案 貌似`H5`游戏引擎也是使用这个实现
 */
function audioComponent() {
    /**
     * 音频上下文
     * @type {AudioContext}
     */
    const context = new (window.AudioContext || window.webkitAudioContext || window.mozAudioContext || window.msAudioContext)();
    /** 
     * @type {AnalyserNode} 
     */
    const analyser = context.createAnalyser();;
    /**
     * @type {AudioBufferSourceNode}
     */
    let bufferNode = null;
    /**
     * @type {AudioBuffer}
     */
    let buffer = null;
    /** 是否加载完成 */
    let loaded = false;

    analyser.fftSize = 256;

    return {
        /**
         * 加载路径音频文件
         * @param {string} url 音频路径
         * @param {(res: AnalyserNode) => void} callback 加载完成回调
         */
        loadPath(url, callback) {
            const XHR = new XMLHttpRequest(); 
            XHR.open("GET", url, true); 
            XHR.responseType = "arraybuffer"; 
            // 先加载音频文件
            XHR.onload = () => {
                context.decodeAudioData(XHR.response, audioBuffer => {
                    // 最后缓存音频资源
                    buffer = audioBuffer;
                    loaded = true;
                    typeof callback === "function" && callback(analyser);
                });
            }
            XHR.send(null);
        },

        /** 
         * 加载 input 音频文件
         * @param {File} file 音频文件
         * @param {(res: AnalyserNode) => void} callback 加载完成回调
         */
        loadFile(file, callback) {
            const FR = new FileReader();
            // 先加载音频文件
            FR.onload = e => {
                const res = e.target.result;
                // 然后解码
                context.decodeAudioData(res, audioBuffer => {
                    // 最后缓存音频资源
                    buffer = audioBuffer;
                    loaded = true;
                    typeof callback === "function" && callback(analyser);
                });
            }
            FR.readAsArrayBuffer(file);
        },

        /** 播放音频 */
        play() {
            if (!loaded) return console.warn("音频未加载完成 !!!");
            // 这里有个问题，就是创建的音频对象不能缓存下来然后多次执行 start , 所以每次都要创建然后 start()
            bufferNode = context.createBufferSource();
            bufferNode.connect(analyser);
            analyser.connect(context.destination);
            bufferNode.buffer = buffer;
            bufferNode.start(0);
        },

        /** 停止播放 */
        stop() {
            if (!bufferNode) return console.warn("音频未播放 !!!");
            bufferNode.stop();
        }
    }
}

```

## 全局监听图片错误并替换到默认图片
```javascript
window.addEventListener("error", e => {
    /** 默认`base64`图片 */
    const defaultImg = 'data:image/png;base64,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';
    /**
     * @type {HTMLImageElement}
     */
    const node = e.target;
    if (node.nodeName && node.nodeName.toLocaleLowerCase() === "img") {     
        node.style.objectFit = "cover";
        node.src = defaultImg;
    }
}, true);

```

## 复制功能
翻 ``Clipboard.js`` 这个插件库源码的时候找到核心代码 ``setSelectionRange(start: number, end: number)``，百度上搜到的复制功能全部都少了这个操作，所以搜到的复制文本代码在 ios 和 IE 等一些浏览器上复制不了。
```javascript
/**
 * 复制文本
 * @param {string} text 复制的内容
 * @param {() => void} success 成功回调
 * @param {(tip: string) => void} fail 出错回调
 */
function copyText(text, success = null, fail = null) {
    text = text.replace(/(^\s*)|(\s*$)/g, "");
    if (!text) {
        typeof fail === "function" && fail("复制的内容不能为空！");
        return;
    }
    const id = "the-clipboard";
    /**
     * 粘贴板节点
     * @type {HTMLTextAreaElement}
     */
    let clipboard = document.getElementById(id);
    if (!clipboard) {
        clipboard = document.createElement("textarea");
        clipboard.id = id;
        clipboard.readOnly = true
        clipboard.style.cssText = "font-size: 15px; position: fixed; top: -1000%; left: -1000%;";
        document.body.appendChild(clipboard);
    }
    clipboard.value = text;
    clipboard.select();
    clipboard.setSelectionRange(0, text.length);
    const state = document.execCommand("copy");
    if (state) {
        typeof success === "function" && success();
    } else {
        typeof fail === "function" && fail("复制失败");
    }
}

```

## 检测类型
```javascript
/**
 * 检测类型
 * @param {any} target 检测的目标
 * @returns {"string"|"number"|"array"|"object"|"function"|"null"|"undefined"} 只枚举一些常用的类型
 */
function checkType(target) {
    /** @type {string} */
    const value = Object.prototype.toString.call(target);
    const result = value.match(/\[object (\S*)\]/)[1];
    return result.toLocaleLowerCase();
}

```

## 格式化日期（代码极少版）
```javascript
/**
 * 获取指定日期时间戳
 * @param {number} time 毫秒数
 */
function getDateFormat(time = Date.now()) {
    const date = new Date(time);
    return `${date.toLocaleDateString()} ${date.toTimeString().slice(0, 8)}`;
}

```

## js小数精度计算
```javascript
/**
 * 数字运算（主要用于小数点精度问题）
 * @param {number} a 前面的值
 * @param {"+"|"-"|"*"|"/"} type 计算方式
 * @param {number} b 后面的值
 * @example 
 * ```js
 * // 可链式调用
 * const res = computeNumber(1.3, "-", 1.2).next("+", 1.5).next("*", 2.3).next("/", 0.2).result;
 * console.log(res);
 * ```
 */
function computeNumber(a, type, b) {
    /**
     * 获取数字小数点的长度
     * @param {number} n 数字
     */
    function getDecimalLength(n) {
        const decimal = n.toString().split(".")[1];
        return decimal ? decimal.length : 0;
    }
    /**
     * 修正小数点
     * @description 防止出现 `33.33333*100000 = 3333332.9999999995` && `33.33*10 = 333.29999999999995` 这类情况做的处理
     * @param {number} n
     */
    const amend = (n, precision = 15) => parseFloat(Number(n).toPrecision(precision));
    const power = Math.pow(10, Math.max(getDecimalLength(a), getDecimalLength(b)));
    let result = 0;

    a = amend(a * power);
    b = amend(b * power);

    switch (type) {
        case "+":
            result = (a + b) / power;
            break;
        case "-":
            result = (a - b) / power;
            break;
        case "*":
            result = (a * b) / (power * power);
            break;
        case "/":
            result = a / b;
            break;
    }

    result = amend(result);

    return {
        /** 计算结果 */
        result,
        /**
         * 继续计算
         * @param {"+"|"-"|"*"|"/"} nextType 继续计算方式
         * @param {number} nextValue 继续计算的值
         */
        next(nextType, nextValue) {
            return computeNumber(result, nextType, nextValue);
        }
    };
}

```

## 一行css适配rem
750是设计稿的宽度：之后的单位直接1:1使用设计稿的大小，单位是rem
```javascript
html{ font-size: calc(100vw / 750); }
```

## 好用的格式化日期方法
```javascript
/**
 * 格式化日期
 * @param {string | number | Date} value 指定日期
 * @param {string} format 格式化的规则
 * @example
 * ```js
 * formatDate();
 * formatDate(1603264465956);
 * formatDate(1603264465956, "h:m:s");
 * formatDate(1603264465956, "Y年M月D日");
 * ```
 */
function formatDate(value = Date.now(), format = "Y-M-D h:m:s") {
    const formatNumber = n => `0${n}`.slice(-2);
    const date = new Date(value);
    const formatList = ["Y", "M", "D", "h", "m", "s"];
    const resultList = [];
    resultList.push(date.getFullYear().toString());
    resultList.push(formatNumber(date.getMonth() + 1));
    resultList.push(formatNumber(date.getDate()));
    resultList.push(formatNumber(date.getHours()));
    resultList.push(formatNumber(date.getMinutes()));
    resultList.push(formatNumber(date.getSeconds()));
    for (let i = 0; i < resultList.length; i++) {
        format = format.replace(formatList[i], resultList[i]);
    }
    return format;
}

```

## 网页定位
这里使用百度定位，无论代码封装、调用方式还是位置准确性都比微信sdk那个好用太多了，包括在任何网页端；
[文档说明](http://lbsyun.baidu.com/index.php?title=jspopularGL/guide/getkey)
[获取百度地图key](http://lbsyun.baidu.com/apiconsole/key#/home)
```javascript
/**
 * 插入脚本
 * @param {string} link 脚本路径
 * @param {Function} callback 脚本加载完成回调
 */
function insertScript(link, callback) {
    const label = document.createElement("script");
    label.src = link;
    label.onload = function () {
        if (label.parentNode) label.parentNode.removeChild(label);
        if (typeof callback === "function") callback();
    }
    document.body.appendChild(label);
}

/**
 * 获取定位信息 
 * @returns {Promise<{ city: string, districtName: string, province: string, longitude: number, latitude: number }>}
*/
function getLocationInfo() {
    /**
     * 使用百度定位
     * @param {(value: any) => void} callback
     */
    function useBaiduLocation(callback) {
        const geolocation = new BMap.Geolocation({
            maximumAge: 10
        })
        geolocation.getCurrentPosition(function(res) {
            console.log("%c 使用百度定位 >>", "background-color: #4e6ef2; padding: 2px 6px; color: #fff; border-radius: 2px", res);
            callback({
                city: res.address.city,
                districtName: res.address.district,
                province: res.address.province,
                longitude: Number(res.longitude),
                latitude: Number(res.latitude)
            })
        })
    }

    return new Promise(function (resolve, reject) {
        if (!window._baiduLocation) {
            window._baiduLocation = function () {
                useBaiduLocation(resolve);
            }
            // ak=你自己的key
            insertScript("https://api.map.baidu.com/api?v=2.0&ak=66vCKv7PtNlOprFEe9kneTHEHl8DY1mR&callback=_baiduLocation");
        } else {
            useBaiduLocation(resolve);
        }
    })
}

```

## 输入保留数字``<input type="text">``
使用场景：用户在输入框输入内容时，实时过滤保持数字值显示；

tips：在Firefox中设置 ``<input type="number">`` 会有样式 bug
```javascript
/**
 * 输入只能是数字
 * @param {string | number} value 输入的值
 * @param {boolean} decimal 是否要保留小数
 * @param {boolean} negative 是否可以为负数
 */
function inputOnlyNumber(value, decimal, negative) {
    let result = value.toString().trim();
    if (result.length === 0) return "";
    const minus = (negative && result[0] == "-") ? "-" : "";
    if (decimal) {
        result = result.replace(/[^0-9.]+/ig, "");
        let array = result.split(".");
        if (array.length > 1) {
            result = array[0] + "." + array[1];
        }
    } else {
        result = result.replace(/[^0-9]+/ig, "");
    }
    return minus + result;
}

```

## Intl.NumberFormat（格式化数字）
* 现在JS提供了一个更加可用和规范化的API——Intl.NumberFormat。对于常用的货币格式化都有良好的支持。
>推荐使用
```javascript
new Intl.NumberFormat().format(123456.789);
// 显示结果为：123,456.789
```

```javascript
new Intl.NumberFormat('ja-JP', { style: 'currency', currency: 'JPY' }).format(12345.678);
// 结果显示为："￥12,346"
```

* 正则表达式 （古早的做法）
```javascript
const number = 1234567;
number.toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,');
// 结果为：1,234,567
```

* Date API
```javascript
const number = 123456.789;
number.toLocaleString();// 结果为：123,456.789
```

## 加密
### base64加密
```javascript
var str = 'hello';
var str64 = window.btoa(str);
console.log('经base64编码后：'+ str64); // 经base64编码后：aGVsbG8=
console.log('经base64解码后：' + window.atob(str64)); // 经base64解码后：hello

```

### 编码和解码字符串
使用JS函数的``escape()``和``unescape()``，分别是编码和解码字符串
```javascript
var escape1 = escape('我的名字是：Neo'); // 编码
var unescape1 = unescape(escape1); // 解码
console.log(escape1) // %u6211%u7684%u540D%u5B57%u662F%uFF1ANeo
console.log(unescape1) // "我的名字是：Neo"
```

## lucky-canvas【大转盘/九宫格】抽奖
[lucky-canvas【大转盘/九宫格】抽奖](https://100px.net/)

## FineBI大屏
[FineBI大屏](https://help.fanruan.com/finebi/)

## 大屏数据展示模板
[大屏数据展示模板](https://gitee.com/lvyeyou/DaShuJuZhiDaPingZhanShi)

## js拖动滑块验证功能
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>滑块解锁封装js方法</title>
    <!--注：这里首次用到了iconfont的语法，即矢量图标-->
    <link rel="stylesheet" href="font/iconfont.css" />
    <style>
      * {
        padding: 0;
        margin: 0;
      }
      #box {
        position: relative;
        width: 300px;
        height: 40px;
        margin: 0 auto;
        margin-top: 10px;
        background-color: #e8e8e8;
        box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.2);
      }
      .bgColor {
        position: absolute;
        left: 0;
        top: 0;
        width: 40px;
        height: 40px;
        background-color: lightblue;
      }
      .txt {
        position: absolute;
        width: 100%;
        height: 40px;
        line-height: 40px;
        font-size: 14px;
        color: #000;
        text-align: center;
      }
      .slider {
        position: absolute;
        left: 0;
        top: 0;
        width: 50px;
        height: 38px;
        border: 1px solid #ccc;
        background: #fff;
        text-align: center;
        cursor: move;
      }
      .slider > i {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
      .slider.active > i {
        color: green;
      }
    </style>
  </head>
  <body>
    <div id="box" onselectstart="return false;">
      <div class="bgColor"></div>
      <div class="txt">滑动解锁</div>
      <!--给i标签添加上相应字体图标的类名即可-->
      <div class="slider"><i class="iconfont icon-double-right"></i></div>
    </div>
    <script>
      //一、定义了一个获取元素的方法
      function getEle(selector) {
        return document.querySelector(selector)
      }
      //二、获取到需要用到的DOM元素
      var box = getEle("#box"), //容器
        bgColor = getEle(".bgColor"), //背景色
        txt = getEle(".txt"), //文本
        slider = getEle(".slider"), //滑块
        icon = getEle(".slider>i"),
        successMoveDistance = box.offsetWidth - slider.offsetWidth, //解锁需要滑动的距离
        downX, //用于存放鼠标按下时的位置
        isSuccess = false //是否解锁成功的标志，默认不成功

      //三、给滑块添加鼠标按下事件
      slider.onmousedown = mousedownHandler

      //3.1鼠标按下事件的方法实现
      function mousedownHandler(e) {
        bgColor.style.transition = ""
        slider.style.transition = ""
        var e = e || window.event || e.which
        downX = e.clientX
        //在鼠标按下时，分别给鼠标添加移动和松开事件
        document.onmousemove = mousemoveHandler
        document.onmouseup = mouseupHandler
      }

      //四、定义一个获取鼠标当前需要移动多少距离的方法
      function getOffsetX(offset, min, max) {
        if (offset < min) {
          offset = min
        } else if (offset > max) {
          offset = max
        }
        return offset
      }

      //3.1.1鼠标移动事件的方法实现
      function mousemoveHandler(e) {
        var e = e || window.event || e.which
        var moveX = e.clientX
        var offsetX = getOffsetX(moveX - downX, 0, successMoveDistance)
        bgColor.style.width = offsetX + "px"
        slider.style.left = offsetX + "px"

        if (offsetX == successMoveDistance) {
          success()
        }
        //如果不设置滑块滑动时会出现问题（目前还不知道为什么）
        e.preventDefault()
      }

      //3.1.2鼠标松开事件的方法实现
      function mouseupHandler(e) {
        if (!isSuccess) {
          bgColor.style.width = 0 + "px"
          slider.style.left = 0 + "px"
          bgColor.style.transition = "width 0.8s linear"
          slider.style.transition = "left 0.8s linear"
        }
        document.onmousemove = null
        document.onmouseup = null
      }

      //五、定义一个滑块解锁成功的方法
      function success() {
        isSuccess = true
        txt.innerHTML = "解锁成功"
        bgColor.style.backgroundColor = "lightgreen"
        slider.className = "slider active"
        icon.className = "iconfont icon-xuanzhong"
        //滑动成功时，移除鼠标按下事件和鼠标移动事件
        slider.onmousedown = null
        document.onmousemove = null
      }
    </script>
  </body>
</html>

```

## canvas实现贪吃蛇
[原文](https://juejin.cn/post/6959789039566192654#heading-14)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>贪吃蛇</title>
  <style>
    body {
      background-color: #eee;
    }
    .container {
      text-align: center;
    }
    .top {
      margin: 20px auto;
      width: 640px;
    }
    #score {
      float: left;
    }
    .main {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      width: 642px;
      height: 402px;
    }
    #snake {
      border: 1px solid #000;
      width: 640px;
      height: 400px;
      display: inline-block;
      z-index: 99;
      background-color: rgba(0, 0, 0, .1);
    }
    #mask {
      background-color: rgba(0, 0, 0, .5);
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      z-index: 100;
      display: block;
      color: #fff;
      line-height: 400px;
      text-align: center;
      font-size: 30px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="top">
      <span id="score">Score: 0</span>
      <button id="restart">重新开始</button>
      <button id="stop">暂停</button>
      <button id="continue">继续</button>
    </div>
    <div class="main">
      <canvas id="snake" width="640" height="400"></canvas>
      <div id="mask">开始</div>
    </div>
  </div>

<script>
  let greedySnake = null
  let score = document.querySelector('#score')
  let restart = document.querySelector('#restart')
  let stop = document.querySelector('#stop')
  let conti = document.querySelector('#continue')
  let mask = document.querySelector('#mask')

  restart.onclick = () => {
    if (!greedySnake.isStart) return
    greedySnake.start()
  }
  stop.onclick = () => {
    if (greedySnake.isStop || !greedySnake.isStart) return
    greedySnake.stop()
  }
  conti.onclick = () => {
    if (!greedySnake.isStop || !greedySnake.isStart) return
    greedySnake.continue()
  }
  mask.onclick = () => {
    if (!greedySnake.isStart) {
      greedySnake.start()
    } else {
      greedySnake.continue()
    }
  }

  // 大小为64 * 40
  class GreedySnake {
    constructor() {
      this.canvas = document.querySelector('#snake')
      this.ctx = this.canvas.getContext('2d')
      this.maxX = 64          // 最大行
      this.maxY = 40          // 最大列
      this.itemWidth = 10     // 每个点的大小
      this.direction = 'right'// up down right left 方向
      this.speed = 150        // ms 速度
      this.isStop = false     // 是否暂停
      this.isOver = false     // 是否结束
      this.isStart = false    // 是否开始
      this.score = 0          // 分数
      this.timer = null       // 移动定时器
      this.j = 1
      this.canChange = true
      
      this.grid = new Array()

      for (let i = 0; i < this.maxX; i++) {
        for (let j = 0; j < this.maxY; j++) {
          this.grid.push([i, j])
        } 
      }

      this.drawGridLine()
      this.getDirection()
    }

    // 开始
    start() {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      if (!this.isStart) {
        this.isStart = true
      }
      this.score = 0
      this.speed = 150
      this.isStop = false
      this.isOver = false
      this.direction = 'right'
      this.createSnake()
      this.createFood()
      this.draw()
      this.move()
      mask.style.display = 'none'
    }

    // 创建蛇主体
    createSnake() {
      this.snake = [
        [4, 25],
        [3, 25],
        [2, 25],
        [1, 25],
        [0, 25]
      ]
    }

    // 移动
    move() {
      if (this.isStop) return

      let [x, y] = this.snake[0]
      switch(this.direction) {
        case 'left':
          x--
          break
        case 'right':
          x++
          break
        case 'up':
          y--
          break
        case 'down':
          y++
          break
      }
      
      // 如果下一步不是食物的位置
      if (x !== this.food[0] || y !== this.food[1]) {
        this.snake.pop()
      } else {
        this.createFood()
      }

      if (this.over([x, y])) {
        this.isOver = true
        mask.style.display = 'block'
        mask.innerHTML = '结束'
        return
      }
      if (this.completed()) {
        mask.style.display = 'block'
        mask.innerHTML = '恭喜您，游戏通关'
        return
      }

      this.snake.unshift([x, y])
      
      this.draw()
      this.canChange = true
      this.timer = setTimeout(() => this.move(), this.speed)
    }
    
    // 暂停游戏
    stop() {
      if (this.isOver) return
      this.isStop = true
      mask.style.display = 'block'
      mask.innerHTML = '暂停'
    }

    // 继续游戏
    continue() {
      if (this.isOver) return
      this.isStop = false
      this.move()
      mask.style.display = 'none'
    }

    getDirection() {
      // 上38 下40 左37 右39 不能往相反的方向走
      document.onkeydown = (e) => {
        // 在贪吃蛇移动的间隔内不能连续改变两次方向
        if (!this.canChange) return
        switch(e.keyCode) {
          case 37:
            if (this.direction !== 'right') {
              this.direction = 'left'
              this.canChange = false
            }
            break
          case 38:
            if (this.direction !== 'down') {
              this.direction = 'up'
              this.canChange = false
            }
            break
          case 39:
            if (this.direction !== 'left') {
              this.direction = 'right'
              this.canChange = false
            }
            break
          case 40:
            if (this.direction !== 'up') {
              this.direction = 'down'
              this.canChange = false
            }
            break
          case 32:
            // 空格暂停与继续
            if (!this.isStop) {
              this.stop()
            } else {
              this.continue()
            }
            break
        }
      }
    }
    createPos() {
      let [x, y] = this.grid[(Math.random() * this.grid.length) | 0]

      for (let i = 0; i < this.snake.length; i++) {
        if (this.snake[i][0] == x && this.snake[i][1] == y) {
          return this.createPos()
        }
      }

      return [x, y]
    }
    // 生成食物
    createFood() {
      this.food = this.createPos()

      // 更新分数
      score.innerHTML = 'Score: '+ this.score++
      
      if (this.speed > 50) {
        this.speed--
      }
    }

    // 结束
    over([x, y]) {
      if (x < 0 || x >= this.maxX || y < 0 || y >= this.maxY) {
        return true
      }
      
      if (this.snake.some(v => v[0] === x && v[1] === y)) {
        return true
      }
    }

    // 完成
    completed() {
      if (this.snake.length == this.maxX * this.maxY) {
        return true
      }
    }

    // 网格线
    drawGridLine() {
      for (let i = 1; i < this.maxY; i++) {
        this.ctx.moveTo(0, i * this.itemWidth)
        this.ctx.lineTo(this.canvas.width, i * this.itemWidth)
      }
      
      for (let i = 1; i < this.maxX; i++) {
        this.ctx.moveTo(i * this.itemWidth, 0)
        this.ctx.lineTo(i * this.itemWidth, this.canvas.height)
      }
      this.ctx.lineWidth = 1
      this.ctx.strokeStyle = '#ddd'
      this.ctx.stroke()
    }

    // 绘制
    draw() {
      // 清空画布
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)

      this.drawGridLine()

      this.ctx.fillStyle="#000"
      this.ctx.fillRect(
        this.food[0] * this.itemWidth + this.j,
        this.food[1] * this.itemWidth + this.j,
        this.itemWidth - this.j * 2,
        this.itemWidth -  + this.j * 2
      )
      this.j ^= 1

      this.ctx.fillStyle="green"
      this.ctx.fillRect(
        this.snake[0][0] * this.itemWidth + 0.5,
        this.snake[0][1] * this.itemWidth + 0.5,
        this.itemWidth - 1,
        this.itemWidth - 1
      )
      this.ctx.fillStyle="red"
      for (let i = 1; i < this.snake.length; i++) {
        this.ctx.fillRect(
          this.snake[i][0] * this.itemWidth + 0.5,
          this.snake[i][1] * this.itemWidth + 0.5,
          this.itemWidth - 1,
          this.itemWidth - 1
        )
      }
    }
  }
  greedySnake = new GreedySnake()
</script>
</body>
</html>

```

## canvas实现带笔锋手写笔记
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <title>canvas 手写毛笔字效果</title>
    <style type="text/css">
      #canvasId {
        background-color: #ffffcc;
      }
    </style>
  </head>

  <body style="touch-action: none">
    <canvas id="canvasId" width="800" height="720"></canvas><br />
    <script>
      Array.prototype.clone = function () {
        return [].concat(this)
        //或者 return this.concat();
      }
      class Point {
        constructor(x, y, time) {
          this.x = x
          this.y = y
          this.isControl = false
          this.time = Date.now()
          this.lineWidth = 0
          this.isAdd = false
        }
      }

      class Line {
        constructor() {
          this.points = new Array()
          this.changeWidthCount = 0
          this.lineWidth = 10
        }
      }
      class HandwritingSelf {
        constructor(canvas) {
          this.canvas = canvas
          this.ctx = canvas.getContext("2d")
          // this.points = new Array();
          this.line = new Line()
          this.pointLines = new Array() //Line数组
          this.k = 0.5
          this.begin = null
          this.middle = null
          this.end = null
          this.preTime = null
          this.lineWidth = 8
          this.isDown = false
        }
        down(x, y) {
          this.isDown = true
          this.line = new Line()
          this.line.lineWidth = this.lineWidth
          let currentPoint = new Point(x, y, Date.now())
          this.addPoint(currentPoint)

          this.preTime = Date.now()
        }
        move(x, y) {
          // console.log("move:",x,y)
          if (this.isDown) {
            let currentPoint = new Point(x, y, Date.now())
            this.addPoint(currentPoint)
            this.draw()
          }
        }
        up(x, y) {
          // if (e.touches.length > 0) {
          let currentPoint = new Point(x, y, Date.now())
          this.addPoint(currentPoint)
          // }
          this.draw(true)

          this.pointLines.push(this.line)

          this.begin = null
          this.middle = null
          this.end = null
          this.isDown = false
        }
        draw(isUp = false) {
          this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height)
          this.ctx.strokeStyle = "rgba(255,20,87,1)"

          //绘制不包含this.line的线条
          this.pointLines.forEach((line, index) => {
            let points = line.points
            this.ctx.beginPath()
            this.ctx.ellipse(
              points[0].x - 1.5,
              points[0].y,
              6,
              3,
              Math.PI / 4,
              0,
              Math.PI * 2
            )
            this.ctx.fill()
            this.ctx.beginPath()
            this.ctx.moveTo(points[0].x, points[0].y)
            let lastW = line.lineWidth
            this.ctx.lineWidth = line.lineWidth
            this.ctx.lineJoin = "round"
            this.ctx.lineCap = "round"
            let minLineW = line.lineWidth / 4
            let isChangeW = false

            let changeWidthCount = line.changeWidthCount
            for (let i = 1; i <= points.length; i++) {
              if (i == points.length) {
                this.ctx.stroke()
                break
              }
              if (i > points.length - changeWidthCount) {
                if (!isChangeW) {
                  this.ctx.stroke() //将之前的线条不变的path绘制完
                  isChangeW = true
                  if (i > 1 && points[i - 1].isControl) continue
                }
                let w =
                  ((lastW - minLineW) / changeWidthCount) *
                    (points.length - i) +
                  minLineW
                points[i - 1].lineWidth = w
                this.ctx.beginPath() //为了开启新的路径 否则每次stroke 都会把之前的路径在描一遍
                // this.ctx.strokeStyle = "rgba("+Math.random()*255+","+Math.random()*255+","+Math.random()*255+",1)";
                this.ctx.lineWidth = w
                this.ctx.moveTo(points[i - 1].x, points[i - 1].y) //移动到之前的点
                this.ctx.lineTo(points[i].x, points[i].y)
                this.ctx.stroke() //将之前的线条不变的path绘制完
              } else {
                if (points[i].isControl && points[i + 1]) {
                  this.ctx.quadraticCurveTo(
                    points[i].x,
                    points[i].y,
                    points[i + 1].x,
                    points[i + 1].y
                  )
                } else if (i >= 1 && points[i - 1].isControl) {
                  //上一个是控制点 当前点已经被绘制
                } else this.ctx.lineTo(points[i].x, points[i].y)
              }
            }
          })

          //绘制this.line线条
          let points
          if (isUp) points = this.line.points
          else points = this.line.points.clone()
          //当前绘制的线条最后几个补点 贝塞尔方式增加点
          let count = 0
          let insertCount = 0
          let i = points.length - 1
          let endPoint = points[i]
          let controlPoint
          let startPoint
          while (i >= 0) {
            if (points[i].isControl == true) {
              controlPoint = points[i]
              count++
            } else {
              startPoint = points[i]
            }
            if (startPoint && controlPoint && endPoint) {
              //使用贝塞尔计算补点
              let dis =
                this.z_distance(startPoint, controlPoint) +
                this.z_distance(controlPoint, endPoint)
              let insertPoints = this.BezierCalculate(
                [startPoint, controlPoint, endPoint],
                Math.floor(dis / 6) + 1
              )
              insertCount += insertPoints.length
              var index = i //插入位置
              // 把insertPoints 变成一个适合splice的数组（包含splice前2个参数的数组）
              insertPoints.unshift(index, 1)
              Array.prototype.splice.apply(points, insertPoints)

              //补完点后
              endPoint = startPoint
              startPoint = null
            }
            if (count >= 6) break
            i--
          }
          //确定最后线宽变化的点数
          let changeWidthCount = count + insertCount
          if (isUp) this.line.changeWidthCount = changeWidthCount

          //制造椭圆头
          this.ctx.fillStyle = "rgba(255,20,87,1)"
          this.ctx.beginPath()
          this.ctx.ellipse(
            points[0].x - 1.5,
            points[0].y,
            6,
            3,
            Math.PI / 4,
            0,
            Math.PI * 2
          )
          this.ctx.fill()

          this.ctx.beginPath()
          this.ctx.moveTo(points[0].x, points[0].y)
          let lastW = this.line.lineWidth
          this.ctx.lineWidth = this.line.lineWidth
          this.ctx.lineJoin = "round"
          this.ctx.lineCap = "round"
          let minLineW = this.line.lineWidth / 4
          let isChangeW = false
          for (let i = 1; i <= points.length; i++) {
            if (i == points.length) {
              this.ctx.stroke()
              break
            }
            //最后的一些点线宽变细
            if (i > points.length - changeWidthCount) {
              if (!isChangeW) {
                this.ctx.stroke() //将之前的线条不变的path绘制完
                isChangeW = true
                if (i > 1 && points[i - 1].isControl) continue
              }

              //计算线宽
              let w =
                ((lastW - minLineW) / changeWidthCount) * (points.length - i) +
                minLineW
              points[i - 1].lineWidth = w
              this.ctx.beginPath() //为了开启新的路径 否则每次stroke 都会把之前的路径在描一遍
              // this.ctx.strokeStyle = "rgba(" + Math.random() * 255 + "," + Math.random() * 255 + "," + Math.random() * 255 + ",0.5)";
              this.ctx.lineWidth = w
              this.ctx.moveTo(points[i - 1].x, points[i - 1].y) //移动到之前的点
              this.ctx.lineTo(points[i].x, points[i].y)
              this.ctx.stroke() //将之前的线条不变的path绘制完
            } else {
              if (points[i].isControl && points[i + 1]) {
                this.ctx.quadraticCurveTo(
                  points[i].x,
                  points[i].y,
                  points[i + 1].x,
                  points[i + 1].y
                )
              } else if (i >= 1 && points[i - 1].isControl) {
                //上一个是控制点 当前点已经被绘制
              } else this.ctx.lineTo(points[i].x, points[i].y)
            }
          }
        }

        addPoint(p) {
          if (this.line.points.length >= 1) {
            let last_point = this.line.points[this.line.points.length - 1]
            let distance = this.z_distance(p, last_point)
            if (distance < 10) {
              return
            }
          }

          if (this.line.points.length == 0) {
            this.begin = p
            p.isControl = true
            this.pushPoint(p)
          } else {
            this.middle = p
            let controlPs = this.computeControlPoints(
              this.k,
              this.begin,
              this.middle,
              null
            )
            this.pushPoint(controlPs.first)
            this.pushPoint(p)
            p.isControl = true

            this.begin = this.middle
          }
        }

        addOtherPoint(p1, p2, w1, w2) {
          let otherPoints = new Array()
          let dis = this.z_distance(p1, p2)
          if (dis >= 25) {
            otherPoints.push(p1)
            let insertPCount = Math.floor(dis / 20)
            for (let j = 0; j < insertPCount; j++) {
              let insertP = new Point(
                p1.x + ((j + 1) / (insertPCount + 1)) * (p2.x - p1.x),
                p1.y + ((j + 1) / (insertPCount + 1)) * (p2.y - p1.y)
              )
              insertP.isAdd = true
              otherPoints.push(insertP)
            }
            otherPoints.push(p2)
          }
          let count = otherPoints.length
          if (count > 0) {
            console.log("addOtherPoint")
            debugger
            let diffW = (w2 - w1) / (count - 1)
            for (let i = 1; i < count; i++) {
              let w = w1 + diffW * i
              this.ctx.beginPath()
              this.ctx.lineWidth = w
              this.ctx.moveTo(otherPoints[i - 1].x, otherPoints[i - 1].y)
              this.ctx.lineTo(otherPoints[i].x, otherPoints[i].y)
              this.ctx.stroke()
            }
          }
          return otherPoints
        }
        pushPoint(p) {
          //排除重复点
          if (
            this.line.points.length >= 1 &&
            this.line.points[this.line.points.length - 1].x == p.x &&
            this.line.points[this.line.points.length - 1].y == p.y
          )
            return
          this.line.points.push(p)
        }
        computeControlPoints(k, begin, middle, end) {
          if (k > 0.5 || k <= 0) return

          let diff1 = new Point(middle.x - begin.x, middle.y - begin.y)
          let diff2 = null
          if (end) diff2 = new Point(end.x - middle.x, end.y - middle.y)

          // let l1 = (diff1.x ** 2 + diff1.y ** 2) ** (1 / 2)
          // let l2 = (diff2.x ** 2 + diff2.y ** 2) ** (1 / 2)

          let first = new Point(middle.x - k * diff1.x, middle.y - k * diff1.y)
          let second = null
          if (diff2)
            second = new Point(middle.x + k * diff2.x, middle.y + k * diff2.y)
          return { first: first, second: second }
        }
        // W_current =
        // 　　W_previous + min( abs(k*s - W_previous), distance * K_width_unit_change) (k * s-W_previous) >= 0
        // 　　W_previous - min( abs(k*s - W_previous), distance * K_width_unit_change) (k * s-W_previous) < 0
        // 　　W_current 　　　　  当前线段的宽度
        // 　　W_previous　　　　与当前线条相邻的前一条线段的宽度
        // 　　distance 　　	　　    当前线条的长度
        // 　　w_k 　　　　　　　	设定的一个固定阈值,表示:单位距离内, 笔迹的线条宽度可以变化的最大量.
        // 　　distance * w_k 　　  即为当前线段的长度内, 笔宽可以相对于前一条线段笔宽的基础上, 最多能够变宽或者可以变窄多少.
        z_linewidth(b, e, bwidth, step) {
          if (e.time == b.time) return bwidth

          let max_speed = 2.0
          let d = this.z_distance(b, e)
          let s = d / (e.time - b.time) //计算速度
          console.log("s", e.time - b.time, s)
          s = s > max_speed ? max_speed : s

          // let w = (max_speed - s) / max_speed;
          let w = 0.5 / s

          let max_dif = d * step
          console.log(w, bwidth, max_dif)
          if (w < 0.05) w = 0.05
          if (Math.abs(w - bwidth) > max_dif) {
            if (w > bwidth) w = bwidth + max_dif
            else w = bwidth - max_dif
          }
          // printf("d:%.4f, time_diff:%lld, speed:%.4f, width:%.4f\n", d, e.t-b.t, s, w);
          return w
        }
        z_distance(b, e) {
          return Math.sqrt(Math.pow(e.x - b.x, 2) + Math.pow(e.y - b.y, 2))
        }
        BezierCalculate(poss, precision) {
          //维度，坐标轴数（二维坐标，三维坐标...）
          let dimersion = 2

          //贝塞尔曲线控制点数（阶数）
          let number = poss.length

          //控制点数不小于 2 ，至少为二维坐标系
          if (number < 2 || dimersion < 2) return null

          let result = new Array()

          //计算杨辉三角
          let mi = new Array()
          mi[0] = mi[1] = 1
          for (let i = 3; i <= number; i++) {
            let t = new Array()
            for (let j = 0; j < i - 1; j++) {
              t[j] = mi[j]
            }

            mi[0] = mi[i - 1] = 1
            for (let j = 0; j < i - 2; j++) {
              mi[j + 1] = t[j] + t[j + 1]
            }
          }

          //计算坐标点
          for (let i = 0; i < precision; i++) {
            let t = i / precision
            let p = new Point(0, 0)
            p.isAdd = true
            result.push(p)
            for (let j = 0; j < dimersion; j++) {
              let temp = 0.0
              for (let k = 0; k < number; k++) {
                temp +=
                  Math.pow(1 - t, number - k - 1) *
                  (j == 0 ? poss[k].x : poss[k].y) *
                  Math.pow(t, k) *
                  mi[k]
              }
              j == 0 ? (p.x = temp) : (p.y = temp)
            }
          }

          return result
        }
      }

      //以下代码为鼠标移动事件部分
      let handwriting = new HandwritingSelf(document.getElementById("canvasId"))
      // document.ontouchstart = document.onmousedown
      document.onpointerdown = function (e) {
        if (e.type == "touchstart")
          handwriting.down(e.touches[0].pageX, e.touches[0].pageY)
        else handwriting.down(e.x, e.y)
      }
      // document.ontouchmove = document.onmousemove
      document.onpointermove = function (e) {
        if (e.type == "touchmove")
          handwriting.move(e.touches[0].pageX, e.touches[0].pageY)
        else handwriting.move(e.x, e.y)
      }
      // document.ontouchend = document.onmouseup
      document.onpointerup = function (e) {
        if (e.type == "touchend")
          handwriting.up(e.touches[0].pageX, e.touches[0].pageY)
        else handwriting.up(e.x, e.y)
      }
    </script>
  </body>
</html>

```

## 代码雨
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Code</title>
    <style>
      body {
        margin: 0;
        overflow: hidden;
      }
    </style>
  </head>

  <body>
    <canvas id="myCanvas"></canvas>
    <script>
      const width = (document.getElementById("myCanvas").width = 1920) //screen.availWidth;
      const height = (document.getElementById("myCanvas").height = 1080) //screen.availHeight;
      const ctx = document.getElementById("myCanvas").getContext("2d")
      const arr = Array(Math.ceil(width / 10)).fill(0)
      const str = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789".split("")

      function rain() {
        ctx.fillStyle = "rgba(0,0,0,0.05)"
        ctx.fillRect(0, 0, width, height)
        ctx.fillStyle = "#0f0"
        arr.forEach(function (value, index) {
          ctx.fillText(
            str[Math.floor(Math.random() * str.length)],
            index * 10,
            value + 10
          )
          arr[index] =
            value >= height || value > 8888 * Math.random() ? 0 : value + 10
        })
      }

      setInterval(rain, 30)
    </script>
  </body>
</html>

```

## qq企鹅
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      html,
      body,
      div,
      strong {
        margin: 0;
        padding: 0;
      }
      body {
        overflow: hidden;
      }
      .wrap {
        width: 600px;
        margin: 50px auto 0;
        position: relative;
      }
      /* ……………………………………………………………………企鹅头部…………………………………………………………………… */

      /* 上半部分黑色头部 */
      .headtop {
        width: 240px;
        height: 110px;
        background: #000;
        margin: 0 auto;
        border-top-left-radius: 120px 110px;
        border-top-right-radius: 120px 110px;
        position: relative;
        z-index: 999;
      }
      /* 下半部分黑色头部 */
      .headbottom {
        width: 240px;
        height: 90px;
        background: #000;
        border-bottom-left-radius: 120px 90px;
        border-bottom-right-radius: 120px 90px;
        position: absolute;
        top: 110px;
        z-index: 1;
      }

      h1 {
        position: absolute;
      }
      /* 左眼部分 */
      .lefteye {
        width: 46px;
        height: 70px;
        background: #fff;
        border-radius: 50% 50%;
        position: absolute;

        top: 30px;
        left: 64px;
        z-index: 2;
      }
      .lefteye_in {
        width: 20px;
        height: 30px;
        background: #000;
        border-radius: 50% 50%;
        margin-left: 23px;
        margin-top: 20px;
      }
      .eyeshow {
        display: block;
        width: 8px;
        height: 10px;
        border-radius: 50% 50%;
        background: #fff;
        position: absolute;
        top: 26px;
        left: 32px;
      }

      /*右眼部分 */
      .righteye {
        width: 46px;
        height: 70px;
        background: #fff;
        border-radius: 50% 50%;
        position: absolute;
        top: 30px;
        left: 130px;
        z-index: 2;
      }
      .righteye_in {
        width: 19px;
        height: 23px;
        background: #000;
        border-top-left-radius: 17px 30px;
        border-top-right-radius: 17px 30px;
        border-bottom-left-radius: 5px;
        border-bottom-right-radius: 5px;
        border: 1px solid #000;
        margin-left: 7px;
        margin-top: 16px;
      }
      .eyebai {
        display: block;
        width: 12px;
        height: 19px;
        border-top-left-radius: 5px 14px;
        border-top-right-radius: 8px 14px;
        background: #fff;
        margin-top: 10px;
        margin-left: 4px;
      }
      /*右眼部分 */

      /*嘴巴*/
      .mouth {
        width: 158px;
        height: 56px;
        background: #ffa600;
        border-radius: 50%;
        position: absolute;
        top: 106px;
        left: 42px;
        z-index: 2;
      }
      .mouth_bar {
        width: 126px;
        height: 30px;
        background: #000;
        position: absolute;
        top: 142px;
        left: 55px;
        z-index: 2;
        border-bottom-left-radius: 76px 96px;
        border-bottom-right-radius: 76px 96px;
      }
      .mouth_bar1 {
        width: 126px;
        height: 20px;
        background: #ffa600;
        position: absolute;
        z-index: 3;
        border-bottom-left-radius: 104px 32px;
        border-bottom-right-radius: 104px 32px;
      }
      /*嘴巴*/
      /* ……………………………………………………………………end  企鹅头部…………………………………………………………………… */

      /* ……………………………………………………………………star 企鹅身体…………………………………………………………………… */

      /* 企鹅身体黑色部分 */
      .body {
        width: 276px;
        height: 260px;
        background: #000;
        position: absolute;
        top: 142px;
        left: 167px;
        border-top-left-radius: 160px 140px;
        border-bottom-left-radius: 160px 140px;
        border-top-right-radius: 160px 140px;
        border-bottom-right-radius: 160px 140px;
        z-index: 2;
      }
      /* 企鹅身体黑色部分 */

      /* 红色围脖 */
      .body_1 {
        width: 264px;
        height: 137px;
        background: #ff0000;
        border: 5px solid #000;
        border-top-left-radius: 195px 100px;
        border-bottom-left-radius: 237px 146px;
        border-top-right-radius: 195px 100px;
        border-bottom-right-radius: 269px 146px;
        position: absolute;
        bottom: 159px;
        left: 0px;
      }

      .body_2 {
        width: 249px;
        height: 139px;
        position: absolute;
        background: #000;
        top: -33px;
        left: 7px;
        border-radius: 50%;
        border: 1px #000 solid;
      }

      .body_3 {
        width: 241px;
        height: 145px;
        position: absolute;
        background: red;
        top: 0px;
        left: 5px;
        border-radius: 50%;
      }
      /* 红色围脖 */

      /* 白色企鹅肚子 */
      .tummy {
        width: 240px;
        height: 240px;
        background: #fff;
        position: absolute;
        top: 11px;
        left: 17px;
        border-radius: 50%;
      }
      /* 白色企鹅肚子 */

      /* 企鹅口袋 */
      .pocket {
        width: 58px;
        height: 78px;
        position: absolute;
        top: 72px;
        left: 19px;
        border: 3px solid #000;
        background: red;
        border-top-left-radius: 20px 52px;
        border-bottom-left-radius: 40px 40px;
        border-top-right-radius: 0px 0px;
        border-bottom-right-radius: 21px 21px;
      }

      .pocket .pocket_line1 {
        width: 11px;
        height: 43px;
        border-bottom-left-radius: 29px 57px;
        border-top-left-radius: 0px 0px;
        border: 9px solid #000;
        border-top: none;
        border-right: none;
        position: absolute;
        top: 0px;
        left: 30px;
        -webkit-transform: rotateZ(10deg);
        -moz-transform: rotateZ(10deg);
        -ms-transform: rotateZ(10deg);
        -o-transform: rotateZ(10deg);
        transform: rotateZ(10deg);
      }

      .pocket .pocket_line2 {
        width: 2px;
        height: 45px;
        border-bottom-left-radius: 11px 24px;
        border-top-left-radius: 10px 15px;
        border: 9px solid red;
        border-top: none;
        border-right: none;
        position: absolute;
        top: 0px;
        left: 2px;
      }

      /* 企鹅左右手 */
      .lefthand,
      .righthand {
        width: 49px;
        height: 160px;
        background: #000;
        position: absolute;
      }

      .lefthand {
        top: 20px;
        left: -29px;
        border-top-left-radius: 89px 166px;
        border-top-right-radius: 6px 63px;
        border-bottom-left-radius: 85px 194px;
        border-bottom-right-radius: 40px 128px;
        -webkit-transform: rotateZ(20deg);
        -moz-transform: rotateZ(20deg);
        -ms-transform: rotateZ(20deg);
        -o-transform: rotateZ(20deg);
        transform: rotateZ(20deg);
        -webkit-animation: left_rotate 0.5s infinite;
        -moz-animation: left_rotate 0.5s infinite;
        -ms-animation: left_rotate 0.5s infinite;
        -o-animation: left_rotate 0.5s infinite;
        animation: left_rotate 0.5s infinite;
        -webkit-animation-direction: alternate;
        -moz-animation-direction: alternate;
        -ms-animation-direction: alternate;
        -o-animation-direction: alternate;
        animation-direction: alternate;
      }
      .righthand {
        top: 20px;
        left: 258px;
        border-top-right-radius: 89px 166px;
        border-bottom-right-radius: 85px 194px;
        border-top-left-radius: 6px 63px;
        border-bottom-left-radius: 40px 128px;
        -webkit-transform: rotateZ(-20deg);
        -moz-transform: rotateZ(-20deg);
        -ms-transform: rotateZ(-20deg);
        -o-transform: rotateZ(-20deg);
        transform: rotateZ(-20deg);
        -webkit-animation: right_rotate 0.5s infinite;
        -moz-animation: right_rotate 0.5s infinite;
        -ms-animation: right_rotate 0.5s infinite;
        -o-animation: right_rotate 0.5s infinite;
        animation: right_rotate 0.5s infinite;
        -webkit-animation-direction: alternate;
        -moz-animation-direction: alternate;
        -ms-animation-direction: alternate;
        -o-animation-direction: alternate;
        animation-direction: alternate;
      }
      /* 企鹅左右手 */
      /* ……………………………………………………………………end 企鹅身体…………………………………………………………………… */

      /* ……………………………………………………………………star 企鹅脚部…………………………………………………………………… */
      .footer .left_footer,
      .footer .right_footer {
        width: 134px;
        height: 74px;
        position: absolute;
        background: #ffa600;
        border: 3px solid #000;
        border-radius: 50%;
        z-index: 1;
      }

      .footer .left_footer {
        top: 347px;
        left: 163px;
      }

      .footer .right_footer {
        top: 347px;
        left: 320px;
      }
      /* ……………………………………………………………………end 企鹅脚部…………………………………………………………………… */

      @keyframes left_rotate {
        from {
          -webkit-transform-origin: right 30%;
          -webkit-transform: rotateZ(60deg);
        }

        to {
          -webkit-transform-origin: right 30%;
          -webkit-transform: rotateZ(30deg);
        }
      }

      @keyframes right_rotate {
        from {
          -webkit-transform-origin: left 30%;
          -webkit-transform: rotateZ(-60deg);
        }

        to {
          -webkit-transform-origin: left 30%;
          -webkit-transform: rotateZ(-30deg);
        }
      }

      @-webkit-keyframes left_rotate {
        from {
          -webkit-transform-origin: right 30%;
          -webkit-transform: rotateZ(60deg);
        }

        to {
          -webkit-transform-origin: right 30%;
          -webkit-transform: rotateZ(30deg);
        }
      }

      @-webkit-keyframes right_rotate {
        from {
          -webkit-transform-origin: left 30%;
          -webkit-transform: rotateZ(-60deg);
        }

        to {
          -webkit-transform-origin: left 30%;
          -webkit-transform: rotateZ(-30deg);
        }
      }
      @-moz-keyframes left_rotate {
        from {
          transform-origin: right 30%;
          transform: rotateZ(60deg);
        }

        to {
          transform-origin: right 30%;
          transform: rotateZ(30deg);
        }
      }

      @-moz-keyframes right_rotate {
        from {
          transform-origin: left 30%;
          transform: rotateZ(-60deg);
        }

        to {
          transform-origin: left 30%;
          transform: rotateZ(-30deg);
        }
      }
      @-ms-keyframes left_rotate {
        from {
          transform-origin: right 30%;
          transform: rotateZ(60deg);
        }

        to {
          transform-origin: right 30%;
          transform: rotateZ(30deg);
        }
      }

      @-ms-keyframes right_rotate {
        from {
          transform-origin: left 30%;
          transform: rotateZ(-60deg);
        }

        to {
          transform-origin: left 30%;
          transform: rotateZ(-30deg);
        }
      }
      @-o-keyframes left_rotate {
        from {
          transform-origin: right 30%;
          transform: rotateZ(60deg);
        }

        to {
          transform-origin: right 30%;
          transform: rotateZ(30deg);
        }
      }

      @-o-keyframes right_rotate {
        from {
          transform-origin: left 30%;
          transform: rotateZ(-60deg);
        }

        to {
          transform-origin: left 30%;
          transform: rotateZ(-30deg);
        }
      }
    </style>
  </head>
  <body>
    <div class="wrap">
      <!-- 企鹅头部 -->
      <div class="headtop">
        <div class="headbottom"></div>
        <!-- 眼睛部分 -->
        <h1 class="lefteye">
          <p class="lefteye_in">
            <strong class="eyeshow"></strong>
          </p>
        </h1>
        <h1 class="righteye">
          <p class="righteye_in">
            <strong class="eyebai"></strong>
          </p>
        </h1>
        <!-- 嘴巴部分 -->
        <h1 class="mouth"></h1>
        <p class="mouth_bar">
          <strong class="mouth_bar1"></strong>
        </p>
      </div>

      <!-- 企鹅头部 -->

      <!-- 企鹅身体 -->
      <div class="body">
        <div class="tummy">
          <div class="pocket">
            <div class="pocket_line1">
              <div class="pocket_line2"></div>
            </div>
          </div>
        </div>
        <div class="hand">
          <div class="lefthand"></div>
          <div class="righthand"></div>
        </div>
        <div class="body_1">
          <div class="body_2">
            <div class="body_3"></div>
          </div>
        </div>
      </div>
      <!-- 企鹅身体 -->

      <!-- 企鹅脚丫 -->
      <div class="footer">
        <div class="left_footer"></div>
        <div class="right_footer"></div>
      </div>
      <!-- 企鹅脚丫 -->
    </div>
  </body>
</html>

```
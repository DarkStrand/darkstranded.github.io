---
title: 授权认证
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
keywords: 授权认证
description: 授权认证
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/3.jpg
---

[原文](https://blog.csdn.net/huangpb123/article/details/103933400)



# 一、先了解几个基础概念

## 1.1 什么是认证（Authentication）

通俗地讲就是验证当前用户的身份。



互联网中的认证：

- 用户名密码登录
- 邮箱发送登录链接
- 手机号接收验证码
- 只要你能收到邮箱/验证码，就默认你是账号的主人



## 1.2 什么是授权（Authorization）

用户授予第三方应用访问该用户某些资源的权限。



实现授权的方式有：cookie、session、token、OAuth。



## 1.3 什么是凭证（Credentials）

实现认证和授权的前提是需要一种媒介（证书）来标记访问者的身份。



在互联网应用中，一般网站（如掘金）会有两种模式，游客模式和登录模式。游客模式下，可以正常浏览网站上面的文章，一旦想要点赞/收藏/分享文章，就需要登录或者注册账号。当用户登录成功后，服务器会给该用户使用的浏览器颁发一个令牌（token），这个令牌用来表明你的身份，每次浏览器发送请求时会带上这个令牌，就可以使用游客模式下无法使用的功能。



# 二、Cookie

[原文](https://blog.csdn.net/huangpb123/article/details/109107461)



## 2.1 了解Cookie

- Cookie最开始被设计出来是为了弥补HTTP在状态管理上的不足。HTTP协议是一个无状态协议，客户端向服务器发请求，服务器返回响应，故事就这样结束了，但是下次发请求如何让服务端知道客户端是谁呢？这种情况下，就产生了Cookie。
- cookie存储在客户端：cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。因此，服务端脚本就可以读、写存储在客户端的cookie的值。
- cookie是不可跨域的：每个cookie都会绑定单一的域名（绑定域名下的子域都是有效的），无法在别的域名下获取使用，**同域名不同端口也是允许共享使用的**。



服务器向客户端发送Cookie是通过HTTP响应报文实现的，在Set-Cookie中设置需要向客户端发送的cookie，cookie格式如下：

```js
Set-Cookie: "name=value;domain=.domain.com;path=/;expires=Sat, 11 Jun 2016 11:29:42 GMT;HttpOnly;secure"
```



## 2.2 检测cookie是否启用

有些用户为了避免隐私泄露会在它们的浏览器中禁用cookie。因此，在js代码使用cookie前，首先要确保cookie是启用的。可以用`navigator.cookieEnabled`属性来判断，如果值为true，则当前cookie是启用的；反之则是禁用的（但是，只具备“当前浏览器会话生命周期”的非持久化cookie仍然是启用的）。



## 2.3 cookie属性：有效期和作用域

cookie默认的有效期很短暂，它只能维持在Web浏览器的会话期间，一旦用户关闭浏览器，cookie保存的数据就丢失了。要注意的是，这与sessionStorage的有效期还是有区别的：cookie的作用域不是局限在浏览器的单个窗口中，它的有效期和整个浏览器进程而不是单个浏览器窗口的有效期一致。如果想要延长cookie的有效期，可以通过设置max-age属性。一旦设置了有效期，浏览器就会将cookie数据存储在一个文件中，并且直到过了指定的有效期才会删除该文件。

和localStorage和sessionStorage类似，cookie的作用域是通过文档源和文档路径来确定的。该作用域通过cookie的path和domain属性也是可配置的。默认情况下，cookie和创建它的Web页面有关，并对该Web页面以及和该Web页面同目录或者子目录的其他Web页面可见。比如，Web页面 http://www.example.com/catalog/index.html 页面创建了一个cookie，那么该cookie对 http://www.example.com/catalog/order.html 页面和 http://www.example.com/catalog/widgets/index.html 页面都是可见的，但它对 http://www.example.com/about.html 页面不可见。

cookie的作用域默认由文档源限制。但是，有的大型网站想要子域之间能够互相共享cookie。比如，order.example.com 域下的服务器想要读取 catalog.example.com 域下设置的cookie值。可以将 catalog.example.com 域下的cookie的path属性设置成“/”，其domain属性设置成“.example.com”，那么该cookie就对所有 catalog.example.com，order.example.com 以及任何其他 example.com 域下的任何其他服务器都可见。如果没有为一个cookie设置域属性，那么domain属性的默认值是当前Web服务器的主机名。要注意的是，cookie的域只能设置为当前服务器的域。



## 2.4 Cookie的重要属性

| 属性       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| name=value | 键值对，设置Cookie的名称及相对应的值，都必须是字符串类型（name不区分大小写）<br />- 如果值为Unicode字符，需要字符编码。<br />- 如果值为二进制数据，则需要使用BASE64编码。 |
| domain     | 指定cookie所属域名，默认是当前域名                           |
| path       | 指定cookie在哪个路径（路由）下生效，默认是'/'。<br />如果设置为`/abc`，则只有`/abc`下的路由可以访问到该cookie，如：`/abc/red`。 |
| expires    | 过期时间（GMT时间格式），在设置的某个时间点后该cookie就会失效。<br />如果客户端和服务器时间不一致，使用expires就会存在偏差。<br />一般浏览器的cookie都是默认储存的，当关闭浏览器结束这个会话的时候，这个cookie也会被删除 |
| max-age    | cookie有效期，单位秒。如果为正数，则该cookie在maxAge秒后失效。如果为负数，该cookie为临时cookie，关闭浏览器即失效，浏览器也不会以任何形式保存cookie。如果为0，表示删除该cookie。默认为-1。<br />- 优先级高于expires |
| HttpOnly   | 如果给某个cookie设置了httpOnly属性，则无法通过JS脚本读写该cookie的信息，但还是能通过Application中手动修改cookie，所以只是在一定程度上可以防止CSRF攻击，不是绝对的安全 |
| secure     | 该cookie是否仅被使用安全协议传输。安全协议有HTTPS，SSL等，在网络上传输数据之前先将数据加密。默认为false。<br />当secure值为true时，cookie在HTTP中是无效的。 |

cookie集合中的每个cookie都拥有这些属性，而且每个cookie的这些属性都是独立分开的，各自控制各自的cookie。



## 2.5 cookie的局限性

### 2.5.1 每个域名下cookie个数限制

- Chrome和Safari没有做硬件限制
- Firefox最多50个cookie
- IE7和之后的版本最多可以有50个cookie
- IE6或更低版本最多20个cookie

RFC 2965标准不允许浏览器保存超过300个cookie，为每个web服务器保存的cookie数不能超过20个（是对整个服务器而言，而不仅仅指服务器上的页面和站点），而且，每个cookie保存的数据不能超过4kb。实际上，现代浏览器允许cookie总数超过300个，但是部分浏览器对单个cookie大小仍有4kb的限制。



## 2.6 客户端对cookie的存取

### 2.6.1 读取cookie

可以用`document.cookie`获取当前页面可用的cookie集合，其返回的值是一个字符串，该字符串都是由一系列键/值对组成，不同键/值对之间通过“分号和空格”分开。例如：

```js
document.cookie;
// "name1=value1; name2=value2"
```

这些返回的cookie值并不包含键/值以外的其他cookie属性。



### 2.6.2 设置cookie

```js
document.cookie = `name=${encodeURIComponent(name)}; max-age=1000;`;
```

name这个cookie会被添加到现有的cookie集合中。



由于cookie的键/值中的值是不允许包含分号、逗号和空白符，因此，在存储前一般可以采用`encodeURIComponent()`函数对值进行编码。相应的，读取cookie值的时候要用`decodeURIComponent()`函数解码。



### 2.6.3 更新cookie

要改变cookie的值，需要使用相同的名字、路径和域，但是新的值重新设置cookie的值。同样地，设置新`max-age`属性就可以改变原来的cookie的有效期。



### 2.6.4 删除cookie

要删除一个cookie，需要使用相同的名字、路径和域，然后指定一个任意（非空）的值，并且将`max-age`属性指定为0，再次设置cookie。



## 2.7 封装对cookie的操作

由于cookie的读写非常不方便，我们可以自己封装一些函数来处理cookie。



### 2.7.1 获取全部cookie，返回一个对象

```js
function getAllCookies() {
  let cookies = {};
  const all = document.cookie;
  if (all) {
    const list = all.split('; ');
    list.forEach(cookie => {
      const cookieArr = cookie.split('=');
      const name = cookieArr[0];
      cookies[name] = decodeURIComponent(cookieArr[1]);
    });
  }
  return cookies;
}
```



### 2.7.2 获取单个cookie，设置、删除cookie

```js
class cookieUtils {
  get(name) {
    var arr,
      reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
    if ((arr = document.cookie.match(reg))) return decodeURIComponent(arr[2]);
    else return null;
  }
 
  set(name, value, daysToLive) {
    let cookie = `${name}=${encodeURIComponent(value)}`;
    // daysToLive指天数
    if (typeof daysToLive === 'number') {
      cookie += `; max-age=${daysToLive * 24 * 60 * 60}`;
    }
    document.cookie = cookie;
  }
 
  delete(name) {
    var date = new Date();
    date.setTime(date.getTime() - 10000);
    document.cookie = name + "=-1;expires=" + date.toGMTString();
  }
}
 
export default new cookieUtils();
```

在Chrome控制台Application的Cookies里可以对cookie进行读写操作。

**移动端对cookie的支持不是很好，而session需要基于cookie实现，所以移动端常用的是token**。



## 2.8 服务器端设置cookie示例（Node）

```js
var http = require('http');
var fs = require('fs');
 
http.createServer(function(req, res) {
    res.setHeader('status', '200 OK');
    res.setHeader('Set-Cookie', 'isVisit=true;domain=.yourdomain.com;path=/;max-age=1000');
    res.write('Hello World');
    res.end();
}).listen(8888);
 
console.log('running localhost:8888')
```

![](https://img-blog.csdnimg.cn/20200111122509425.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5ncGIxMjM=,size_16,color_FFFFFF,t_70)



# 三、什么是Session

- session是另一种记录服务器和客户端会话状态的机制

- session是基于cookie实现的，session存储在服务器端，sessionid会被存储到客户端的cookie中

  ![](https://img-blog.csdnimg.cn/20200111152258612.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5ncGIxMjM=,size_16,color_FFFFFF,t_70)



> session认证流程：

- 用户第一次请求服务器的时候，服务器根据用户提交的相关信息，创建对应的Session
- 请求返回时将此Session的唯一标识SessionID返回给浏览器
- 浏览器接收到服务器返回的SessionID后，会将此信息存入到Cookie中，同时Cookie记录此SessionID属于哪个域名
- 当用户第二次访问服务器的时候，请求组会自动把此域名下的Cookie信息也发送给服务端，服务端会从Cookie中获取SessionID，再根据SessionID查找对应的Session信息，如果没有找到说明用户没有登录或者登录失效，如果找到Session证明用户已经登录可执行后面操作。

根据以上流程可知，**SessionID是连接Cookie和Session的一道桥梁**，大部分系统也是根据此原理来验证用户登录状态。



# 四、Cookie和Session的区别

- 安全性：Session比Cookie安全，Session是存储在服务器端的，Cookie是存储在客户端的。
- 存取值的类型不同：Cookie只支持存字符串数据，Session可以存任意数据类型。
- 有效期不同：Cookie可设置为长时间保持，比如我们经常使用的默认登录功能，Session一般失效时间较短，客户端关闭（默认情况下）或者Session超时都会失效。
- 存储大小不同：单个Cookie保存的数据不能超过4k，Session可存储数据远高于Cookie，但是当访问量过多，会占用过多的服务器资源。



# 五、什么是Token（令牌）

## 5.1 Access Token

- 访问资源接口（API）时所需要的资源凭证
- 简单token的组成：uid（用户唯一的身份标识）、time（当前时间的时间戳）、sign（签名，token的前几位以哈希算法压缩成的一定长度的十六进制字符串）

## 5.2 服务器对token的存储方式

1. 存到数据库中，每次客户端请求的时候取出来验证（服务端有状态）
2. 存到redis中，设置过期时间，每次客户端请求的时候取出来验证（服务端有状态）
3. 不存，每次客户端请求的时候根据之前的生成方法再生成一次来验证（JWT，服务端无状态）



> 特点

- 服务端无状态化、可扩展性好
- 支持移动端设备
- 安全
- 支持跨程序调用



> token的身份验证流程

![token的身份验证流程](https://img-blog.csdnimg.cn/20200114175835320.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5ncGIxMjM=,size_16,color_FFFFFF,t_70)

1. 客户端使用用户名和密码请求登录
2. 服务端收到请求，去验证用户名与密码
3. 验证成功后，服务端会签发一个`token`并把这个`token`发送给客户端
4. 客户端收到`token`以后，会把它存储起来，比如放在`cookie`里或者`localStorage`里
5. 客户端每次向服务端请求资源的时候需要带着服务端签发的`token`
6. 服务端收到请求，然后去验证客户端请求里面带着的`token`，如果验证成功，就向客户端返回请求的数据



- 每一次请求都需要携带`token`，需要把`token`放到`HTTP`的`Header`里
- token完全由应用管理，所以它可以避开同源策略



> 注意：登录时token不宜保存在localStorage，被XSS攻击时容易泄露。所以比较好的方式是把token写在cookie里。为了保证XSS攻击时cookie不被获取，还要设置cookie的http-only。





## 5.3 Refresh Token

- 另外一种token——`refresh token`
- `refresh token`是专门用于刷新`access token`的token。如果没有`refresh token`，也可以刷新`access token`，但每次刷新都要用户输入登录用户名与密码，会很麻烦。有了`refresh token`，可以减少这个麻烦，客户端直接用`refresh token`去更新`access token`，无需用户进行额外的操作。

![refresh token](https://img-blog.csdnimg.cn/20200114182230452.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5ncGIxMjM=,size_16,color_FFFFFF,t_70)



- `Access Token`的有效期比较短，当`Access Token`由于过期而失效时，使用`Refresh Token`就可以获取到新的Token，如果`Refresh Token`也失效了，用户就只能重新登录了。
- `Refresh Token`及过期时间是存储在服务器的数据库中，只有在申请新的`Access Token`时才会验证，不会对业务接口响应时间造成影响，也不需要像`Session`一样一直保持在内存中以应对大量的请求。



# 六、Token和Session的区别

- `Session`是一种记录服务器和客户端会话状态的机制，使服务端有状态化，可以记录会话信息。而`Token`是令牌，访问资源接口（API）时所需要的资源凭证。`Token`使服务端无状态化，不会存储会话信息。
- `Session`和`Token`并不矛盾，作为身份认证`Token`安全性比`Session`好，因为每一个请求都有签名还能防止监听以及重复攻击，而`Session`就必须依赖链路层来保障通讯安全了。**如果你需要实现有状态的会话，仍然可以增加Session来在服务器端保存一些状态**。
- 如果你的用户数据可能需要和第三方共享，或者允许第三方调用API接口，用`Token`。如果永远只是自己的网站，自己的App，用什么就无所谓了。



# 七、什么是JWT

- JSON Web Token（简称JWT）是目前最流行的跨域认证解决方案。
- 是一种`认证授权机制`。
- JWT是为了在网络应用环境间`传递声明`而执行的一种基于JSON的开放标准。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源。比如用在用户登录上。
- 可以使用HMAC算法或者是RSA的公/私秘钥对JWT进行签名。因为数字签名的存在，这些传递的信息是可信的。



## 7.1 JWT的原理

JWT的原理是，服务器认证以后，生成一个JSON对象，返回给用户，就像下面这样。

```json
{
	"姓名": "张三",
    "角色": "管理员",
    "到期时间": "2018年7月1日0点0分"
}
```

以后，用户与服务端通信的时候，都要发回这个JSON对象。服务器完全只靠这个对象认定用户身份。为了防止用户篡改数据，服务器在生成这个对象的时候，会加上签名。

![JWT原理](https://img-blog.csdnimg.cn/20200114184248329.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5ncGIxMjM=,size_16,color_FFFFFF,t_70)



> JWT认证流程

1. 用户输入用户名/密码登录，服务端认证成功后，会返回给客户端一个JWT
2. 客户端将token保存到本地（通常使用localStorage，也可以使用cookie）
3. 当用户希望访问一个受保护的路由或者资源的时候，需要请求头的Authorization字段中使用Bearer模式添加JWT，其内容看起来是下面这样

```js
Authorization: Bearer <token>
```

4. 服务端的保护路由将会检查请求头Authorization中的JWT信息，如果合法，则允许用户的行为
5. 因为JWT是自包含的（内部包含了一些会话信息），因此减少了需要查询数据库的需要
6. 因为JWT并不使用Cookie的，所以你可以使用任何域名提供你的API服务而不需要担心跨域问题
7. 因为用户的状态不再存储在服务端的内存中，所以这是一种无状态的认证机制

## 7.2 JWT的数据结构

实际的JWT大概就像下面这样：

![jwt数据结构](https://img-blog.csdnimg.cn/20200114185248941.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5ncGIxMjM=,size_16,color_FFFFFF,t_70)

它是一个很长的字符串，中间用点(.)分割成三个部分。



JWT的三个部分依次如下：

- Header（头部）
- Payload（负载）
- Signature（签名）



[JWT详细数据结构](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)



## 7.3 生成JWT

- https://jwt.io/
- https://www.jsonwebtoken.io/



# 八、Token和JWT的区别

## 8.1 相同

- 都是访问资源的令牌
- 都可以记录用户的信息
- 都是使服务端无状态化
- 都是只有验证成功后，客户端才能访问服务端上受保护的资源



## 8.2 区别

- Token：服务端验证客户端发送过来的Token时，还需要查询数据库获取用户信息，然后验证Token是否有效。
- JWT：将Token和Payload加密后存储于客户端，服务端只需要使用秘钥解密进行校验（校验也是JWT自己实现的）即可，不需要查询或者减少查询数据库，因为JWT自包含了用户信息和加密的数据。



# 九、要注意的问题

## 9.1 使用session时需要考虑的问题

- 将session存储在服务器里面，当用户同时在线量比较多时，这些session会占据较多的内存，需要在服务器定期的去清理过期的session
- 当网站采用**集群部署**的时候，会遇到多台web服务器之间如何做session共享的问题。因为session是由单个服务器创建的，但是处理用户请求的服务器不一定是那个创建session的服务器，那么该服务器就无法拿到之前已经放入到session中的登录凭证之类的信息了。
- 当多个应用要共享session时，除了以上问题，还会遇到跨域问题，因为不同的应用可能部署的主机不一样，需要在各个应用做好cookie跨域的处理
- **sessionid是存储在cookie中的，假如浏览器禁止cookie或不支持cookie怎么办？**一般会把sessinid跟在url参数后面即重写url，所以session不一定非得需要靠cookie实现
- **移动端对cookie的支持不是很好，而session需要基于cookie实现，所以移动端常用的是token**



## 9.2 使用JWT时需要考虑的问题

- 因为JWT并不依赖Cookie的，所以你可以使用任何域名提供你的API服务而不需要担心跨域问题
- JWT默认是不加密，但也是可以加密的。生成原始Token以后，可以用密钥再加密一次。
- JWT不加密的情况下，不能将秘密数据写入JWT。
- JWT不仅可以用于认证，也可以用于交换信息。有效使用JWT，可以降低服务器查询数据库的次数。
- JWT最大的优势是服务器不再需要存储Session，使得服务器认证鉴权业务可以方便扩展。但这也是JWT最大的缺点：由于服务器不需要存储Session状态，因此使用过程中无法废弃某个Token或者更改Token的权限。也就是说一旦JWT签发了，到期之前就会始终有效，除非服务器部署额外的逻辑。
- JWT本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT的有效期应该设置得比较短。对于一些较重要的权限，使用时应该再次对用户进行认证。
- JWT适合一次性的命令认证，颁发一个有效期极短的JWT，即使暴露了危险也很小，由于每次操作都会生成新的JWT，因此也没必要保存JWT，真正实现无状态。
- 为了减少盗用，JWT不应该使用HTTP协议明码传输，要使用HTTPS协议传输。

## 9.3 使用加密算法时需要考虑的问题

- 绝不要以明文存储密码
- **永远使用哈希算法来处理密码，绝不要使用Base64或其他编码方式来存储密码，这和以明文存储密码是一样的，使用哈希，而不要使用编码。**编码以及加密，都是双向的过程，而密码是保密的，应该只被它的所有者知道，这个过程必须是单向的。哈希正是用于做这个的，从来没有解哈希这种说法，但是编码就存在解码，加密就存在解密。
- 绝不要使用弱哈希或已被破解的哈希算法，像MD5或SHA1，只使用强密码哈希算法。
- 绝不要以明文形式显示或发送密码，即使是对密码的所有者也应该这样。如果你需要“忘记密码”的功能，可以随机生成一个新的**一次性**（这点很重要）密码，然后把这个密码发送给用户。



## 9.4只要关闭浏览器 ，session 真的就消失了？

不对。浏览器关闭时，是不会主动去通知服务器的。之所以会有这种错觉，是大部分 session 机制都使用会话 cookie 来保存 sessionId，而关闭浏览器后这个 cookie 就消失了，再次连接服务器时也就无法找到原来的 session。如果服务器设置的 cookie 被保存在硬盘上，或者使用某种手段改写浏览器发出的 HTTP 请求头，把原来的 sessionId 发送给服务器，则再次打开浏览器仍然能够打开原来的 session。
恰恰是由于关闭浏览器不会导致 session 被删除，迫使服务器为 session 设置了一个失效时间，当距离客户端上一次使用 session 的时间超过这个失效时间时，服务器就认为客户端已经停止了活动，才会把 session 删除以节省存储空间。
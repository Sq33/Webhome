# 博学谷环境搭建

## 设置虚拟主机

- 在C盘的www目录，新建了一个`boxuegu`文件夹


- `D:\phpStudy\Apache\conf\extra`目录下找到`httpd-vhosts.conf`,打开编辑

```javascript
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    #根目录
    DocumentRo ot "C:\www\boxuegu"
    #域名
    ServerName boxuegu.com
    #完整域名
    ServerAlias www.boxuegu.com
    ErrorLog "logs/dummy-host.example.com-error.log"
    CustomLog "logs/dummy-host.example.com-access.log" common
</VirtualHost>
```

- 修改host文件，`C:\Windows\System32\drivers\etc`找到`hosts`文件,添加以下内容

```javascript
127.0.0.1 boxuegu.com
127.0.0.1 www.boxuegu.com
```

**注意：如果提示没有权限保存，先把hosts文件复制到桌面上，进行修改，修改完成之后，再拖回去，覆盖即可。** 

- 重启服务器，进行测试

##  修改项目结构

```javascript
public 
	assets    //资源文件
    images    //图片
    js        //js文件
    less	  //样式文件
uploads       //上传图片文件夹
views         //所有的html页面
	advert    //广告
    category  //分类
    course    //课程
	default   //默认页面
	teacher   //讲师管理
	user      //用户管理
```

+ 修改项目的结构
+ 初始化git，将代码提交到github

## 提取页面公共部分

```javascript
aside.html: 侧边栏内容
header.html:头部内容
css.html:css内容
js.html :js内容
```

通过php的`include`将页面引入进来即可。

问题：html页面并不能识别`<?php ?>`标签，只有php文件才能识别，应该怎么办？

## index.php

通过index.php的include解决html文件无法识别`<?php ?>`标签的问题

### 基本功能

```php
$path = $_SERVER['PATH_INFO'];
//去除第一个/
$path = substr($path, 1);
//对路径信息进行切割
$arr = explode("/", $path);
//通过include引入对应的html文件，这样引入的html文件就可以识别html标签了
include '/views/' . $arr[0] ."/". $arr[1] . ".html";
```

### 路径中省略index.php

[Rewrite](http://www.jb51.net/article/52956.htm)主要的功能就是实现URL的跳转和隐藏真实地址，基于Perl语言的正则表达式规范。平时帮助我们实现拟静态，虚拟目录，域名跳转，防止盗链等

1. 开启apache服务器的`rewrite`功能。找到`httpd.conf`文件的151行。去掉`LoadModule rewrite_module modules/mod_rewrite.so`的注释
2. 将`.htaccess`文件放到根目录即可。

```javascript
<IfModule mod_rewrite.c>
  Options +FollowSymlinks -Multiviews
  RewriteEngine On

  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  # 无论地址栏输入了什么东西，最终都是访问到了index.php页面。
  RewriteRule ^(.*)$ index.php/$1 [QSA,PT,L]
</IfModule>
```

### 优化index.php页面

+ bxg.com          对应views/index/index.html
+ bxg.com/login    对应views/index/login.html
+ bxg.com/teacher/add  对应views/teacher/add.html

```php
//获取路径信息
if (array_key_exists("PATH_INFO", $_SERVER)) {
    $path = $_SERVER['PATH_INFO'];
    //去除第一个/
    $path = substr($path, 1);
    //对路径信息进行切割
    $arr = explode("/", $path);

    //如果路径信息，有两个值，那么就对应2级的目录
    if (count($arr) == 2) {
        include '/views/' . $arr[0] . "/" . $arr[1] . ".html";
    }
    //如果路径信息只有一个，那么默认跳转到index目录
    if (count($arr) == 1) {
        include "/views/index/" . $arr[1] . ".html";
    }
} else {
    //如果没有路径信息，那么直接对应/views/index/index.html
    include '/views/index/index.html';
}
```

**提交git，推送到github** 



## 跨域解决方案

### 同源策略



### 跨域



### 解决方案

+ jsonp
+ cors
+ 反向代理

### 反向代理

反向代理（Reverse Proxy）方式是指以[代理服务器](https://baike.baidu.com/item/%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8)来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。



反向代理是一种跨域的方案，这种方案是通过后台配置进行实现的，配置后不需要前端做任何的使用。

+ 服务端开发完成的接口已经部署到公网上，并没有和前端页面部署到同一台服务器。
+ 接口域名和前端页面不在同一个域名下，所以前端使用ajax请求访问时，浏览器会报跨域错误。



反向代理的配置：

+ 打开`httpd-conf`文件,去掉135行和143行的注释

```bash
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
```

+ 修改 `D:\phpStudy\Apache\conf\extra\httpd-vhosts.conf` 文件，找到bxg对应的配置，添加以下代码

```javascript
# 关闭正向代理
ProxyRequests off
# 开启反向代理，当服务器碰到/api的请求之后，会帮我们替换成http://api.botue.com/v8，并且发送请求获取数据。
ProxyPass /api http://api.botue.com/v8
```



反向代理的原理

![](images/proxy.png)




# HTTP状态保持

## HTTP协议是无状态的

[HTTP无状态协议](https://baike.baidu.com/item/HTTP%E6%97%A0%E7%8A%B6%E6%80%81%E5%8D%8F%E8%AE%AE/5808645?fr=aladdin)，是指协议对于事务处理没有记忆能力。当我们给服务器发送请求，服务器根据请求响应对应的数据，但是服务器不会记录任何的信息。

对于一个浏览器，发出的多次请求，web服务器无法区分是不是来源于同一个服务器。

服务器不会记录客户端的信息，每次请求的状态仅限于当次的请求，下次请求时，服务器会把浏览器当成一个新的会话进行处理。

```javascript
- 公交车司机不会记住每天上车的乘客。
- 自动售货机不会记录每次买东西的客户。
- 服务器不会记住每次浏览器请求的状态。
```

好处：服务器不需要先前的信息，应答会比较快

缺点：如果后续的请求需要前面的信息，会导致每次传送的数据量变大。

```javascript
http协议是无状态的，那么当我们登录之后，服务器无法记录我们是否登录过，难道每次操作都需要用户重新登录么？应该怎么解决这个问题？
```

## 状态保持

HTTP无状态的特性有时候严重的阻碍了某些功能的实现，因为交互是需要承前启后的。

+ 一个网站需要登录，但是在你登录过后，因为是无状态的，导致下个请求，服务端还是无法识别你的身份。
+ 一个简单的购物车功能，在你选好了商品，跳转到结算页面时，浏览器无法确定用户之前选择了什么商品。

**HTTP是无状态的，因此两种用于保持HTTP状态的技术就应运而生了，一个是Cookie，一个是Session，我们可以通过Cookie和Session存储一些额外的信息，从而达到保持HTTP状态的目的。**

喝咖啡的例子：

```javascript
一家咖啡厅有一个长期的活动，只要喝满5杯咖啡，就可以赠送一杯咖啡，当然一次性消费5杯咖啡基本不可能。但是咖啡的消费数量可以累计。但是服务员又不可能每次记下来所有的客人喝了多少的咖啡。应该什么时候赠送免费的咖啡。应该如何解决这个问题？
1. 咖啡厅给客户一张卡片，卡片都会有一个唯一的卡号，一般还有一个有效的期限。（客户保存卡号信息：Cookie）
2. 咖啡厅用一个本子，记录下来每张卡片的消费数量和消费信息。（咖啡厅保存消费信息：Session）
3. 每次消费时，客户只需要出示卡片，咖啡厅根据卡号就能查询出客户的消费信息，并且修改这些消费信息。（Cookie+Seesion实现状态的保持）
4. 这样就能够实现咖啡厅记录每一个客户的消费状态了。
```

## Cookie

### cookie概念

Cookie是客户端保持状态的解决方案。Cookie存储了服务端发送给客户端的一些特殊信息，这些信息以文本的方式存储在客户端。客户端每次向服务器发送请求时都会带上这些特殊的信息。

+ Cookie是记录在客户端的一小段文本信息，伴随这用户请求，会在浏览器和服务器之间传递。
+ Cookie是以域名为单位存储的，每个域名之间的Cookie之间是相互隔离的，也就是说不同域名之间的Cookie是不可以相互操作的（同源策略）
+ 必须通过http协议访问页面，才能访问到cookie中的内容。
+ cookie有大小限制，不会超过4kb，不要存储大量数据。
+ 一个域名下的cookie是在该域名下所有的页面都可以访问的。子路径中可以访问父路径中存储的cookie，但是父路径中无法访问子路径中的cookie，一半情况下，我们会直接把cookie存储到域名的根目录下，这样，该域名下所有的页面都可以访问了。

### js操作cookie

cookie其实就是一个document的一个属性，cookie是一个字符串，这个字符串有特定的格式。
cookie的格式： key=value;key=value;key=value;

+ 设置cookie

```javascript
//设置cookie，设置多个cookie，不是覆盖，而是追加操作
//设置的cookie默认是会话级别的，即浏览器关闭就失效
document.cookie = "name=zs";
document.cookie = "age=18";

//设置过期时间 
// max-age:1  单位秒  7 * 60 * 60 * 24
document.cookie = "desc=hehe;max-age=60";
// expires: 指定过期的具体日期（不推荐）
document.cookie = "desc=hehe;expires="+new Date("2017-08-04 23:59:59");

//设置访问路径,通常都会设置成/，因为这样才能保证该网站下所有的页面都能获取到cookie。
//cookie的访问规则：子路径中可以访问父路径中存储的cookie，但是父路径中无法访问子路径中的cookie
document.cookie = "desc=hehe;max-age=60;path=/";
```

+ 获取cookie

```javascript
//获取到的cookie格式为：desc=hehe; name=zs; age=18
//如果想要获取指定的cookie，需要对字符串进行切割，太麻烦
consoloe.log(document.cookie);
```

+ 删除cookie

```javascript
//cookie无法覆盖，因此无法直接删除，通过我们删除一个cookie的操作就是设置max-age:-1即可。
 document.cookie = "age=18;max-age=-1;path=/";
```

总结：使用原生js操作cookie太过于麻烦。通常我们都会使用jquery.cookie插件操作cookie

### jquery操作cookie

jquery.cookie.js插件是一个专门用于操作cookie的一个插件，使用起来非常的方便和简单。

[官方网站](https://plugins.jquery.com/cookie/)

[github地址](https://plugins.jquery.com/cookie/)

+ 设置cookie

```javascript
//设置一个会话级别的cookie，浏览器关闭就消失
$.cookie('name', 'value');

//设置一个7天有效期的cookie
//expires也可以指定一个具体的过期时间
$.cookie('name', 'value', { expires: 7 });

//设置cookie的有效期和路径
$.cookie('name', 'value', { expires: 7, path: '/' });
```

+ 获取cookie

```javascript
//如果存在，返回对应的值
$.cookie('name'); // => "value"
//如果不存在，返回undefined
$.cookie('nothing'); // => undefined

//获取所有的额cookie
$.cookie(); // => { "name": "value" }
```

+ 移除cookie

```javascript
// 移除cookie，如果成功，返回true，否则false
$.removeCookie('name'); // => true
$.removeCookie('nothing'); // => false

// 如果路径不同时，需要指定路径
$.cookie('name', 'value', { path: '/' });
// 移除失败
$.removeCookie('name'); // => false
// 移除成功
$.removeCookie('name', { path: '/' }); // => true
```



思考：cookie的用途是什么？ 实现不同页面之间的数据共享

### php操作cookie（了解）

```php
//每次请求，浏览器都会讲cookie发送给服务器。
//$_COOKIE是一个伪数组，里面存放了所有的cookie信息。
print_r($_COOKIE);

//设置cookie,
//实质：响应头中添加了set-cookie, 最终还是浏览器添加的这个cookie
setcookie("age", "18");

```

php对于cookie的具体操作，我们不需要深究，我们只需要知道一点，服务器可以操作cookie，实质是通过响应头，让浏览器操作cookie。



## Session

与cookie相对的一个技术是session，它是通过服务器来保持状态的。session这个单词包含的意思有很多。

+ Session指的是服务器端为客户端所开辟的存储空间，在其中保存的信息就是用于保持状态
+ 我们需要关注如何往session中存放东西，如果从session中获取东西。
+ session会为客户端的请求开辟一个小空间，用于存储信息，每一个空间都会有一个唯一的id




### session的操作

+ 开启session

```php
//1. 开启session
session_start();

//2. 设置内容
$_SESSION["name"] = "zhangsan";
$_SESSION["age"] = 18;

//3. 获取session
echo $_SESSION["name"];

//4. 同一个服务器中，session也是可以共享的
```

问题：http是无状态的，为什么session也可以进行数据共享？

### cookie和session实现状态保持

+ 第一次访问服务器的时候，`session_start()`函数会生成一个`PHPSESSID`,返回给客户端
+ 客户端会将`PHPSESSID`保存下来，每次请求都会带上cookie
+ 下次请求时，服务器通过`PHPSESSID`就可以确认客户端的身份了。
+ 状态保持，cookie和session缺一不可，不然无法实现。

【画图解释】

【思考：登录功能应该怎么实现】

### session的过期时间（了解）

在`php.ini`配置文件中有一个`gc_maxlifetime:1440`，如果`PHPSESSID`的最后修改时间操作了1440秒，那么这个session就会被认为过期了，那么这个session就会被删除。

简单的说，如果我登录到某网站，如果在1440秒（默认值）内没有操作过，那么对应的session就认为是过期了。我们可以通过修改 `gc_maxlifetime`来延长过期时间。

# 登录功能

思考：

+ 登录功能如何实现？  
+ 首页的头像和用户名信息如何显示？







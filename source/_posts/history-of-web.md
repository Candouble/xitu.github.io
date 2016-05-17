---
title: Web 进化
date: 2016-05-11 12:54:03
tags:
- nodejs
---

## 传统后台架构

 ### 上古时代
![上古时代](http://upload-images.jianshu.io/upload_images/931251-4fbfc489cb120a16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
<!DOCTYPE html>
<html>
<body>

<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDB";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);
// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
} 

$sql = "SELECT id, firstname, lastname FROM MyGuests";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    // output data of each row
    while($row = $result->fetch_assoc()) {
        echo "<div> id: " . $row["id"]. " - Name: " . $row["firstname"]. " " . $row["lastname"]. "</div>";
    }
} else {
    echo "0 results";
}
$conn->close();
?>

</body>
</html>
```
这种模式非常适合创业型小项目，不分前后端，经常 3-5 人搞定所有开发。页面由 JSP、PHP 等工程师在服务端生成，浏览器负责展现。基本上是服务端给什么浏览器就展现什么，展现的控制在Server 层。

### 后端MVC时代
![e2.png](http://upload-images.jianshu.io/upload_images/931251-ef92f22e5bed2be1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
为了降低复杂度，以后端为出发点，有了 Web Server 层的架构升级，比如 thinkPHP 、Laravel、 Spring MVC 等。

### Ajax时代
![e3.png](http://upload-images.jianshu.io/upload_images/931251-a809a12e5002d3b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
XMLHttpRequest异步调用服务器端来获取数据，并将数据应用在客户端，实现了无刷新的效果，这使得Google Maps依赖其极好的用户体验获取了巨大的成功，Ajax这个概念开始火爆，SPA （Single Page Application 单页面应用）时代就开始了。

### 前端 MV* 时代 
![e4.png](http://upload-images.jianshu.io/upload_images/931251-e5a8193bf580852f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
为了降低前端开发复杂度，除了 Backbone，还有大量框架涌现，比如 EmberJS、KnockoutJS、AngularJS、Vue、React等等。

好处很明显：
1、**前后端职责很清晰。**前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难，前端可以本地开发。后端则可以专注于业务逻辑的处理，输出 RESTful 等接口。
2、**前端开发的复杂度可控。**前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本的厚度去说明。
3、**部署相对独立**，产品体验可以快速改进。
但依旧有不足之处：
1、代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。2、全异步，对 SEO 不利。往往还需要服务端做同步渲染的降级方案。
3、性能并非最佳，特别是移动互联网环境下。
4、SPA 不能满足所有需求，依旧存在大量多页面应用。URL Design 需要后端配合，前端无法完全掌控。

## Node全栈&同构
前端为主的 MV* 模式解决了很多很多问题，但如上所述，依旧存在不少不足之处。随着 Node.js 的兴起，JavaScript 开始有能力运行在服务端。

React 同构渲染
![975d4efef933f05fc62a68d27e36e11a_r.png](http://upload-images.jianshu.io/upload_images/931251-8598833fae300cc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Koa+React

有了服务端渲染的基础，后端和前端的路由也可以做到统一。
同构的实现弥补了前面提到的单纯前端实现的SPA的提到前后端代码复用，SEO，URL Design这些问题。

## 引入NodeJS实现前后端分离
 
![51d06ddc3f781892df0a19a053c249bb5d0a4108.jpg](http://upload-images.jianshu.io/upload_images/931251-4066d673a8cb8361.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 为什么要前后端分离？

由于之前的Web基本上都是基于后端的MVC框架，架构决定了前端只能依赖后端。前端写好静态demo，然后根据分工前端或者后端改写成相应的模版语言，比如 blade，smarty等等。这种基于后端环境开发是很痛苦和低效率的，后端没法摆脱对展现的强关注，从而专心于业务逻辑层的开发，前端也吐槽后端乱改他们的代码。所以加上一个nodejs中间层来分离。

- 怎么做前后端分离？

前端：负责视图和交互层。
后端：负责Model层，业务处理/数据等。

![e6.png](http://upload-images.jianshu.io/upload_images/931251-e32efd91b5a24d5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 掘金的架构

-  leancloud

leancloud 是核心功能基于 Clojure 开发的云端数据存储解决方案，不同于传统关系型数据库，提供了标准的 REST API 和在其上封装的各平台的 SDK，数据对象以 JSON 格式随存随取，全平台支持数据增删改查操作。

- 掘金前后端架构

![e5.png](http://upload-images.jianshu.io/upload_images/931251-408832150f2a5ea3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
掘金使用了Vue作为核心的前端框架，部署在leancloud云引擎的Nodejs服务器做为API服务器。Nodejs直接对接以json为数据模型的Leancloud。




参考
[https://github.com/lifesinger/blog/issues/184](https://github.com/lifesinger/blog/issues/184)
[http://ued.taobao.org/blog/2014/04/full-stack-development-with-nodejs/](http://ued.taobao.org/blog/2014/04/full-stack-development-with-nodejs/)
[https://zhuanlan.zhihu.com/p/20669111](https://zhuanlan.zhihu.com/p/20669111)
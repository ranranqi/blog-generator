---
title: Cookie
date: 2018-11-08 16:21:28
tags: http
---
# Cookie是什么？
1，Cookie是浏览器访问服务器后，服务器传给浏览器的一段数据；
2，浏览器保存这段数据，不得轻易删除；
2，此后每次浏览器访问该服务器都必须带上这段数据。

# Cookie怎么实现？
Cookie技术通过在请求和响应报文中写入Cookie信息来控制客户端的状态，Cookie会根据从服务端发送的响应报文内的一个叫做Set-Cookie的首部字段信息，通知客户端保存Cookie。当下次客户端再往服务端发送请求时，客户端会自动在请求报文中加入保存下来的Cookie的值后在发送请求。服务端发现客户端发送过来的请求中有Cookie后，会检查时哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的一个状态信息。
## 实现步骤
1，在没有Cookie值时客户端（浏览器）发送请求--->服务器生成Cookie并记录是谁发送的--->然后服务器在响应报文中添加Cookie后返回--->客户端保存Cookie。
2，第二次发送请求时存下Cooke的客户端在请求中添加Cookie后发送--->服务器检查Cookie（）--->响应返回之前状态的信息给客户端。
步骤1没有Cookie时发送请求报文，例：
```
GET /xxx HTTP/1.1
Host: sp0.baidu.com
...
*请求的首部字段没有Cookie相关信息
```
响应报文：
```
HTTP/1.1 200 ok
Content-Length: 0
Content-Type: image/gif
<Set-Cookie: BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; max-age=86400; domain=.baidu.com; path=/>
```
步骤2客户端存下Cookie后再次发送的请求报文：
```
GET /xxx HTTP/1.1
Host: sp0.baidu.com
...
Cookie: BDORZ=B490B5EBF6F3CD402E515D22BCDA1598
```
# Cookie的作用
1，识别用户身份；
比如A用户访问a.com，那么a.com服务器就会返回一段数据给A(uid=1这就是Cookie)，A再次访问a.com会带上(uid=1);
比如B用户访问a.com，那么a.com服务器就会返回一段数据给A(uid=2这就是Cookie)，B再次访问a.com会带上(uid=2);
那么，a.com的服务器就能区分用户A和B了。
2，记录用户的行为。
假设一个购物车网站b,用户B[uID=2]，此时他把B1,B2商品加入购物车，[uID=2，CART=B1,B2]，过去了几天，B再次打开b网站，发现商品仍在购物车中，因为浏览器不会轻易删除cookie。

例子来源于知乎方应杭老师，感谢分享。



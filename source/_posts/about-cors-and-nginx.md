---
title: 有关跨域 CORS 及 Nginx 配置
author: ijse
clearReading: true
autoThumbnailImage: false
thumbnailImagePosition: left
metaAlignment: left
coverSize: partial
coverMeta: in
comments: true
meta: true
actions: true
date: 2018-02-06 14:28:58
tags: [ cors, nginx, http ]
keywords:
  - cors
  - nginx
  - http
thumbnailImage: false #//tigerbrokers.github.io/assets/images/mike-bryant-75071.jpg
coverImage: /assets/images/mike-bryant-75071.jpg
coverCaption:
gallery:
---
跨域请求针对浏览器的同源策略(Same-Origin Policy)而言，指一个网站主动请求另外一个网站的资源(图片、javascript、视频等)。

同源策略要求网站只能有限制的访问外部网站的资源，不合法的请求会被拦截。网站的源由协议、域名、端口三部分组成，有一部分不同就被视为不同源，两个不同的域名即便指向同一个ip地址也是跨域的。网站通过AJAX请求资源是典型的跨域请求，需要外部网站许可才能访问。
<!-- excerpt -->

# 什么是跨域？
跨域请求针对浏览器的同源策略(Same-Origin Policy)而言，指一个网站主动请求另外一个网站的资源(图片、javascript、视频等)。

同源策略要求网站只能有限制的访问外部网站的资源，不合法的请求会被拦截。网站的源由协议、域名、端口三部分组成，有一部分不同就被视为不同源，两个不同的域名即便指向同一个ip地址也是跨域的。网站通过AJAX请求资源是典型的跨域请求，需要外部网站许可才能访问。

同源策略是浏览器端的安全限制。

<!-- more -->

![](/assets/images/15178986212867.jpg)
> “Domains, protocols and ports must match”

# 跨域对前端的影响

* AJAX：请求无法发送和接收
* WebFonts：字体文件无法从别的域加载
* SVG：通过use引用的外部资源无法使用
* Canvas/WebGL：如无法读取图片的点阵数据，无法toDateURL等
* Video：无法截取快照


通常浏览器控制台会明确打印出跨域错误及其帮助信息，如：

![](/assets/images/15178986972043.jpg)


# CORS的浏览器支持
![](/assets/images/15178987236463.jpg)

基本主流浏览器都已经支持。

# 跨域请求过程
![](/assets/images/15178987563714.jpg)

1. 首先查看http头部有无origin字段；
2. 如果没有，或者不允许，直接当成普通请求处理，结束；
3. 如果有并且是允许的，那么再看是否是preflight(method=OPTIONS)；
4. 如果是preflight，就返回Allow-Headers、Allow-Methods等，内容为空；
5. 如果不是preflight，就返回Allow-Origin、Allow-Credentials等，并返回正常内容。


## 简单请求(Simple Request)
GET, HEAD, POST 方法，并且POST方法的Content-Type只包含 text/plain, application/x-www-form-urlencoded, multipart/form-data

## 预请求(Preflighted Request)
在简单请求之外的其它请求发出前，都会先发一个OPTION请求，以探测服务器是否支持及允许CORS请求的配置参数。

> 我们的大多数AJAX请求包含Authentication头，因此这些都不会是简单请求，所有请求都需要先发一个预请求。


# 跨域相关HTTP Headers
## 相关Headers介绍

![](/assets/images/15179074088703.jpg)

* 请求头都是由浏览器自动生成的，根据请求的不是当前域进行判断。
* 服务器端需要支持OPTION请求，并返回204
* 其它请求服务器端需要添加http headers即可，所有验证逻辑均在浏览器端完成。

## 支持Cookies的跨域
若接口（比如http://api.com/ping）需要在自己域（http://api.com）下读写Cookies, 在没有 `Access-Control-Allow-Credentials: true` 的请求中，Cookies是不支持的，即 接口不能够读写域下的Cookies 。

因此要支持Cookies的跨域，需要服务器端返回如下头：
```
// OPTION Request
OPTIONS /api/v1/captcha?deviceId=web20180110_71007&platform=desktop-web&env=Chrome&vendor=web&lang=&appVer=4.0.0 HTTP/1.1
Host: test-oauth.itiger.com
Access-Control-Request-Method: GET
Origin: https://test-web.itiger.com
Access-Control-Request-Headers: authorization
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7


// OPTION Response
HTTP/1.1 204 No Content
Date: Wed, 10 Jan 2018 09:13:22 GMT
Content-Type: text/plain charset=UTF-8
Connection: keep-alive
Server: nginx/1.9.6
Access-Control-Allow-Origin: https://test-web.itiger.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: POST, GET, OPTIONS, HEAD, DELETE, PUT
Access-Control-Allow-Headers: DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization


// GET Request
GET /api/v1/captcha?deviceId=web20180110_71007&platform=desktop-web&env=Chrome&vendor=web&lang=&appVer=4.0.0 HTTP/1.1
Host: test-oauth.itiger.com
Connection: keep-alive

Accept: application/json, text/plain, */*
Origin: https://test-web.itiger.com
Authorization: Bearer gMnH2uCUzsoncdiC90XJaYDpoYgxDYiBMydIUGfC9NDkfSiiD0



// GET Response
HTTP/1.1 200 OK
Date: Wed, 10 Jan 2018 09:16:19 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Server: TornadoServer/3.2.2
Etag: W/"d8b94d7da2b3d9cec8a195d024dea7147a470d4b"
Pragma: no-store
Cache-Control: no-store
Set-Cookie: captcha="2|1:0|10:1515575779|7:captcha|88:NDdkNmYzYjYwMzQ5N2ViNTU0YjQ0YmEyNTY3MDVjZmVkMWNmZTc1ODRmNzNiNGNjN2M1Yzc5Mzg4MjE0YTg3Yg==|7fe5bc2d6d748af5fd3442a09ad07f8d819db249e0c13d1047e1411611e25878"; expires=Fri, 09 Feb 2018 09:16:19 GMT; Path=/
Access-Control-Allow-Origin: https://test-web.itiger.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: POST, GET, OPTIONS, HEAD, DELETE, PUT
Access-Control-Allow-Headers: Referer,Accept,Origin,User-Agent,Authorization,NT,X-CustomHeader,Keep-Alive,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Connection,DNT
Content-Encoding: gzip
```

除此之外，前端在进行请求时需要特别标注：

```js
// Vanilla
let xhr = new XMLHttpRequest()
xhr.withCredentials = true


// JQuery
$.ajax({
    type: 'POST',
    data: {},
    dataType: 'json',
    xhrFields: {
       withCredentials: true
    }
})


// Vue-resource
const result = await this.$http.get(url, {
  credentials: true
})
```


# 调试CORS请求问题
主要有两种方法：

## 简单判定是否CORS问题，
1. 查看控制台出错日志信息
2. 下载 [CORS 调试](https://chrome.google.com/webstore/detail/nlfbmbojpeacfghkpbjhddihlkkiljbi)  浏览器插件，开启后重新请求

![](/assets/images/15179075649303.jpg)


> 注意：
> 开启并用默认配置(*://*)，可能会导致某些网站访问异常，如GitHub。建议自行添加过滤。
> 此种方法也适用于开发时调用暂未配置跨域的接口。

## 用Charles修改HTTP请求调试

# Nginx 配置参考

```nginx
map $http_origin $allow_origin {
  default "";
  "~^https?://(?:[^/]*\.)?(stevebuzonas\.(?:com|local))(?::[0-9]+)?$" "$http_origin";
}

map $request_method $cors_method {
  default "allowed";
  "OPTIONS" "preflight";
}

map $cors_method $cors_max_age {
  default "";
  "preflight" 1728000;
}

map $cors_method $cors_allow_methods {
  default "";
  "preflight" "GET, POST, OPTIONS";
}

map $cors_method $cors_allow_headers {
  default "";
  "preflight" "Authorization,Content-Type,Accept,Origin,User-Agent,DNT,Cache-Control,X-Mx-ReqToken,Keep-Alive,X-Requested-With,If-Modified-Since,Authorization";
}

map $cors_method $cors_content_length {
  default $initial_content_length;
  "preflight" 0;
}

map $cors_method $cors_content_type {
  default $initial_content_type;
  "preflight" "text/plain charset=UTF-8";
}

add_header Access-Control-Allow-Origin $allow_origin always;
add_header Access-Control-Allow-Credentials 'true' always;
add_header Access-Control-Max-Age $cors_max_age always;
add_header Access-Control-Allow-Methods $cors_allow_methods always;
add_header Access-Control-Allow-Headers $cors_allow_headers always;

set $initial_content_length $sent_http_content_length;
add_header 'Content-Length' "";
add_header 'Content-Length' $cors_content_length;

set $initial_content_type $sent_http_content_type;
add_header Content-Type "";
add_header Content-Type $cors_content_type;

if ($request_method = 'OPTIONS') {
  return 204;
}
```

注意：
1. 相关Access-Control的add_header后加了`always`，因为默认情况下，`add_header`只作用于状态码为 200, 201 (1.3.10), 204, 206, 301, 302, 303, 304, 307 (1.1.16, 1.0.13), or 308 (1.13.0) 之中的响应，并不支持40x,50x等，因此添加`always`可以支持所有状态码。 见：http://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header
2. 之所以用`map`命令而不是`if`，是由于if其实是归属于nginx_rewrite模块的，对于`try_files`等会不支持，Nginx官方也并不建议使用`if`。见：https://www.nginx.com/resources/wiki/start/topics/depth/ifisevil/
3. 以上`nginx-cors.conf`可如下方式使用：
```nginx
location / {
  include ./nginx-cors.conf;


  proxy_pass http://127.0.0.1:4000;
  proxy_set_http_header HOST $http_host;
  ...
}
```

# 参考资源
* https://www.w3.org/TR/cors/
* https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
* https://www.test-cors.org/
* https://enable-cors.org/index.html
* https://www.html5rocks.com/en/tutorials/cors/
# 扩展知识
* HTTPS
* Nginx
* CSP
* HTTP/1.1 vs HTTP2



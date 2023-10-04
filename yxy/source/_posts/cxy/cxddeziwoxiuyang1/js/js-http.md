---
title: '【第9章】http与restfulAPI'
date: 2019-04-12 15:33:35
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

const { get } = require("http");

## 状态码类型
  1xx 服务器收到了请求
  2xx请求成功 如200
  3xx重定向 如302
  4xx客户端错误
  5xx服务端错误，如500

## 常见状态码：
**200 请求成功**
**301 永久重定向**
（配合location，浏览器记住了该网址返回了301，然后浏览器之后会永久自动处理到新的网站，如域名到期换新域名的情况）
**302 临时重定向**
（配合location，浏览器记住了该网址返回了302，但下次访问浏览器还是会访问到旧地址，如百度首页搜索的二级跳转、短网址等）
点击链接：[为什么百度需要二级跳转重定向？](https://www.bilibili.com/read/cv6711239/)简单说，是为了打点。
点击链接：[短网址的作用](https://www.zhihu.com/question/20790447) 简单说，微博字数那么长，再来个那么长的网址真心hold不住。
**304 资源未被修改**（缓存了）请求过来的数据没有改变
**404 资源未找到**
**401 用户未授权** 比如密码错误、用户名错误
**403 用户得到授权，但访问时禁止的**
**500 服务器错误**
**504 网关超时** (服务器端的操作有超时)

## 新method可以写的状态码
/*
    200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
    201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
    202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
    204 NO CONTENT - [DELETE]：用户删除数据成功。
    400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
    401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
    403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
    404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
    406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
    410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
    422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
    500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
*/
// “幂等性”指的是 1 次或 N 次的副作用(对系统造成的影响)相同，而不是返回结果相同，因此 GET 请求总是幂等的。
// 无论一个操作被执行一次还是多次，执行后的效果都相同。比如对某资源发送GET请求，如果访问一次和访问十次获得的数据一样，那么就说这个请求具有幂等性。

## http methods
#### 1. 传统methods
get获取服务器数据
post向服务器提交数据

#### 2. 现在的methods
点击链接：[restful api 的设计方法](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
（1）get获取服务器数据 ()
（2）head 和get类似，但服务器在响应中只返回头部，不返回实体的主体部分，这就允许客户端在没获取资源时对资源首部进行检查
（3）post新建数据 (新建blog)
（4）patch/put更新数据 （更新blog）
patch:局部更新资源，比如只更新某一字段 put：完整更新资源，对带宽会有影响  https://blog.csdn.net/varyall/article/details/80895945
（5） delete删除数据 （删除blog）
（6）options 获取信息，关于资源的哪些属性是客户端可以修改的的。

## restfulApi 
**restfulApi 是一种新的api设计方法。**
#### 1. 概念：
RESTful API 就是REST风格的API。现在终端平台多样，移动、平板、PC等许多媒介向服务端发送请求后，如果不适用RESTful API，需要为每个平台的数据请求定义相应的返回格式，以适应前端显示。但是RESTful API 要求前端以一种预定义的语法格式发送请求，那么服务端就只需要定义一个统一的响应接口，不必像之前那样解析各色各式的请求。
#### 2. restfulapi的设计
（1）传统API 把每个url当做一个功能
（2）restful api 把每个url当做一个唯一的资源\(^o^)/~
就比如获取博客和更新博客 /api/blog/create 创建博客 /api/blog/update?blog=100 更新博客 这是一种功能
// 而新的restfulapi，获取博客、更新博客、创建博客，都可以用/api/blog去表示，唯一不同的是method，这是一种资源集合
（3）在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

## 附：
URI：Uniform Resource Identifier，统一资源标识符
URL：Uniform Resource Location统一资源定位符 URL是URI的子集。
点击链接：[URI 和 URL](https://www.zhihu.com/question/21950864)


#### 3. 如何把每个url设计成一个资源？
###### （1）不使用url参数 ，即问号后面的部分，然后做一个url的唯一标识
传统： /api/list?pageIndex=2
restful api: /api/list/2

###### （2）使用method表示操作类型
**传统：**
```
post: /api/blog/create 创建
post: /api/blog/update?blog=100 更新
get:  /api/blog/get?blog=100 获取
```
**restful api：**
```
post: /api/blog method:post
patch: /api/blog/100 method:patch/put
delete: /api/blog/100 method:delete
```
下面以动物园为例子：
```
    GET /zoos：列出所有动物园  /api/zoos get
    POST /zoos：新建一个动物园 /api/zoos post
    GET /zoos/ID：获取某个指定动物园的信息 /api/zoos/ID get
    PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息） /api/zoos/ID put
    PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息） /api/zoos/ID patch
    DELETE /zoos/ID：删除某个动物园 /api/zoos/ID patch delete
    GET /zoos/ID/animals：列出某个指定动物园的所有动物 /api/zoos/ID/animals get
    DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物 /api/zoos/ID/animals/ID delete
```
## http headers
#### 1. 常见的request headers（客户端像服务端发送的）

Accept: image/avif,image/webp,image/apng,text/html
浏览器可接受的数据格式

Accept-Encoding: gzip, deflate, br
浏览器可接受的压缩算法

Accept-Language: zh-CN,zh;q=0.9
浏览器可接受的语言

Connection: keep-alive   
连接方式 
keep-alive：一次tcp连接可以重复使用

Cookie: BIDUPSID=8B5F551B8E92D363D2BF09A17F1787F0; BAIDUID=5ccc19b28fd87ed0af236b203e83021a
什么是cookie：
Cookie，有时也用其复数形式 Cookies。类型为“小型文本文件”，是某些网站为了辨别用户身份，进行Session跟踪而储存在用户本地终端上的数据（通常经过加密），由用户客户端计算机暂时或永久保存的信息 [1]  。

Host: m.baidu.com
域名

Content-type: application/json text-plain
客户端像服务端请求时，告诉服务端我发送数据的格式，一般只有post请求有，get是获取数据一般没有

User-Agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.193 Mobile Safari/537.36
浏览器信息

Cache-Control



#### 2. 常见的response headers（服务端像客户端返回的）
连接方式      Connection: keep-alive
返回数据的大小 Content-Length: 0
返回的数据的格式 Content-Type: text/plain; charset=utf-8
返回的数据的压缩算法 Content-Encoding
服务端改cookie的时候，需要发送 Set-Cookie: __bsi=10004501327375333328_00_32_N_R_10_0303_c02f_Y; max-age=3600; domain=m.baidu.com; path=/

#### 3. 缓存相关的headers
cache-control （响应头，缓存时间）
expires (响应头中，已被cache-control代替，有这个属性和cache-control一起兼容写法的情况)
last-modified（响应头，资源的最后修改时间）
 if-modified-since （请求头，协商缓存时浏览器再次请求会带着这个值，这个值和响应头的laast-modified相同）
etag （资源的唯一标识）
if-none-match (请求头，值和响应头的etag相同，协商缓存中的唯一标识)

**header也可以自定义,axios插件就可以自定义header**
除了以上之外，可以自定义header，如果在客户端自定义在request中加，如果在服务端中自定义要后端在response中加
**常用场景：**如服务端需要前端加上一个验证header做非法请求的验证

## 总结一下restful api的设计模式
1.协议遵守https协议
2.api应该有单独域名
3.加上版本号
4.由于restfulapi中url表示一种资源的集合，而不再是一个功能一个url，所以网址中不能有动词，应该全是名词，且与数据库的表名一一对应，也由于是一种集合，所以api的名词尽量为复数
5.用http动词去表示资源的具体操作类型，如putpatchpostgetdelete增删改查等
6.如果涉及分页这种返回结果相当多的情况，api也可以提供一些参数，如?limit=10&page=2
7.有单独的状态码
8.4xx错误时要返回出错信息，用统一的error作为键值
9.返回结果应符合规范
    GET /collection：返回资源对象的列表（数组）
    GET /collection/resource：返回单个资源对象
    POST /collection：返回新生成的资源对象
    PUT /collection/resource：返回完整的资源对象
    PATCH /collection/resource：返回完整的资源对象
    DELETE /collection/resource：返回一个空文档
10.Hypermedia API 返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。


## http缓存(见有道云)
#### 1. 什么是缓存？
网页第二次访问时，有相同资源可以不用重新请求一遍。
#### 2. 为什么需要缓存
为了让页面加载更快，网络请求加载相比cpu计算等等会比较慢，所以要尽量减少网络请求的数量和体积，可以让加载更快一些
#### 3. 哪些资源可以被缓存 —— 静态资源（js css img）
html一般是不被缓存的，网页的业务数据（比如留言板列表、博客列表）一般也不能被缓存，因为有可能随时会更新。

#### 2. http缓存策略(见有道云)

###### （1）强制缓存 cache-control
比如请求一个js资源
浏览器初次请求服务器，服务器返回资源和cache-control，若服务端感觉这个资源可以被缓存，则加入cache-control（在response-headers中，因为在响应头中，所以客户端无法控制，服务端控制）
如：cache-control：max-age=5184000 单位为秒
浏览器再次请求时，之前服务端的cache-control本地缓存会缓存下js的内容，如果cache-control没过期，浏览器就请求本地缓存，不通过网络，由本地缓存返回资源。

###### （2）协商缓存
服务端去判断客户端资源是否和服务端一致，一致则返回304（服务端判断客户端资源未改动），否则返回200和最新的资源。
浏览器初次请求，服务端返回资源和资源标识给浏览器。浏览器再次请求会带上资源标识，服务端判断该资源标识的资源是否和服务端最新资源一致，则返回304，否则返回新资源和新资源标识。
资源标识：在response-header中（因为是服务端返回的呗）
last-Modified 资源的最后修改时间
Etag资源的唯一标识（一个字符串，类似人类指纹）

###### （3）刷新方式对缓存的影响

## https协议(见有道云)

## 一、 http面试题
1. http常见状态码 
2. http常见header 
3. 什么是restful 
4. 描述一下http缓存机制

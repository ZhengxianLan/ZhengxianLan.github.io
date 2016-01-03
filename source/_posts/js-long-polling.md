---
title: js-long-polling
layout: post
date: 2016-01-03 10:32:14
categories: ['js']
tags: ['polling']
---

ref： [简单轮巡实例](http://techoctave.com/c7/posts/60-simple-long-polling-example-with-javascript-and-jquery/)

要想从服务器获取数据，有几种方式。
### 传统的方式
1. f5
2. 定时刷新
- 使用 setInterval
```javascript
setInterval(function(){
    $.ajax({ url: "server", success: function(data){
        //Update your dashboard gauge
        salesGauge.setValue(data.value);
        }, dataType: "json"});
    }, 30000);
```
优点： 简单易懂，并且是异步的哦。
缺点： 如果处理时间大于30s，server端还是没返回怎么办？一个忙不过来的服务器+坑爹的网络都有可能造成请求不能及时返回结果。这样，就可能产生大量无用的请求队列。
- setTimeout
```javascript
(function poll(){
 setTimeout(function(){
     $.ajax({ url: "server", success: function(data){
         //Update your dashboard gauge
         salesGauge.setValue(data.value);

         //Setup the next poll recursively
         poll();
         }, dataType: "json"});
     }, 30000);
 })();
```
通过首次的自执行，来启动获取数据的setTimeout函数，超时时间仍然为30s
不同的是，我们只在上次请求返回之后才会立即执行下一次的获取数据操作。但是，以上2种方法都有同样的问题，那就是每次通过 $.ajax 向服务器请求数据时，都会打开一个新的链接。每一次的打开关闭都会消耗资源和时间。
所以，有人就想，如果我保持这个连接不断开会怎么样?

### long polling - An efficient Server-Push Technique
为了提供一个实时交互体验的应用，我们希望能在服务器和客户端之间实现一个长连接。
但是，当前仅仅依靠客户端的js还不能完成这样的功能。
我们需要的是融合了HTML5规范的 WebSorckets 和一些服务端的组件。

要实现长轮巡，仅靠js目前还没有办法。一般都需要服务端提供相应的功能，比如 SingalR for .Net, Socket.IO for Node.JS

如果还是想仅仅通过js来实现轮巡效果，那么可以试试
```javascript
(function poll() {
 setTimeout(function() {
     $.ajax({ url: "server", success: function(data) {
         sales.setValue(data.value);
         }, dataType: "json", complete: poll });
     }, 30000);
 })();
```
如果能保持连接不自动关闭，那么能明显提供服务相应速度，而获得更好的用户体验。

### HTML5 WebSockets
这些奇淫异巧的产生，代表了一种强烈的需求，也为HTML5 WebSockets 打下了一定的基础。

而Socket.IO 对于 WebSockets ，就像 jQuery 对于 javascript。

Socket.IO 希冀通过提供统一的调用接口来屏蔽不同厂商实现 WebSockets 的差异，这样就达到了简化开发的目的。

比如，这样使用 Socket.IO
```javascript
<script src="/socket.io/socket.io.js"></script>
<script>
var socket = io.connect("http://localhost");
socket.on('sales', function (data) {
    //Update your dashboard gauge
    salesGauge.setValue(data.value);

    socket.emit('profit', { my: 'data' });
    });
</script>
```

如果浏览器支持 WebSockets,那么就使用
否则，会使用 Adobe Flash Socket 或者 Ajax polling

在浏览器全面支持 WebSockets 前， jQuery Long Polling 不失为实时交互应用的一个不错的解决方案。

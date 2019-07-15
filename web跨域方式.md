# 跨域方式

同源策略：协议，域名，端口三者中有一个不同就算跨域。

跨域方式：

1）JSONP，通过<script>标签的异步加载来实现的跨域；

2）CORS “跨域资源共享”（Cross-origin resource sharing），它允许浏览器向跨源服务器，发出XMLHttpRequest请求。分为简单请求和非简单请求；（浏览器自动完成）

3）WebSocket（网络通信协议），不受同源的限制，可在客户端和服务器端之间双向通信；（客户端可以向服务器发送请求，服务器也可以向客户端发送请求，而http只能由客户端向服务器发送请求，不能反过来）

4）postMessage()，H5新增的跨域通信方法。应用场景：窗口 A (http:A.com)向跨域的窗口 B (http:B.com)互通信息；

5）Hash，原理：Hash的改变，页面不会刷新。应用场景：A 通过iframe或frame嵌入了跨域的页面 B，A与B间可以通过hash通信。（A和B的domain相同）；

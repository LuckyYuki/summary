# 状态码

## 301/302/303都表示重定向，所以放在一起讲解

301表示永久重定向（301 moved permanently），表示请求的资源分配了新url，以后应使用新url。

302表示临时性重定向（302 found），请求的资源临时分配了新url，本次请求暂且使用新url。302与301的区别是，302表示临时性重定向，重定向的url还有可能还会改变。

303 表示请求的资源路径发生改变，使用GET方法请求新url。她与302的功能一样，但是明确指出使用GET方法请求新url。

新url指的是，第一次请求返回的location。

- 举例说明(302 303 状态码)

1、浏览器访问http://write.blog.csdn.net, csdn中“我的博客”

2、服务器，返回状态码 302（url临时改变）和location

3、浏览器，请求location指定的地址，完成请求。也就是说，浏览器一共请求了2次！

## 304 not modified

客户端发送附带条件的请求时（if-matched,if-modified-since,if-none-match,if-range,if-unmodified-since任一个）服务器端允许请求访问资源，但因发生请求未满足条件的情况后，直接返回304Modified（服务器端资源未改变，可直接使用客户端未过期的缓存）。304状态码返回时，不包含任何响应的主体部分。304虽然被划分在3xx类别中，但是和重定向没有关系。

## 400 bad request

表示请求的报文中存在语法错误，比如url含有非法字符。

提交json时，如果json格式有问题，接收端接收json，也会出现400 bad request

比如常见的json串，数组不应该有",但是有"了。

## 405 method not allowed

问题原因：  请求的方式（get、post、delete）方法与后台规定的方式不符合。

比如： 后台方法规定的请求方式只接受get，如果用post请求，就会出现 405 method not allowed的提示

## 415

后台程序不支持提交的content-type，就会返回415

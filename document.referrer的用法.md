## document.referrer的用法

document对象有很多属性，其中有3个与对网页的请求有关的属性，它们分别是URL、domain和referrer。

URL属性包含页面完整的URL，domain属性中只包含页面的域名，而**referrer属性中则保存着链接到当前页面的那个页面的URL**。

**referrer属性简单来说就是上一个页面的URL**


```js
var back = document.getElementById('back');   //假设该返回按钮元素id为back
back.onclick = function(){
    history.back();    //返回上一个页面，也可以写成history.go(-1)
};


<a id="back" href="javascript:history.back();"></a>



if(document.referrer){
    back.style.display = 'block';   //默认让其隐藏，当referrer属性不为空时让其显示
}
```

 其实判断当前页面是否是用户一开始打开的页面，方法也不止通过判断referrer属性这一种方法，还可以通过history.length是否为零来判断。

 
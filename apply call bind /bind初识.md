## 在讨论bind()方法之前我们先来看一道题目：

```js
var altwrite = document.write;
altwrite("hello");
```

**结果：Uncaught TypeError: Illegal invocation**

**altwrite()函数改变this的指向global或window对象，导致执行时提示非法调用异常，正确的方案就是使用bind()方法：**

```js
altwrite.bind(document)("hello")
```

当然也可以使用call()方法：

```js
altwrite.call(document, "hello")
```

## 绑定函数

bind()最简单的用法是创建一个函数，使这个函数不论怎么调用都有同样的this值。常见的错误就像上面的例子一样，将方法从对象中拿出来，然后调用，并且希望this指向原来的对象。如果不做特殊处理，一般会丢失原来的对象。使用bind()方法能够很漂亮的解决这个问题：

```js
this.num = 9; 
var mymodule = {
  num: 81,
  getNum: function() { 
    console.log(this.num);
  }
};

mymodule.getNum(); // 81

var getNum = mymodule.getNum;
getNum(); // 9, 因为在这个例子中，"this"指向全局对象

var boundGetNum = getNum.bind(mymodule);
boundGetNum(); // 81
```

## 再看一个例子

```js
var foo = {
    bar : 1,
    eventBind: function(){
        var _this = this;
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(_this.bar);     //1
        });
    }
}


var foo = {
    bar : 1,
    eventBind: function(){
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(this.bar);      //1
        }.bind(this));
    }
}
```


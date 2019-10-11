# apply call

在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。
JavaScript 的一大特点是，函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

```javascript
function fruits() {}
 
fruits.prototype = {
    color: "red",
    say: function() {
        console.log("My color is " + this.color);
    }
}
 
var apple = new fruits;
apple.say();    //My color is red
```

但是如果我们有一个对象banana= {color : "yellow"} ,我们不想对它重新定义 say 方法，那么我们可以通过 call 或 apply 用 apple 的 say 方法：

```javascript
banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow

```

所以，可以看出 call 和 apply 是为了动态改变 this 而出现的，当一个 object 没有某个方法（本栗子中banana没有say方法），但是其他的有（本栗子中apple有say方法），我们可以借助call或apply用其它对象的方法来操作。

## apply call 实例

### 数组之间追加

```javascript
var array1 = [12 , "foo" , {name:"Joe"} , -2458]; 
var array2 = ["Doe" , 555 , 100]; 
Array.prototype.push.apply(array1, array2); 
//array1 值为  [12 , "foo" , {name:"Joe"} , -2458 , "Doe" , 555 , 100] 
```

### 获取数组中的最大值和最小值

```javascript
var  numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
    maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
```

### 验证是否是数组（前提是toString()方法没有被重写过）

```javascript
functionisArray(obj){ 
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```

### 类（伪）数组使用数组方法

```javascript
var domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));
```

## 面试题

### 定义一个 log 方法，让它可以代理 console.log 方法，常见的解决方法是：

```javascript
function log(msg)　{
  console.log(msg);
}
log(1);    //1
log(1,2);    //1
```

上面方法可以解决最基本的需求，但是当传入参数的个数是不确定的时候，上面的方法就失效了，这个时候就可以考虑使用 apply 或者 call，注意这里传入多少个参数是不确定的，所以使用apply是最好的，方法如下：

```js
function log(){
  console.log.apply(console, arguments);
};
log(1);    //1
log(1,2);    //1 2
```

接下来的要求是给每一个 log 消息添加一个"(app)"的前辍，比如：

log("hello world"); //(app)hello world

**该怎么做比较优雅呢?这个时候需要想到arguments参数是个伪数组，通过 Array.prototype.slice.call 转化为标准数组，再使用数组方法unshift，像这样：**

```js
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');
 
  console.log.apply(console, args);
};
```

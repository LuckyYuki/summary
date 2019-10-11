## 和setTimeout一起使用

```js
function Bloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// 1秒后调用declare函数
Bloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 100);
};

Bloomer.prototype.declare = function() {
  console.log('我有 ' + this.petalCount + ' 朵花瓣!');
};

var bloo = new Bloomer();
bloo.bloom(); //我有 5 朵花瓣!
```

注意：对于事件处理函数和setInterval方法也可以使用上面的方法



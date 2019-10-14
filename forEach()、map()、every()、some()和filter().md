## forEach()、map()、every()、some()和filter()

先给个数组：var arr = [1,-2,3,4,-5];

### forEach()，用于遍历数组，无返回值

```js
arr.forEach(function(item,index,array){
    array[index] = item * 2;
});
console.log(arr);   // [2,-4,6,8,-10]
```

> array[index]是全等于item的。

```js
arr.forEach(function(item,index,array){
    console.log(array[index] === item);   // true
});
```

### map()，用于遍历数组，返回处理之后的新数组

```js
var newArr = arr.map(function(item,index,array){
    return item * 2;
});
console.log(newArr);   // [2,-4,6,8,-10]
```

可以看到，该方法与forEach()的功能类似，只不过map()具有返回值，会返回一个新的数组，这样处理数组后也不会影响到原有数组。

### every()，用于判断数组中的每一项元素是否都满足条件，返回一个布尔值

```js
var isEvery = arr.every(function(item,index,array){
    return item > 0;
});
console.log(isEvery);   // false
```

### some()，用于判断数组中的是否存在满足条件的元素，返回一个布尔值

```js
var isSome = arr.some(function(item,index,array){
    return item < 0;
});
console.log(isSome);   // true
```

### filter()，用于筛选数组中满足条件的元素，返回一个筛选后的新数组

```js
var minus = arr.filter(function(item,index,array){
    return item < 0;
});
console.log(minus);   // [-2, -5]
```

可以看到，示例中是要筛选出数组arr中的所有负数，所以该方法最终返回一个筛选后的新数组[-2, -5]。

## 补充

**以上五大方法除了传递一个匿名函数作为参数之外，还可以传第二个参数，该参数用于指定匿名函数内的this指向**

```js
// 只传一个匿名函数
arr.forEach(function(item,index,array){
    console.log(this);  // window
});
```

```js
// 传两个参数
arr.forEach(function(item,index,array){
    console.log(this);  // [1, -2, 3, 4, -5]
},arr);
```

**兼容性： 由于以上方法均属ES5方法，所以IE8及其以下浏览器均不兼容。**

## 总结

- ① forEach()无返回值，map()和filter()返回新数组，every()和some()返回布尔值
- ② 匿名函数中this指向默认为window，可通过传第二参数来更改之
- ③ 五种遍历方法均为ES5方法

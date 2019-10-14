## reduce

语法： 

```js
arr.reduce(function(prev,cur,index,arr){
...
}, init);
```

其中，
arr 表示原数组；
prev 表示上一次调用回调时的返回值，或者初始值 init;
cur 表示当前正在处理的数组元素；
index 表示当前正在处理的数组元素的索引，若提供 init 值，则索引为0；若不提供 init，索引为1；
init 表示初始值。

```js
var arr=[3,4,7];

// 不提供初始值
arr.reduce((prev,cur,index,arr)=>{
console.log('prev',prev);
console.log('cur',cur);
console.log('index',index);
console.log('arr',arr);
console.log('-----------');
});

// prev 3
// cur 4
// index 1
// arr (3) [3, 4, 7]
// -----------
// prev undefined
// cur 7
// index 2
// arr (3) [3, 4, 7]
// -----------

// 提供初始值

arr.reduce((prev,cur,index,arr)=>{
console.log('prev',prev);
console.log('cur',cur);
console.log('index',index);
console.log('arr',arr);
console.log('-----------');
},2);

// prev 2
// cur 3
// index 0
// arr (3) [3, 4, 7]
// -----------
// prev undefined
// cur 4
// index 1
// arr (3) [3, 4, 7]
// -----------
// prev undefined
// cur 7
// index 2
// arr (3) [3, 4, 7]
// -----------
```

## 实例

先提供一个原始数组：var arr = [3,9,4,3,6,0,9];


### 求数组之和

```js
var sum = arr.reduce(function (prev, cur) {
    return prev + cur;
}, 0);
```

### 求数组项最大值

```js
var max = arr.reduce(function (prev, cur) {
    return Math.max(prev,cur);
});
```

### 数组去重

```js
var newArr = arr.reduce(function (prev, cur) {
    prev.indexOf(cur) === -1 && prev.push(cur);
    return prev;
},[]);
```

## 其他相关方法


### reduceRight

**该方法用法与reduce()其实是相同的，只是遍历的顺序相反，它是从数组的最后一项开始，向前遍历到第一项。**

### forEach()、map()、every()、some() 和 filter()

## 总结

reduce() 是数组的归并方法，与forEach()、map()、filter()等迭代方法一样都会对数组每一项进行遍历，但是reduce() 可同时将前面数组项遍历产生的结果与当前遍历项进行运算，这一点是其他迭代方法无法企及的。

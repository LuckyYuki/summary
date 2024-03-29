# 数组排序

JS数组排序方法有两个：reverse()和sort()，其中reverse()可将数组进行倒序，而sort()则可将数组项灵活地进行升序或降序排列。

## reverse

```js
var arr = [8,4,9,1];

console.log(arr.reverse());   //  [1, 9, 4, 8]
console.log(arr);   //  [1, 9, 4, 8]
```

**可以看出，reverse()会直接改变原数组，并且返回值也是倒序后的数组。**

## sort

### 不传参数

```js
var arr = [8,4,9,1];

console.log(arr.sort());   //  [1, 4, 8, 9]
console.log(arr);   //  [1, 4, 8, 9]
```

**可以看出，sort()不传参数时会按升序方式对数组项进行排序，并且与reverse()一样既改变原数组，同时返回的也是排序后的数组。** 其实不是这样的

**事实上，sort()并不是按照数值进行排序，而是按字符串字母的ASCII码值进行比较排序的，所以当数组项为数字时，sort()也会自动先将数字转换成字符串，然后再按字母比较的规则进行排序处理。**

```js
var arr = [8,90,9,16];

console.log(arr.sort());  //  [16, 8, 9, 90]
```

### 传入一个参数

```js
var arr = [8,90,9,16];

// 升序
console.log(arr.sort(function (a, b) {
    return a - b;
}));    //  [8, 9, 16, 90]

// 降序
console.log(arr.sort(function (a, b) {
    return b - a;
}));   //  [90, 16, 9, 8]
```

**这种用法的规则是，当sort()传入函数中的第一个参数a位于第二个参数b之前，则返回一个负数，相等则返回0，a位于b之后则返回正数。**

比如，当要做升序排序时，我们需要想到前面的数肯定是要比后面的数小，所以传入的这个函数参数返回值应该要是个负数，因此函数参数返回a - b。

- ① sort() 不传参时默认为升序，且是按字符串比较的方式排序；传参时，其参数为函数，且该函数带俩参数a 和 b，返回值 a - b 为升序，b - a为降序
- ② reverse() 和 sort() 两函数均会改变原数组，且返回值同样也是改变后的数组

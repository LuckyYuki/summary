# slice()、substring()和substr()

## 共同点

① 接受一个或两个参数，其中第一个参数为裁剪的开始位置
② 都会返回被裁剪下来的子字符串，而原字符串不受影响
③ 若不传第二个参数，则从开始位置（第一个参数）一直截取到字符串结尾。


```js
var str = '微信公众号：前端微站';
var str1 = str.slice(2);  
console.log(str1);    // "公众号：前端微站"
str1 = str.substring(2);
console.log(str1);   // "公众号：前端微站"
str1 = str.substr(2);
console.log(str1);   // "公众号：前端微站"
// 都不会影响到原字符串。
console.log(str);   // "微信公众号：前端微站"
```

## 区别

① slice()和substring()的第二个参数均表示的是裁剪的结束位置（但不包括该项，这与数组中的slice()方法类似），而substr()的第二个参数则表示的是裁剪下来字符串长度

② 当传入的参数为负值时，slice()会将所有负参数与字符串的长度相加，substring()会把所有负参数都转换为0，而substr()就相对比较复杂了，它会将第一个负参数加上字符串长度，第二个参数转换为0

### 参数均为正数

```js
var str = '微信公众号：前端微站';
var str1 = str.slice(2,5);
console.log(str1);    // "公众号"
str1 = str.substring(2,5);
console.log(str1);    // "公众号"
str1 = str.substr(2,3);
console.log(str1);    // "公众号"
```

### 参数存在负数


```js
var str = '微信公众号：前端微站';
console.log(str.length);   // 10
var str1 = str.slice(-4);   //  相当于str.slice(6)
console.log(str1);   //  "前端微站"
str1 = str.substring(-4);   //  相当于str.substring(0)
console.log(str1);   //  "微信公众号：前端微站"
str1 = str.substr(-4);   //  相当于str.substr(6)
console.log(str1);   //  "前端微站"

var str2 = str.slice(2,-4);   //  相当于str.slice(2,6)
console.log(str2);   //  "公众号："
// TODO: 注意这个
str2 = str.substring(2,-4);   //  相当于str.substring(2,0)，也就是str.substring(0,2)
console.log(str2);   //  "微信"
str2 = str.substr(2,-4);   //  相当于str.substr(2,0)
console.log(str2);   //  ""

var str3 = str.slice(-2,-4);   //  相当于str.slice(8,6)
console.log(str3);    // ""
str3 = str.substring(-2,-4);   //  相当于str.substring(0,0)
console.log(str3);   // ""
str3 = str.substr(-2,-4);   //  相当于str.substr(8,0)
console.log(str3);   // ""

var str4 = str.slice(-4,-2);   //  相当于str.slice(6,8)
console.log(str4);   // "前端"
str4 = str.substring(-4,-2);  //  相当于str.substring(0,0)
console.log(str4);  // ""
str4 = str.substr(-4,-2);  //  相当于str.substr(6,0)
console.log(str4);  // ""
```

当参数为负数时，只需牢记，slice见负加总长，substring见负则归零，substr一加总长一归零。 另外还需要特别注意的一点是，slice()第一个参数须小于第二个参数才能正常截取字符串，否则返回的是空字符串，而substring()则没有这个问题。

## 总结

① substr()第二个参数是裁剪长度，只要为负，裁剪结果必定是空字符串
② 不管如何裁剪，均不影响原字符串
③ 当参数为负，slice加总长，substring则归零，substr一加总长一归零。

## 另一资料中记载

> slice() 第一个参数代表开始位置,第二个参数代表结束位置的下一个位置,截取出来的字符串的长度为第二个参数与第一个参数之间的差;若参数值为负数,则将该值加上字符串长度后转为正值;若第一个参数等于大于第二个参数,则返回空字符串.

> substring() 第一个参数代表开始位置,第二个参数代表结束位置的下一个位置;若参数值为负数,则将该值转为0;两个参数中,取较小值作为开始位置,截取出来的字符串的长度为较大值与较小值之间的差.

> substr() 第一个参数代表开始位置,第二个参数代表截取的长度

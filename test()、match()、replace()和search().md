# test()、match()、replace()和search()

## test()，用于检测一个字符串是否匹配某个正则表达式

使用方法：

> RegExpObject.test(string)

RegExpObject代表正则表达式，string代表需要检测的字符串。**该方法返回一个布尔值，true代表匹配成功，false代表失败。**

检测字符串中是否存在英文字符串，这时就可以使用test()来实现：

```js
var str1 = 'Hello，我叫Real';
var str2 = '大家好，我叫张三';

console.log(/\w+/.test(str1));  //true
console.log(/\w+/.test(str2));  //false

```

## match()，可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配

使用方法

> stringObject.match(searchvalue)
> stringObject.match(regexp)

其中stringObject代表需要匹配的字符串，searchvalue代表需要从字符串中检索的内容，regexp代表正则表达式。**该方法返回一个数组，但是分为两种情况：**

### regexp 没有标志 g。

这种情况返回的数组只包含第一个匹配项，如果未找到匹配项将返回null。该返回的数组中，除了常规的数组元素外，还存在index和input两个对象属性，index存储的是匹配项在stringObject中的位置，而input存储的是stringObject的引用。比如：

```js
console.log(str1.match('Hello'));  //传入字符串，返回 ["Hello"]
console.log(str1.match(/\w+/));  //传入正则表达式，返回 ["Hello"]
```

### regexp 具有标志 g。

这种情况代表全局匹配，返回的数组由所有匹配到的字符串元素组成。比如：

```js
console.log(str1.match(/\w+/g));  //找到两个匹配项，返回 ["Hello", "Real"]
console.log(str2.match(/\w+/));  //未找到匹配项，返回null
```

## replace()，用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串

使用方法： 

> stringObject.replace(regexp/substr,replacement)

其中stringObject与regexp同上，substr代表需要被替换掉的子字符串，replacement代表替换文本或生成替换文本的函数。该方法返回替换成功之后的字符串。

```js
console.log(str1.replace('Hello','Hi'));  // "Hi，我叫Real"
console.log(str1.replace(/Hello/,'Hi'));  // "Hi，我叫Real"
console.log(str2.replace(/Hello/,'Hi'));  // 未匹配到被替换内容，返回原字符串"大家好，我叫张三"

str3 = "Hi,Real";
console.log(str3.replace(/(\w+),(\w+)/, "$2,$1"));   //将子字符串交换位置，返回"Real,Hi"
console.log(str1.replace(/\w+/,function(word){      //将字符串中的第一个匹配元素改为大写，返回字符串"HELLO，我叫Real"
    return word.toUpperCase();
}));
```

## search()，用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串

使用方法：

> stringObject.search(searchvalue)
> stringObject.search(regexp)

其中stringObject与regexp同上，返回stringObject中第一个与 regexp 相匹配的子串的起始位置。

```js
console.log(str1.search(/\w+/));  // 0
console.log(str1.search('Real'));  // 8
console.log(str1.search(/\w+/g));  // 0
console.log(str2.search(/\w+/));  // -1
```

通过第一行和第三行可以看出，用于全局匹配的 “g” 然而并没有什么卵用，原因是search()只返回第一个匹配元素的起始位置。
通过最后一行可以看出，当无法匹配任何元素时该函数将返回 -1。

## 总结

除了test()之外的其他三个函数，正则表达式都是作为函数参数传入其中的，所以只需记住test()这一个操作特例就行啦~~

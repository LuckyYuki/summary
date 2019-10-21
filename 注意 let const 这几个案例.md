## Tips

```js
{   // 代码块
  var a = 0;  // 全局变量，外部可访问
}
console.log(a);   // 0
```

```js
{   // 代码块
  let a = 0;  // 局部变量，外部可访问
}
console.log(a);   // 报错
```

```js
{
  a = 0;
  var a;
}
console.log(a);   // 0
```

```js
console.log(a);   // undefined
{
  a = 0;
  var a;
}
```

```js
{
  a = 0;   // 在声明前赋值会报错
  let a;
}

// 在声明前使用就会报错，不管代码块外面是否声明过这个变量
var a; 
{
  a = 0;  // 报错
  let a;
}
```

```js
var a = 0;
var a = 1;
console.log(a);  // 1
// 
let a = 0;
let a = 1;  // 报错
// 
let a = 0;
{
  var a = 1;  // 变量提升，报错
}
```

```js
let a = 0;
{
  let a = 1;  // 无变量提升，是局部变量，与全局声明的变量不在同一个作用域
}
console.log(a);  // 0
```

```js
const a = 0;
a = 1;  // 报错，常量值不可更改

```

```js
const a = [];
a.push(1);
console.log(a)  // [1]

// 
const a = [];
a = [1];  // 报错，引用不可更改
```

```js
// 声明时必须赋值
const a;   // 报错
```

## 总结

- ①let在代码块中声明的变量为局部变量
- ②let声明的变量，必须在其声明之后才能使用
- ③let在同一作用域内不可重复声明同一变量，包括var或const声明过的也不行
- ④const声明常量，不可更改，但如果是引用类型常量，则可以修改其内部数据
- ⑤const具有let相同特性（包括①②③所有特性）
- ⑥const在声明常量时必须同时进行赋值

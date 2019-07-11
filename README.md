# summary

> 12.toString() 会被解析成 12.（数字字面量） 和 toString()。
> 所以正常的写法是12..toString()才是正常的  或者加空格 12 .toString()

- 直接 import 一个模块，只是保证了这个模块代码被执行，引用它的模块是无法获得它的任何信息的。
- import "mod"; // 引入一个模块
- import v from "mod";  // 把模块默认的导出值放入变量 v

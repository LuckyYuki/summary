# ts属性后面的感叹号有什么用处？

首先看一段代码

```js
import { Vue, Component, Prop } from 'vue-property-decorator'

@Component
export default class YourComponent extends Vue {
  @Prop(Number) propA!: number
  @Prop({ default: 'default value' }) propB!: string
  @Prop([String, Boolean]) propC: string | boolean
}
```

这是vue组件的ts写法，仔细观察，里面有一段这样的代码@Prop(Number) propA!: number，这是什么鬼？

查了一下ts的文档说明，原来感叹号是非null和非undefined的类型断言，所以上面的写法就是对propA这个属性进行非空断言。文档的相关说明在这里。

官方文档上的一个例子很好的说明了这个问题

```js
interface Entity {
  name: string
}

// Compiled with --strictNullChecks
function validateEntity(e?: Entity) {
  // Throw exception if e is null or invalid entity
}

function processEntity(e?: Entity) {
  validateEntity(e);
  let s = e!.name;  // Assert that e is non-null and access name
}
```

如果直接使用let s = e.name;，编译器会抛出e可能不存在的错误，但是使用非空断言，则表示e肯定是存在的，从而不会产生编译问题


## 以下为官网文档出处

https://www.tslang.cn/docs/handbook/advanced-types.html

如果编译器不能够去除 null或 undefined，你可以使用类型断言手动去除。 语法是添加 !后缀： identifier!从 identifier的类型里去除了 null和 undefined：

```js
function broken(name: string | null): string {
  function postfix(epithet: string) {
    return name.charAt(0) + '.  the ' + epithet; // error, 'name' is possibly null
  }
  name = name || "Bob";
  return postfix("great");
}

function fixed(name: string | null): string {
  function postfix(epithet: string) {
    return name!.charAt(0) + '.  the ' + epithet; // ok
  }
  name = name || "Bob";
  return postfix("great");
}
```
本例使用了嵌套函数，因为编译器无法去除嵌套函数的null（除非是立即调用的函数表达式）。 因为它无法跟踪所有对嵌套函数的调用，尤其是你将内层函数做为外层函数的返回值。 如果无法知道函数在哪里被调用，就无法知道调用时 name的类型。

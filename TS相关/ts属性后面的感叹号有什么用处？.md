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

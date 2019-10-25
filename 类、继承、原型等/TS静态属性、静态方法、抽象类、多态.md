# TS静态属性、静态方法、抽象类、多态

https://github.com/forijk/start-with-typescript/tree/master/4.TypeScript类/

## 静态属性、静态方法

```js
function Person(){
	this.run1 = function (){} // 实例方法，实例化后调用
}

Person.run2 = function (){} // 静态方法，类名直接调用
Person.name = 'lucy' // 静态属性
Person.run2() // 静态方法的调用
```

### 静态方法、实例方法在jq中的应用

```js
function $(element){
	return new Base(element)
}
$.get = function (){}
function Base(element){
	this.element = '获取dom节点'
	this.css = function (attr, value){
		this.element.style[attr] = value
	}
}
$('#box').css('color', red)
```

## ts中定义静态方法

```js
class Person{
	public name:string
	public age:numer=20
	static sex = '男' // 静态属性
	constructor(name:string){
		this.name = name
	}
	run () {
		console.log(`${this.name}在运动`)
	}
	work(){
		console.log(`${this.name}在工作`)
	}
	static print(){
		console.log('print静态方法'+Person.sex) // 静态方法，没法直接调用类的属性
	}
}
var p = new Person('alex')
p.work()
p.run()
Person.sex // 男
Person.print()
```

## 多态

父类定义一个方法不去实现，让继承他的子类去实现，每一个子类有不同的表现
多态属于继承

```js
class Animal{
	name:string
	constructor(name:string){
		this.name
	}
	/* 具体吃什么不知道，具体吃什么继承它的子类去实现，
	每一个子类的表现不一样 */
	eat(){
		console.log('吃的方法')
	}
}

class Dog extends Animal{
	consturctor(name:string){
		super(name)
	}
	eat(){
		console.log(`${this.name}吃骨头`)
	}
}
class Cat extends Animal{
	constructor(name:string){
		super(name)
	}
	eat(){
		console.log(`${this.name}吃鱼`)
	}
}
```

## ts中的抽象类

ts中的抽象类：它是提供其他类继承的基类，不能直接被实例化
用abstract关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现
abstract抽象方法只能放在抽象类里面
抽象类和抽象方法定义标准

```js
/* 标准Animal这个类要求它的子类必须包含eat方法 */
abstract class Animal{
	name:string
	constructor(name:string){
		this.name = name
	}
	abstract eat():any; // 抽象类中的抽象方法不包含具体实现并且必须在派生类中实现
	run () {} // 其他方法可以不再子类实现	
}

class Dog extends Animal{
	constructor(name:string){
		super(name)
	}
	/* 抽象类的子类必须实现抽象类里面的抽象方法 */
	eat (){
		console.log(`${this.name}吃骨头`)
	}
}

class Cat extends Animal{
	constructor(name:string){
		super(name)
	}
	eat(){
		console.log(`${this.name}吃鱼`)
	}
}

var d = new Dog('小狗')
d.eat()

var c = new Cat('小猫')
c.eat()
```

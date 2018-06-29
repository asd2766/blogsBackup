---
layout: post
title: "学习JS——继承(一)"
date: 2018-06-29
comments: true
---

## ES5 继承类型

> ES5 中并没有传统的面向对象的概念，所以大家模拟了很多继承的方法，但本质上都是两种的变种：原型链继承，构造继承

父类：

```
function Animal() {
	this.type = '动物';
}
```

子类：

```
function Cat(name) {
	this.name = name;
}
```


### 原型链继承（这种比较常见）


现在我们要让 `Cat` 继承 `Animal`

```
// 将 Cat 的原型指向 Animal
Cat.prototype = new Animal();

// 上一句，会将 Cat 的 prototype 完全替换掉，
// 在控制台是打印可以看到， Cat.prototype.constructor 已经指向了 Animal
// 所以我们需要将其给重新指向 Cat, 不然创建的实例都将是指向 Animal的，
// 这样就会导致继承链紊乱（这里不是很懂，紊乱会怎么样，会有什么后果不知道）
Cat.prototype.constructor = Cat;
var cat1 = new Cat('大橘');
console.log(cat1.type); // 动物
```

## 构造继承

构造继承，我们需要在子类中加上 `apply` 或者 `call` 方法，将父类的构造函数绑定到子类中。

```
function Dog(name) {
	Animal.apply(this, arguments);
	this.name = name;
}

var dog1 = new Dog('旺旺');
console.log(dog1.name) // 旺旺
console.log(dog1.type) // 动物
```

> 此种继承方式只能继承父构造器中的属性，不能继承父构造器原型上的属性。Animal.apply(this, arguments) 也可用 Animal.call(this) 替换。Dog 对象有两个层级,第一级存放着自有属性以及父构造器中的属性，第二级存放着自己函数原型上的属性(Dog.prototype)。

![image](./img/js 集成 dog 案例.png)

## 组合继承

顾名思义，将构造继承 和 原型链继承 组合到一起使用。

```
function Fish(name) {
	Animal.call(this);
	this.name = name;
}

Fish.prototype = new Animal();

// 新增方法
Fish.prototype.say = function() { 
	console.log('booooooo');
}

var fish1 = new Fish('hah');

fish1.say(); // booooooo
```

内存优化：

```
function Fish(name) {
	Animal.call(this);
	this.name = name;
}

(function() {
	// 创建一个没有实例的类
	var Super = function() {};
	// 直接继承 prototype
	Super.prototype  = Animal.prototype;
	// 将实例作为子类的原型
	Fish.prototype = new Super();
	// 添加方法
	Fish.prototype.say = function() {
		console.log('boooooo');
	}
	
})()
```

## 直接继承 Prototype

> 这个方法是对 原型链继承 的改进，由于Animal 对象中，不变的属性都可以直接写入到`Animal.prototype`。所以我们可以直接继承 `Animal.prototype`。


1. 我们需要改写下 Animal

```
function Animal() {};
Animal.prototype.type = '动物';
```

2. 将 `Panda.prototype`  指向 `Animal.prototype`

```
function Panda(name) {
	this.name = name;
};
Panda.prototype = Animal.prototype;
Panda.prototype.constructor = Panda;
var panda1 = new Panda('熊熊');
console.log(panda1.name); // 熊熊
```

> 总结，该方法比原型链继承要省内存一点，效率要高一点（少创建了一个实例）。
> 但是在这个方法里面 `Panda.prototype` 和 `Animal.prototype` 指向了同一个，那么改动 Panda.prototype.constructor 时，同时将 Animal.prototype.constructor 也改掉了，从而影响父构造器创建对象

## 利用空对象作为中介

```
// 中介，中间层
// F是空对象，几乎不占内存
var F = function() {};
F.prototype = Animal.prototype;

// 子类
function Panda(name) {
	this.name = name;
}
// 这里 Panda.prototype 就间接的指向了 Animal.prototype
Panda.prototype = new F();
// 这是改动 Panda.prototype.constructor 就不会影响到 Animal.prototype
Panda.prototype.constructor = Panda;

```

所以我们可以将其封装成一个函数来进行调用

```
function extend(subClass, supClass) {
	var F = function() {};
	F.prototype = supClass.prototype;
	
	subClass = new F();
	subClass.prototype.constructor = subClass;
	// 这里定义个 parent 属性 来指向 supClass.prototype
	// 这样就可以通过 subClass.parent 来调用到父类的属性及方法
	subClass.parent = supClass.prototype;
}
```

## 拷贝继承

> 把父类的所有属性和方法，拷贝进子类类实现继承。

1. 将 `Animal` 的所有不变属性都放到 `prototype` 上

```
function Animal() {}
Animal.prototype.type = '动物';
```

2. 再写一个函数来实现拷贝功能

```
function copyExtend(sub, sup) {
	var parent = sup.prototype;
	var child = sub.prototype;
	
	for (var key in parent) {
		child[key] = parent[key];
	}
	
	child.parent = parent;
}
```

3. 使用

```
copyExtend(Cat, Animal);

var cat1 = new Cat('大橘');
console.log(cat1.name); // 大橘
console.log(cat1.type); // 动物
```


## 最后附上自己的练习
https://github.com/asd2766/exercise5.git


---
参考资料：

1. [阮一峰 -- Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)
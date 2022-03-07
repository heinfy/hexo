---
title: js中的原型及原型链
categories:
  - JavasCript
tags:
  - JavasCript
abbrlink: 8814181e
---

<!-- more -->

### 对象

概念：首先，对象是`JavaScript`中的一个基本数据类型，是一种复合值；它用一个（对象）名字命名，把一系列的属性和方法都包装在一个集合中，可以对象名访问这些属性和方法，即属性的无序集合。

#### 通过字面量创建对象

```javascript
//通过字面量创建对象
var obj1 = {
  name: 'Jack',
  age: 18
};
```

#### 通过系统自带的构造函数构造对象

```JavaScript
// 通过系统自带的构造函数构造对象
// 其他构造函数有：new Array(),new Number,new Date(),new Boolean()
 var obj2 = new Object();
 obj2.name = 'Jack';
 obj2.age = 18;
```

#### 通过自定义函数构造对象

```javascript
// 通过自定义函数构造对象
// 为了和普通函数区别，构造函数命名首字母要大写（帕斯卡命名法）
function Obj3(name, age) {
  this.name = name;
  this.age = age;
  this.sayHi = function () {
    alert(this.name);
  };
}
var dx1 = new Obj3('Jack', 18);
var dx2 = new Obj3('Brush', 18);
```

#### new 关键字

首先，我先说下自定义构造函数创建对象的基本流程，使用 new 和不适用 new 差别很大，不用 new 的话，则`Obj3(name)`就作为一个普通的函数执行（只是函数名的首字母大写了），没有返回值，那么默认返回的是`undefined`，而用`new`关键字后，JS 就会把该函数作为一个构造函数看待，经过一系列的隐式操作，返回值是一个对象了。

new 关键字的“隐式操作”：

① 在内存中创建一个空的 Object 类型的对象----看不到

② 让 this 关键字指向这个空的对象（将构造函数的作用域赋给新对象）----看不到

```javascript
// ① ② 的执行过程
var this = Object.creat(Obj3.prototype);
// this 指向 Obj3.prototype(后面讲到)
// 可以看出构造函数创建对象的最根本原理是借用Object.create()方法来实现的，只不过被封装功能化了
```

在前面的例子中，`dx1`和`dx2`分别保存着`Obj3`的一个不同的实例。这俩个对象都有有一个`constructor`（构造函数）的属性，该属性指向`Obj3`，如下：

```javascript
console.log(dx1.constructor == Obj3); // true
console.log(dx2.constructor == Obj3); // true
```

对象的 constructor 属性最初用来表示对象类型的。但是，检测对象类型，还是`instanceof`操作符要更可靠一些-----用来检测一个对象在其原型链中，是否存在一个构造函数的`prototype`属性。

我们在这个例子中创建的所有对象即使`Object`的实例，同时也是`Obj3`的实例，这一点可以通过`instanceof`操作符可以得到验证。

```javascript
console.log(dx1 instanceof Obj3); // true
console.log(dx1 instanceof Object); // true
console.log(dx2 instanceof Obj3); // true
console.log(dx2 instanceof Object); // true
// dx1 和 dx2 之所以同时是Object的实例，是因为所有的对象均继承自Object
```

③ 通过 this 给这个对象添加属性和方法---看得见

```javascript
// this.name = 'Jack';
this.age = 18;
```

④ 将这个对象返回给用`new`关键字调用的构造函数`Obj3(name)`的调用者（`dx1`）

```javascript
return this;
```

注：`Object`在 JS 中，所有不同类型的对象，都直接或间接的集成于它。

### 原型对象和原型链

#### 原型

我们所创建的每一个函数都有一个 prototype （原型）的属性，这个属性是一个指针，指向一个对象，而这个对象的用途包含可以由特定类型的所有实例共享的属性和方法。

那么 prototype 就可以通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中第一对象实例的信息，而是可以将这些信息直接添加到原型对象中去，那么上面的 Obj3（）函数就可以写成这样：

```javascript
function Obj3(name, age) {
  this.name = name;
  this.age = age;
}
Obj3.prototype.sayHi = function () {
  alert(this.name);
};
var dx1 = new Obj3('Jack', 18);
var dx2 = new Obj3('Brush', 18);
```

在默认情况下，所有原型对象都会自动获得一个 constructor（构造函数）属性，这个属性包含一个指向 prototype 属性所在函数的指针。就拿上面的例子来说，Obj3.prototype.constructor 指向 Obj3。而通过这个构造函数，我们还可以继续为原型对象添加其他属性和方法。

当构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型方法。在每一个对象都支持一个属性："\_ _proto_ \_"。

我们可以通过 isPrototype()方法来确定对象之间是否存在这个关系。从本质上讲，如果[Prototype]指向调用 isPrototype()方法的对象（Obj3.prototype），那么这个方法就返回 true：

```javascript
console.log(dx1.prototype.isPrototypeOf(Obj3)); // true
console.log(dx2.prototype.isPrototypeOf(Obj3)); // true
```

使用 hasOwnProperty()可以检测一个属性是否存在在实例中，还是存在在原型中。这个方法（从 Object 继承来的）只是给定属性存在在对象实例中，才会返回 true；存在在原型中，返回 false。

#### 原型对象的原型

所有的原型应用类型（Object、Array、String）都在其构造函数的原型上定义了方法。在 Array.prototype 中可以找到 sort（）方法；在 String.prototype 中可以找到 substring（）方法，如下：

```javascript
console.log(Array.prototype.sort); // ƒ sort() { [native code] }
console.log(String.prototype.substring); // ƒ substring() { [native code] }
```

### 原型链

原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。如果，我们让这个原型对象等于另一个类型的实例，显然，此时的原型对象包含一个指向另一个原型的指针，相应的，另一个原型中野包含着指向另另一个类型的实例，那么，层层递进，就构成了实例与原型的链条，这就是原型链的基本概念。

我们知道，所有引用类型默认都继承了 Object，而这个继承也是通过原型链实现的。所有函数的默认原型都是 Object 的实例，因此默认原型都包含一个内部指针，指向`Object.prototype`。这正是所有自定义类型都会继承`toString()、valueOf()`等默认方式的根本原因。

![原型](/js中的原型及原型链/prototype.jpg)

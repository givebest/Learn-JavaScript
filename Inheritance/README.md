# JavaScript Inheritance

## 原型链

构造函数、原型和实例的关系：

* 每个构造函数都有一个**原型对象**，原型对象都包含一个**指向构造函数**的指针；
* 实例都包含了一个**指向原型对象**的内部指针。

## 继承

### 1. 组合继承

组合继承（伪经典继承），将原型链和借用构造函数的技术组合到一块。

* 使用原型链实现对原型属性和方法的继承。
* 借用构造函数来实现对实例属性的继承。

```js
/**
 * 1. 组合继承（伪经典继承），将原型链和借用构造函数的技术组合到一块。
 * 使用原型链实现对原型属性和方法的继承。
 * 借用构造函数来实现对实例属性的继承。
 */
function SuperType (name) {
   this.name = name;
   this.colors = ['red', 'blue', 'green'];
 }

 SuperType.prototype.sayName = function () {
   console.log(this.name);
 }

 function SubType (name, age) {
   // 继承属性
   SuperType.call(this, name);

   this.age = age;
 }

 // 继承方法
 SubType.prototype = new SuperType();
 SubType.prototype.constructor = SubType; //增强对象
 SubType.prototype.sayAge = function () {
   console.log(this.age);
 }

 console.warn('组合继承');
 let instance1 = new SubType('TB', 20);
 instance1.colors.push('yellow');
 console.log(instance1.colors);  // ["red", "blue", "green", "yellow"]
instance1.sayName();  // TB
instance1.sayAge();  // 20

let instance2 = new SubType('JD', 15);
console.log(instance2.colors);  // ["red", "blue", "green"]
instance2.sayName();  // JD
instance2.sayAge(); // 15
```

### 2. Object.create() 继承

Object.create() 创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create

```js
 /**
  * 2. Object.create() 继承
  * 创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。
  * @see https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create
  */
function SuperCar (name) {
  this.name = name;
  this.colors = ['black', 'white'];
}

SuperCar.prototype.sayName = function () {
  console.log(this.name);
}

function SubCar (name) {
  // 继承属性
  SuperCar.call(this, name)
}

// 继承方法
SubCar.prototype = Object.create(SuperCar.prototype);
SubCar.prototype.constructor = SubCar;

console.warn('Object.create 继承');
let car1 = new SubCar('TSL');
car1.colors.push('yellow');
console.log(car1.colors); // ["black", "white", "yellow"]
car1.sayName();  // TSL

let car2 = new SubCar('KLL');
console.log(car2.colors); // ["black", "white"]
car2.sayName(); // KLL
```

### 3. class & extends 继承

 * class 声明创建一个基于原型继承的具有给定名称的新类。 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class
 * extends 关键字用于类声明或者类表达式中，以创建一个类，该类是另一个类的子类。 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/extends

```js
/**
 * 3. class & extends继承
 * class 声明创建一个基于原型继承的具有给定名称的新类。
 * @see https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/class
 * extends 关键字用于类声明或者类表达式中，以创建一个类，该类是另一个类的子类。
 * @see https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/extends
 */
class SuperPen {
  constructor (name) {
    this.name = name;
    this.colors = ['white', 'black'];
  }

  sayName () {
    console.log(this.name);
  }
}

class SubPen extends SuperPen {
  constructor (name) {
    super(name)
  }

  sayColrs () {
    console.log(this.colors);
  }
}

console.warn('class 继承');
let pen1 = new SubPen('pencil');
pen1.colors.push('gray');
pen1.sayName(); // pencil
pen1.sayColrs(); // ["white", "black", "gray"]

let pen2 = new SubPen('ball-pen');
pen2.sayName();  // ball-pen
pen2.sayColrs(); // ["white", "black"]
``

```
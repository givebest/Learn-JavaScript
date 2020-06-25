# JavaScript Object

## 创建一个对象
```js
let obj1 = {
  a: 1,
  b: 2,
  c: 3,
  d: {
    e: 4,
  },
  f: function () {
    return this.a
  }
};

/*  
Object.prototype.__proto__ 
改变一个对象的 [[Prototype]] 属性。
已经废弃，建议只使用 Object.setPrototypeOf/Object.getPrototypeOf，
或者 Object.create
*/
obj1.__proto__.g = 5;  

/*
Object.setPrototypeOf() 设置一个指定的对象的原型（内部[[Prototype]]属性）到另一个对象或 null。
性能缓慢，应避免使用。
*/
Object.setPrototypeOf(obj1, {h: 6});
console.warn('------obj1--------');
console.log(obj1.a, obj1.g, obj1.h, obj1.f()); // 1 5 6 1
```
## 拷贝

### 浅拷贝

#### JSON

* Function 不能拷贝
* Object.setPrototypeOf 设置的对象不能拷贝

```js
let obj2 = JSON.parse(JSON.stringify(obj1));
// obj1.a = '0'
console.warn('------obj2--------');
console.log(obj2.a, obj2.g, obj2.h); // 1 5 undefined
try {
  console.log(obj2.f());
} catch (error) {
  console.log('obj2.f() 不能访问');  
}
// obj2.f() 不能访问
```

### 深拷贝

#### For in & hasOwnProperty

```js
function cloneObj1 (obj) {
  if (Object.prototype.toString.call(obj) !== '[object Object]') return obj;

  let tempObj = {};
  // let tempObj = obj.constructor();
  for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
      tempObj[key] = obj[key]
    }
  }
  return tempObj;
}


let obj3 = cloneObj1(obj1);
obj1.a = 3;
console.warn('------obj3--------');
console.log(obj3.a, obj3.g, obj3.h, obj3.f());  // 1 5 undefined 1
```

#### Object.assign()

> 将所有可枚举属性的值从一个或多个源对象复制到目标对象。

```js
let obj4 = Object.assign({}, obj1);
obj1.a = 4;
console.warn('------obj4--------');
console.log(obj4.a, obj4.g, obj4.h, obj4.f());  // 3 5 undefined 3
```

## 继承

### Object.create()

> 创建新对象，使用现有的对象来提供新创建的对象的__proto__。

```js
let obj5 = Object.create(obj1);
obj1.a = 5;
// obj5.a = 'x';


console.warn('------obj5--------');
// obj5 继承 obj1，obj1.a 的值改变， obj5 也改变
console.log(obj5.a, obj5.g, obj5.h, obj5.f());  // 5 5 6 5
```
# JavaScript Array

## 一个数组
```js
let array1 = [
  1,
  2,
  3,
  4,
  5,
  [6, 7, [8, 9, [null, undefined, true, false, "a", "b", "c"]]],
];
```
## 拷贝

### 1. slice

```js
let array2 = array1.slice();
array1[0] = "a";
console.warn("Array 拷贝 slice");
console.log(array2, array2[0]); // [1, 2, 3, 4, 5, Array(3)]  1
```

### 2. 展开语法(...)

```js
let array21 = [...array1];
array1[0] = "b";
console.warn("Array 拷贝 ...");
console.log(array21, array21[0]); // ["a", 2, 3, 4, 5, Array(3)] "a"
```

### 3. Array.from() 

> Array.from() 从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

```js
let array22 = Array.from(array1);
array1[0] = 'c';
console.warn('Array 拷贝 Array.from()');
console.log(array22);  // ["b", 2, 3, 4, 5, Array(3)]
```

## 扁平化（展开）

### 1. join() & split()

 * 数字会转换为字符串
 * 布尔值会转换为字符串
 * null 和 undefined 会转换为 ""

```js
function flattenArray(array) {
  if (!Array.isArray(array)) return array;

  return array.join(",").split(",");
}
let array3 = flattenArray(array1);
console.warn("Array 扁平化 1");
console.log(array3); // ["c", "2", "3", "4", "5", "6", "7", "8", "9", "", "", "true", "false", "a", "b", "c"]
```

### 2. 遍历递归

```js
function flattenArray1(array) {
  let result = [];

  array.forEach((item) => {
    Array.isArray(item) ?
      result.push(...flattenArray1(item)) :
      result.push(item);
  });
  return result;
}
let array4 = flattenArray1(array1);
console.warn("Array 扁平化 2");
console.log(array4); // ["c", 2, 3, 4, 5, 6, 7, 8, 9, null, undefined, true, false, "a", "b", "c"]
```

### 3. ES2019 Array.prototype.flat()

> flat() 方法会按照一个可指定的尝试递归遍历数据，并将所有元素和遍历到的子数组中的元素合并为一个新数组返回。

```js
let array5 = array1.flat(Infinity);
array1[0] = "c";
console.warn("Array 扁平化 flat");
console.log(array5); //  ["c", 2, 3, 4, 5, 6, 7, 8, 9, null, undefined, true, false, "a", "b", "c"]
```

## Array 去重

### 1. 筛选 filter

```js
function uniqueArray (array) {
  return array.filter((v, i, a) => {
    return array.indexOf(v) === i
  });
}
let array11 = [1, 2, 5, 4, 3, 5, 7, 8, 9, 3, 2, 1, '1', '3', '6', '2', '3', '2'];
let array6 = uniqueArray(array11);
console.warn('Array 去重 fliter');
console.log(array6);  // [1, 2, 5, 4, 3, 7, 8, 9, "1", "3", "6", "2"]
```

### 2. new Set()

> 传递一个可迭代对象，它的所有元素将不重复地被添加到新的 Set 中。

```js
function uniqueArray1 (array) {
  return Array.from(new Set(array));
}
console.warn('Array 去重 new Set');
let array7 = uniqueArray1(array11);
console.log(array7); // [1, 2, 5, 4, 3, 7, 8, 9, "1", "3", "6", "2"]
```
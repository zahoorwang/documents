---
title: 数组方法
---

## 介绍

虽然 **MDN** 已经很详细了，但还是需要记录一下。掌握这些方法虽然是必须的，但更重要的是学习如何写作，为一个方法添加语法、描述、示例，清晰而又明了。其中的 Polyfill 价值很高，之后单独拆分出来进行理解。

> 1. 示例部分会引用英文版本的介绍。
> 2. 部分方法并不会产生返回值，为了减少描述，省略 `console` 步骤。



## Array.from

用于从一个类似数组或可迭代对象中创建一个新的，浅拷贝的数组实例。

### 1. 语法

::: danger
Array.from(arrayLike[, mapFn[, thisArg]])
:::

参数：

+ arrayLike：想要转换成数组的类数组对象或可迭代对象；
+ mapFn（可选参数）：如果制定了该参数，新数组中的每个元素都会执行该回调函数；
+ thisArg（可选参数）：执行回调函数 mapFn 时 this 对象。

返回值：

一个新的数组实例。

### 2. 描述

`Array.from()` 可以通过以下方式来创建数组对象：

+ 类数组对象（拥有一个 `length` 属性和若干索引属性的任意对象）；
+ 可迭代对象（可以获取对象中的元素，如 Map 和 Set 等）。

`Array.from()` 方法有一个可选参数 `mapFn`，可以在生成的数组上再执行一次 `map` 方法后返回。也就是说 `Array.from(obj, mapFn, thisArg)` 就相当于 `Array.from(obj).map(mapFn, thisArg)`，除非创建的是不可用的中间数组。

`from()` 的 length 属性为 1，即 `Array.from.length === 1` 。

在 ES2015 中，`Class` 语法允许我们为内置类型（比如 Array）和自定义类新建子类（比如叫 SubArray）。这些子类也会继承父类的静态方法，比如 `SubArray.from()`，调用该方法后会返回子类 `SubArray` 的一个实例，而不是 `Array` 的实例。

> 个人在日常开发中常用场景：
>
> 1. 将获取的 DOM 类数组转为数组；
> 2. 将一串字符串转为数组；
> 3. 将 Map 类数组转为数组。

### 3. 示例

+ Array from a String

  ```js
  Array.from('foo');  // ["f", "o", "o"]
  ```

+ Array from a Set

  ```js
  let s = new Set(['foo', window]); 
  Array.from(s);  // ["foo", window]
  ```

+ Array from a Map

  ```js
  let m = new Map([[1, 2], [2, 4], [4, 8]]);
  Array.from(m);  // [[1, 2], [2, 4], [4, 8]]
  ```

+ Array from an Array-like object (arguments)

  ```js
  function f() {
    return Array.from(arguments);
  }
  
  f(1, 2, 3); // [1, 2, 3]
  ```

+ Using arrow functions and Array from

  ```js
  Array.from([1, 2, 3], x => x + x); // [2, 4, 6]
  Array.from({length: 5, 0: 'a'}); //  ["a", undefined, undefined, undefined, undefined]
  Array.from({length: 5}, (v, i) => i); // [0, 1, 2, 3, 4]
  ```

+ Sequence generator (range)

  > 这是一个非常有意思的写法，很值得学习。

  ```js
  const range = (start, stop, step) => Array.from({ length: (stop - start) / step + 1}, (_, i) => start + (i * step));
  
  range(0, 4, 1);
  // [0, 1, 2, 3, 4] 
  
  range(1, 10, 2); 
  // [1, 3, 5, 7, 9]
  
  range('A'.charCodeAt(0), 'Z'.charCodeAt(0), 1).map(x => String.fromCharCode(x));
  // ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"]
  ```

  

## Array.of

用于创建一个具有可变数量参数的新数组实例，而不需要考虑参数的数量或类型。与 Array 方法的区别是当处理单个参数且该参数为正整数类型时（负整数 Array 会报错），Array 方法会创建对应长度的空数组（空数组是指对应长度空位“empty”的数组），该行为会影响 map 方法无法生效。

> 其实我一直很好奇为什么被命名为 of

### 1. 语法

::: danger
Array.of(element0[, element1[, ...[, elementN]]])
:::

参数：

- elementN：任意个参数，将按顺序称为返回数组中的元素。

返回值：

一个新的数组实例。

### 2. 描述

此函数是 ECMAScript 2015 标准的一部分。

> 个人在日常开发中常用场景：
>
> + 尚未使用过。

### 3. 示例

```js
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of(undefined); // [undefined]
Array.of(1, [2]); // [1, [2]]
Array.of(1, {a: "1"}); // [1, {a: "1"}]
```



## Array.prototype.concat

### 1. 语法

::: danger
arr.concat(value1[, value2[, ...[, valueN]]])
:::

参数：

+ valueN：数组或值。

返回值：

一个新的数组实例。

### 2. 描述

`concat` 方法创建一个新的数组，它由被调用的对象中的元素组成，每个参数的顺序依次是该参数的元素（如果参数是数组）或参数本身（如果参数不是数组）。它不会低轨道嵌套数组参数中。

`concat` 方法不会改变 this 或任何作为参数提供的数组，而是返回一个浅拷贝，它包含与原始数组相结合的相同元素的副本。

> 个人在日常开发中常用场景：
>
> - 偶尔需要拼接数组时会使用。

### 3. 示例

+ concatenating two arrays

  ```js
  const letters = ['a', 'b', 'c'];
  const numbers = [1, 2, 3];
  letters.concat(numbers); // ['a', 'b', 'c', 1, 2, 3]
  ```

+ concatenating three arrays

  ```js
  const num1 = [1, 2, 3];
  const num2 = [4, 5, 6];
  const num3 = [7, 8, 9];
  
  num1.concat(num2, num3); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
  ```

+ concatenating values to an array

  ```js
  const letters = ['a', 'b', 'c'];
  
  letters.concat(1, [2, 3]); // ['a', 'b', 'c', 1, 2, 3]
  ```

+ concatenating nested arrays

  ```js
  const num1 = [[1]];
  const num2 = [2, [3]];
  
  num1.concat(num2); // [[1], 2, [3]]
  num1[0].push(4); // [[1, 4], 2, [3]]
  ```




## Array.prototype.copyWithin

### 1. 语法

::: danger
arr.copyWithin(target[, start[, end]])
:::

参数：

+ target：索引（复制序列到该位置）。如果是负数，则从末尾开始，若大于等于 `arr.length`，则不发生拷贝；
+ start：索引（开始复制元素的起始位置），负数同上，默认为 0；
+ end：索引（开始复制元素的结束位置），负数同上，默认为 `arr.length`。

返回值：

改变后的数组。

### 2. 描述

`copyWithin()` 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。

参数 `target`、`start`、`end` 必须为整数。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

```js
let numbers = [1, 2, 3, 4, 5];

// target
numbers.copyWithin(2); // [1, 2, 1, 2, 3]
numbers.copyWithin(-3); //  [1, 2, 1, 2, 1]
// target、start
numbers.copyWithin(0, 3); //  [2, 1, 1, 2, 1]
numbers.copyWithin(2, -2); //  [2, 1, 2, 1, 1]
// target、start、end
numbers.copyWithin(-2, -4, -2); //   [2, 1, 2, 1, 2]

// 类数组
[].copyWithin.call({length: 5, 3: 1}, 0, 3); // {0: 1, 3: 1, length: 5}

let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2); // Int32Array [3, 4, 5, 4, 5]

[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4); // Int32Array [4, 2, 3, 4, 5]
```



## Array.prototype.entries

### 1. 语法

::: danger
arr.entries()
:::

返回值：

一个新的 Array 迭代器对象。

### 2. 描述

`entries()` 方法返回一个新的 Array Iterator 对象，该对象包含数组中每个索引的键/值对。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

+ array iterator

  ```js
  const arr = ['a', 'b', 'c'];
  const iterator = arr.entries();
  
  console.log(iterator); // Array Iterator {}
  ```

+ iterator.next

  ```js
  const arr = ['a', 'b', 'c'];
  const iterator = arr.entries();
  
  console.log(iterator.next()); // Object { value: Array [0, "a"], done: false }
  console.log(iterator.next()); // Object { value: Array [1, "b"], done: false }
  console.log(iterator.next()); // Object { value: Array [2, "c"], done: false }
  console.log(iterator.next()); // Object { value: undefined, done: true }
  console.log(iterator.next()); // Object { value: undefined, done: true }
  ```

+ iterating with index and elements

  ```js
  const a = ['a', 'b', 'c'];
  
  for (const [index, element] of a.entries()) {
    console.log(index, element);
  }
  
  // 0 'a' 
  // 1 'b' 
  // 2 'c'
  ```



## Array.prototype.every

### 1. 语法

::: danger
arr.every(callback[, thisArg])
:::

参数：

+ callback：回调函数；
  + element：当前值；
  + index：当前值的索引；
  + array：当前数组。
+ thisArg：执行 callback 时使用的 `this` 值。

返回值：

返回一个布尔值。

### 2. 描述

`every()` 方法为数组中的每个元素都执行一次 `callback` 函数，直到找到一个返回 `false` 的元素。如果找到一个这样的元素则立即返回 false，否则返回 true。

> 个人在日常开发中常用场景：
>
> - 多用于判断多种状态是否符合，传递给变量，交由 if 处理。

### 3. 示例

+ testing size of all array elements

  ```js
  function isBigEnough(element, index, array) {
    return element >= 10;
  }
  
  [12, 5, 8, 130, 44].every(isBigEnough);   // false
  [12, 54, 18, 130, 44].every(isBigEnough); // true
  ```

+ using arrow functions

  ```js
  [12, 5, 8, 130, 44].every(x => x >= 10); // false
  [12, 54, 18, 130, 44].every(x => x >= 10); // true
  ```



## Array.prototype.fill

### 1. 语法

::: danger
arr.fill(value[, start[, end]])
:::

参数：

+ value：用来填充数组元素的值；
+ start：起始索引，默认 0；
+ end：终止索引，默认 `this.length`。

返回值：

修改后的数组。

### 2. 描述

`fill` 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素，但不包括终止索引。

`fill` 方法故意被设计为通用方法，该方法不要求 `this` 是数组对象。

> 个人在日常开发中常用场景：
>
> + 尚未使用过。

### 3. 示例

```js
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5);         // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}

// Objects by reference.
var arr = Array(3).fill({}) // [{}, {}, {}];
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```



## Array.prototype.filter

### 1. 语法

::: danger
arr.filter(callback(element[, index[, array]])[, thisArg])
:::

参数：

+ callback：回调函数；
  - element：当前值；
  - index：当前值的索引；
  - array：当前数组。
+ thisArg：执行 callback 时使用的 `this` 值。

返回值：

一个新的数组实例。

### 2. 描述

`filter()` 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 通过回调函数返回 true 的元素会包含到新数组中。`filter` 遍历的元素范围在第一次调用 `callback` 时之前就已经确定了，之后添加元素不会被遍历到。

> 个人在日常开发中常用场景：
>
> - 所有需要被过滤的数据都会使用该方法。

### 3. 示例

+ filtering invalid values

  ```js
  const arr = [1, 0, "", "2", false, undefined, null, {}, {a: "hi"}];
  arr.filter(a => a); // [1, "2", {  }, { a: "hi" }]
  ```

+ filtering out all small values

  ```js
  function isBigEnough(value) {
    return value >= 10;
  }
  
  [12, 5, 8, 130, 44].filter(isBigEnough); // [12, 130, 44]
  ```

+ filtering invalid entries from JSON

  ```js
  const arr = [
    { id: 15 },
    { id: -1 },
    { id: 0 },
    { id: 3 },
    { id: 12.2 },
    { },
    { id: null },
    { id: NaN },
    { id: 'undefined' }
  ];
  
  let invalidEntries = 0;
  
  function isNumber(obj) {
    return obj !== undefined && typeof(obj) === 'number' && !isNaN(obj);
  }
  
  function filterByID(item) {
    if (isNumber(item.id) && item.id !== 0) {
      return true;
    }
    invalidEntries++;
    return false;
  }
  
  arr.filter(filterByID);
  // Filtered Array [{ id: 15 }, { id: -1 }, { id: 3 }, { id: 12.2 }]
  //  invalidEntries 5
  ```

+ searching in array

  ```js
  const fruits =  ['apple', 'banana', 'grapes', 'mango', 'orange'];
  
  function filterItems(arr, query) {
    return arr.filter(function(el) {
      return el.toLowerCase().indexOf(query.toLowerCase()) !== -1;
    });
  }
  
  filterItems(fruits, 'ap'); // ['apple', 'grapes']
  filterItems(fruits, 'an'); // ['banana', 'mango', 'orange']
  ```



## Array.prototype.find

### 1. 语法

::: danger
arr.find(callback[, thisArg])
:::

参数：

- callback：回调函数；
  - element：当前值；
  - index：当前值的索引；
  - array：当前数组。
- thisArg：执行 callback 时使用的 `this` 值。

返回值：

返回一个满足回调函数的值，否则返回 undefined。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 返回对象组成的数组中的对应对象。

### 3. 示例

+ find an object in an array by one of its properties

  ```js
  const inventory = [
      {name: 'apples', quantity: 2},
      {name: 'bananas', quantity: 0},
      {name: 'cherries', quantity: 5}
  ];
  
  inventory.find(fruit => fruit.name === 'cherries'); //  { name: 'cherries', quantity: 5 }
  ```

+ find a prime number in an array

  ```js
  function isPrime(element, index, array) {
    var start = 2;
    while (start <= Math.sqrt(element)) {
      if (element % start++ < 1) {
        return false;
      }
    }
    return element > 1;
  }
  
  [4, 6, 8, 12].find(isPrime); // undefined
  [4, 5, 8, 12].find(isPrime); // 5
  ```



## Array.prototype.findIndex

### 1. 语法

::: danger
arr.findIndex(callback[, thisArg])
:::

参数：

- callback：回调函数；
  - element：当前值；
  - index：当前值的索引；
  - array：当前数组。
- thisArg：执行 callback 时使用的 `this` 值。

返回值：

返回对应值的下标，否则返回 -1。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 较少使用该方法，首先考虑能否使用 includes，若不支持才会考虑该项。

### 3. 示例

参考 find 的[示例](/frontend/javascript/array-methods.html#_3-示例-9)即可。



## Array.prototype.flat

### 1. 语法

::: danger
arr.flat(depth)
:::

参数：

- depth：指定要提取嵌套数组的结构深度，默认值为 1，Infinity 表示无限层级。

返回值：

一个包含将数组与子数组中所有元素的新数组。

### 2. 描述

`flat()` 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。方法很美丽，但是兼容性很差，所以提供了替代方案。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

+ flattening nested arrays

  ```js
  const arr = [1, 2, [3, 4]];
  arr.flat();  // [1, 2, 3, 4]
  
  const  arr = [1, 2, [3, 4, [5, 6]]];
  arr.flat(); // [1, 2, 3, 4, [5, 6]]
  
  const arr = [1, 2, [3, 4, [5, 6]]];
  arr.flat(2); // [1, 2, 3, 4, 5, 6]
  ```

+ flattening and array holes

  ```js
  const arr = [1, 2, , 4, 5];
  arr.flat(); // [1, 2, 4, 5]
  ```

### 4. 替代方案

使用 reduce 和 concat：

+ flat single level array

  ```js
  const arr = [1, 2, [3, 4]];
  arr.reduce((acc, val) => acc.concat(val), []); // [1, 2, 3, 4]
  [].concat(...arr); // [1, 2, 3, 4]
  ```

+ enable deep level flatten use recursion with reduce and concat

  ```js
  const arr = [1,2,3,[1,2,3,4, [2,3,4]]];
  
  function flattenDeep(arr) {
    return arr.reduce((acc, val) => Array.isArray(val) ? acc.concat(flattenDeep(val)) : acc.concat(val), []);
  }
  
  flattenDeep(arr); // [1, 2, 3, 1, 2, 3, 4, 2, 3, 4]
  ```

+ non recursive flatten deep using a stack

  ```js
  let arr = [1,2,3,[1,2,3,4, [2,3,4]]];
  function flatten(input) {
    const stack = [...input];
    const res = [];
    
    while (stack.length) {
      const next = stack.pop();
      if (Array.isArray(next)) {
        stack.push(...next);
      } else {
        res.push(next);
      }
    }
    
    return res.reverse();
  }
  flatten(arr); // [1, 2, 3, 1, 2, 3, 4, 2, 3, 4]
  ```



## Array.prototype.flatMap

### 1. 语法

::: danger
arr.flatMap(function callback(currentValue[, index[, array]]) {

​	    // return element for new_array

}[, thisArg])
:::

参数：

- callback：回调函数；
  - currentValue：当前值；
  - index：当前值的索引；
  - array：当前数组。
- thisArg：执行 callback 时使用的 `this` 值。

返回值：

 一个新的数组，其中每个元素都是回调函数的结果，并且结构深度 `depth` 值为 1。

### 2. 描述

相当于执行 `map` 方法后，对返回值组成的数组执行 `flat` 方法。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

+ map and flatMap

  ```js
  const arr = [1, 2, 3, 4];
  
  arr.map(x => [x * 2]); // [[2], [4], [6], [8]]
  arr.flatMap(x => [x * 2]); // [2, 4, 6, 8]
  arr.flatMap(x => [[x * 2]]); // [[2], [4], [6], [8]]
  ```

+ a list of sentences

  ```js
  let arr = ["今天天气不错", "", "早上好"];
  
  arr.map(s => s.split("")); // [["今", "天", "天", "气", "不", "错"],[""],["早", "上", "好"]]
  
  arr.flatMap(s => s.split('')); // ["今", "天", "天", "气", "不", "错", "早", "上", "好"]
  ```



## Array.prototype.forEach

### 1. 语法

::: danger
arr.forEach(callback[, thisArg])
:::

参数：

- callback：回调函数；
  - currentValue：当前值；
  - index：当前值的索引；
  - array：当前数组。
- thisArg：执行 callback 时使用的 `this` 值。

返回值：

 undefined。

### 2. 描述

`forEach` 方法按升序为数组中含有效值的每一项执行一次 callback 函数，那些已删除或未初始化的项将被跳过（例如稀疏数组）。

如之前的方法一致，`forEach` 遍历的范围在第一次调用 callback 前就会确定。调用 callback 后添加到数组中的项不会被 callback 访问到。但是，如果已经存在的值被改变，则传递给 callback 的值是  `forEach` 遍历到它们那一刻的值。参见下方示例。

你无法终止跳出 `forEach` 循环，除了抛出一个异常。如果你需要这样做，使用 `forEach` 方法是错误的，请使用其余循环方法。

> 个人在日常开发中常用场景：
>
> - ~~所有使用 for 循环的场景（今天才改正过来……）。~~
> - 不需要终止 for 循环的场景。

### 3. 示例

+ no opeartion for uninitialized values(sparse arrays)

  ```js
  // 跳过无效值
  const arraySparse = [1, 3,,7];
  let numCallbackRuns = 0;
  
  arraySparse.forEach(el => {
    console.log(el);
    // 1
  	// 3
  	// 7
    numCallbackRuns++;
  });
  
  console.log("numCallbackRuns: ", numCallbackRuns); // numCallbackRuns: 3
  ```

+ printing the contents of an array

  ```js
  function logArrayElements(elements, index, array) {
    console.log('a[' + index + '] = ' + element);
  }
  [2, 5, , 9].forEach(logArrayElements);
  // a[0] = 2
  // a[1] = 5
  // a[3] = 9
  ```

+ using thisArg

  ```js
  function Counter() {
    this.sum = 0;
    this.count = 0;
  }
  
  Counter.prototype.add = function(array) {
    array.forEach(entry => {
      this.sum += entry;
      ++this.count;
    }, this);
  }
  
  const obj = new Counter();
  obj.add([2, 5, 9]);
  obj.count;
  // 3 
  obj.sum;
  // 16
  ```

+ an object copy function

  ```js
  function copy(obj) {
    const copy = Object.create(Object.getPrototypeOf(obj));
    const propNames = Object.getOwnPropertyNames(obj);
    
    propNames.forEach(name => {
      const desc = Object.getOwnPropertyDescriptor(obj, name);
      Object.defineProperty(copy, name, desc);
    });
    
    return copy;
  }
  
  const obj1 = { a: 1, b: 2 };
  const obj2 = copy(obj1); // obj2 looks like obj1 now
  ```

+ if the array is modified during iteration, other elements might be skipped

  ```js
  let words = ['one', 'two', 'three', 'four'];
  words.forEach(word => {
    console.log(word);
    if (word === 'two') {
      words.shift();
    }
  });
  
  // one
  // two
  // four
  ```

+ flatten an array

  ```js
  function flatten(arr) {
    const result = [];
    
    arr.forEach(i => {
      Array.isArray(i) ? result.push(...flatten(i)) : result.push(i);
    });
    
    return result;
  }
  
  const problem = [1, 2, 3, [4, 5, [6, 7], 8, 9]];
  flatten(problem); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
  ```




## Array.prototype.includes

### 1. 语法

::: danger
arr.includes(valueToFind[, fromIndex])
:::

参数：

- valueToFind：需要查找的元素；
- fromIndex：从 fromIndex 索引处开始查找 valueToFind，可为负值。

返回值：

 一个布尔值。

### 2. 描述

includes 方法用来判断一个数组是否包含一个指定的值，包含返回 true，否则返回 false。

> 个人在日常开发中常用场景：
>
> - 常用于替代多个连续的 && 操作。

### 3. 示例

+ fromIndex is greater than or equal to the array length

  ```js
  const arr = ['a', 'b', 'c'];
  
  arr.includes('c', 3); // false
  arr.inclueds('c', 100); // false
  ```

+ computed index is less than o

  ```js
  const arr = ['a', 'b', 'c'];
  
  arr.includes('a', -100); // true
  arr.includes('b', -100); // true
  arr.includes('c', -100); // true
  arr.includes('a', -2); // false // 3 + (-2) = 1
  ```

+ includes() used as a generic method

  ```js
  (function() {
    console.log([].includes.call(arguments, 'a')); // true
    console.log([].includes.call(arguments, 'd')); // false
  })('a','b','c');
  ```



## Array.prototype.indexOf

### 1. 语法

::: danger
arr.indexOf(searchElement[, fromIndex = 0])
:::

参数：

- searchElement：要查找的元素；
- fromIndex：开始查找的位置。

返回值：

首个被找到的元素在数组中的索引位置，若没有找到则返回 -1。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 查找索引，判断元素是否存在。

### 3. 示例

+ finding all the occurrences of an element

  ```js
  let indices = [];
  const array = ['a', 'b', 'a', 'c', 'a', 'd'];
  const element = 'a';
  let idx = array.indexOf(element);
  
  while (idx !== -1) {
    indices.push(idx); // [0, 2, 4]
    idx = array.indexOf(element, idx + 1);
  }
  ```

+ finding if an element exists in the array or not and updating the array

  ```js
  function updateVegetablesCollection(veggies, veggie) {
    if (veggies.indexOf(veggie) === -1) {
      veggies.push(veggie);
      console.log('New veggies collection is : ' + veggies);
    } else if (veggies.indexOf(veggie) > -1) {
  		console.log(veggie + ' already exists in the veggies collection.');
  	}
  }
  
  let veggies = ['potato', 'tomato', 'chillies', 'green-pepper'];
  updateVegetablesCollection(veggies, 'spinach'); 
  // New veggies collection is : potato,tomato,chillies,green-pepper,spinach
  updateVegetablesCollection(veggies, 'spinach'); 
  // spinach already exists in the veggies collection.
  ```



## Array.prototype.join

### 1. 语法

::: danger
arr.join([separator])
:::

参数：

- separator：指定一个字符串来分隔数组的每个元素，默认为 `,`。

返回值：

一个所有数组元素连接的字符串，如果 `arr.length` 为0，则返回空字符串。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 字符串拆分重组。

### 3. 示例

+ joining an array four different ways

  ```js
  const a = ['Wind', 'Water', 'Fire'];
  a.join(); // 'Wind,Water,Fire'
  a.join(', '); // 'Wind, Water, Fire'
  a.join(' + '); // 'Wind + Water + Fire'
  a.join(''); // 'WindWaterFire'
  ```

+ joining an array-like object

  ```js
  function f(a, b, c) {
    return Array.prototype.join.call(arguments);
  }
  f(1, 'a', true); // '1,a,true'
  ```



## Array.prototype.keys

### 1. 语法

::: danger
arr.keys()
:::

返回值：

一个新的 Array 迭代器对象。

### 2. 描述

keys() 方法返回一个包含数组中每个索引键的 Array Iterator 对象。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

+ key iterator doesn't ignore holes

  ```js
  const arr = ['a', , 'c'];
  const sparseKeys = Object.keys(arr); // ['0', '2']
  const denseKeys = [...arr.keys()]; // [0, 1, 2]
  ```



## Array.prototype.lastIndexOf

indexOf 的反向行为，不做描述。



## Array.prototype.map

### 1. 语法

::: danger
arr.map(callback[, thisArg])
:::

参数：

- callback：回调函数；
  - currentValue：当前值；
  - index：当前值的索引；
  - array：当前数组。
- thisArg：执行 callback 时使用的 `this` 值。

返回值：

 一个新的数组，每个元素都是回调函数的结果。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 常用于映射修改后端传递的数据。

### 3. 示例

+ mapping an array of numbers to an array of square roots

  ```js
  const numbers = [1, 4, 9];
  numbers.map(num => Math.sqrt(num)); // [1, 2, 3]
  ```

+ using map to reformat objects in an array

  ```js
  const kvArray = [{key: 1, value: 10}, {key: 2, value: 20}, {key: 3, value: 30}];
  kvArray.map(obj =>  ({[obj.key]: obj.value})); // [{1: 10}, {2: 20}, {3: 30}]
  ```

+ mapping an array of numbers using a function containing an argument

  ```js
  const numbers = [1, 4, 9];
  numbers.map(num => num * 2); // [2, 8, 18]
  ```

+ using map generically

  ```js
  const map = Array.prototype.map;
  map.call('Hello World', x => x.charCodeAt(0)); // [72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100]
  ```

+ using map generically querySelectorAll

  ```js
  const elems = document.querySelectorAll('select option:checked');
  Array.prototype.map.call(elems, obj => obj.value);
  ```

+ tricky use case

  ```js
  ["1", "2", "3"].map(parseInt); // [1, NaN, NaN]
  
  
  function returnInt(element) {
    return parseInt(element, 10);
  }
  ['1', '2', '3'].map(returnInt); // [1, 2, 3]
  ```

+ mapping array have undefined

  ```js
  const numbers = [1, 2, 3, 4];
  numbers.map((num, index) => {
  	if (index < 3) {
  		return num; //  [1, 2, 3, undefined]
  	}
  });
  ```



## Array.prototype.pop

### 1. 语法

::: danger
arr.pop()
:::

返回值：

从数组中删除的元素，当数组为空时返回 undefined。

### 2. 描述

pop() 方法从数组中删除最后一个元素，并返回该元素的值，此方法更改数组的长度。

> 个人在日常开发中常用场景：
>
> - 需要从尾端删除数据时。

### 3. 示例

+ removing the last element of an array

  ```js
  let myFish = ['angel', 'clown', 'mandarin', 'sturgeon'];
  const popped = myFish.pop(); // ['angel', 'clown', 'mandarin' ]  'sturgeon'
  ```

+ using apply() or call() on array-like objects

  ```js
  let myFish = {0:'angel', 1:'clown', 2:'mandarin', 3:'sturgeon', length: 4};
  
  const popped = Array.prototype.pop.call(myFish); // {0:'angel', 1:'clown', 2:'mandarin', length: 3}  'sturgeon'
  ```



## Array.prototype.push

### 1. 语法

::: danger
arr.push(element1, ..., elementN)
:::

参数：

- elementN：被添加到数组末尾的元素。

返回值：

返回新的 length 属性值。

### 2. 描述

push() 方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

> 个人在日常开发中常用场景：
>
> - 需要追加数据时。

### 3. 示例

+ adding elements to an array

  ```js
  let sports = ['soccer', 'baseball'];
  const total = sports.push('football', 'swimming'); // ['soccer', 'baseball', 'football', 'swimming'] 4
  ```

+ merging two arrays

  ```js
  let vegetables = ['parsnip', 'potato'];
  const moreVegs = ['celery', 'beetroot'];
  Array.prototype.push.apply(vegetables, moreVegs); // ['parsnip', 'potato', 'celery', 'beetroot']
  ```

+ using an object in an array-like fashion

  ```js
  let obj = {
    length: 0, // 2
    
    addElem: function (elem) {
      [].push.call(this, elem);
    }
  }
  
  obj.addElem({});
  obj.addElem({});
  ```



## Array.prototype.reduce

### 1. 语法

::: danger
arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
:::

参数：

- callback：回调函数；

  + accumulator：累计器累计回调的返回值；

  - currentValue：当前值；
  - index：当前值的索引；
  - array：当前数组。

- initialValue：作为第一次调用 callback 函数的第一个参数的值，默认值为数组中的第一个元素，空数组会导致报错。

返回值：

函数累计处理的结果。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 累计数据时。

### 3. 示例

+ sum all the values of an array

  ```js
  [0, 1, 2, 3].reduce((accumulator, currentValue) => accumulator + currentValue, 0); // 6
  ```

+ sum of values in an object array

  ```js
  const initialValue = 0;
  [{x: 1}, {x: 2}, {x: 3}].reduce((accumulator, currentValue) => accumulator + currentValue.x, initialValue); // 6
  ```

+ flatten an array of arrays

  ```js
  [[0, 1], [2, 3], [4, 5]].reduce(( accumulator, currentValue ) => accumulator.concat(currentValue), []); // [0, 1, 2, 3, 4, 5]
  ```

+ counting instances of values in an object

  ```js
  const names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];
  names.reduce( (allNames, name) => { 
    if (name in allNames) {
      allNames[name]++;
    }
    else {
      allNames[name] = 1;
    }
    return allNames;
  }, {}); // { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
  ```

+ grouping objects by a property

  ```js
  const people = [
    { name: 'Alice', age: 21 },
    { name: 'Max', age: 20 },
    { name: 'Jane', age: 20 } 
  ];
  
  function groupBy(objectArray, property) {
    return objectArray.reduce((acc, obj) => {
      const key = obj[property];
      if (!acc[key]) {
        acc[key] = [];
      }
      acc[key].push(obj);
      return acc;
    }, {});
  }
  
  groupBy(people, 'age');
  // { 
  //   20: [
  //     { name: 'Max', age: 20 }, 
  //     { name: 'Jane', age: 20 }
  //   ], 
  //   21: [{ name: 'Alice', age: 21 }] 
  // }
  ```

+ bonding arrays contained in an array of objects using the spread operator and initialValue

  ```js
  const friends = [{
    name: 'Anna',
    books: ['Bible', 'Harry Potter'],
    age: 21
  }, {
    name: 'Bob',
    books: ['War and peace', 'Romeo and Juliet'],
    age: 26
  }, {
    name: 'Alice',
    books: ['The Lord of the Rings', 'The Shining'],
    age: 18
  }];
  
  friends.reduce(function(accumulator, currentValue) {
    return [...accumulator, ...currentValue.books];
  }, ['Alphabet']); // ["Alphabet", "Bible", "Harry Potter", "War and peace", "Romeo and Juliet", "The Lord of the Rings", "The Shining"]
  ```

+ remove duplicate items in array

  ```js
  const myArray = ['a', 'b', 'a', 'b', 'c', 'e', 'e', 'c', 'd', 'd', 'd', 'd'];
  myArray.reduce((accumulator, currentValue) => {
    if (accumulator.indexOf(currentValue) === -1) {
      accumulator.push(currentValue);
    }
    return accumulator;
  }, []); // ["a", "b", "c", "e", "d"]
  ```

+ running promises in sequence

  ```js
  function runPromiseInSequence(arr, input) {
    return arr.reduce((promiseChain, currentFunction) => promiseChain.then(currentFunction), Promise.resolve(input));
  }
  
  // promise function 1
  function p1(a) {
    return new Promise((resolve, reject) => {
      resolve(a * 5);
    });
  }
  
  // promise function 2
  function p2(a) {
    return new Promise((resolve, reject) => {
      resolve(a * 2);
    });
  }
  
  // function 3  - will be wrapped in a resolved promise by .then()
  function f3(a) {
   return a * 3;
  }
  
  // promise function 4
  function p4(a) {
    return new Promise((resolve, reject) => {
      resolve(a * 4);
    });
  }
  
  const promiseArr = [p1, p2, f3, p4];
  runPromiseInSequence(promiseArr, 10)
    .then(console.log);   // 1200
  ```

+ function composition enabling piping

  ```js
  const double = x => x + x;
  const triple = x => 3 * x;
  const quadruple = x => 4 * x;
  
  const pipe = (...functions) => input => functions.reduce((acc, fn) => fn(acc), input);
  
  const multiply6 = pipe(double, triple);
  const multiply9 = pipe(triple, triple);
  const multiply16 = pipe(quadruple, quadruple);
  const multiply24 = pipe(double, triple, quadruple);
  
  multiply6(6); // 36
  multiply9(9); // 81
  multiply16(16); // 256
  multiply24(10); // 240
  ```



## Array.prototype.reduceRight

### 1. 语法

::: danger
arr.reduceRight(callback[, initialValue])
:::

参数：

- callback：回调函数；

  - previousValue：上一次调用回调的返回值，或提供的 initialValue；

  - currentValue：当前值；
  - index：当前值的索引；
  - array：当前数组。

- initialValue：作为第一次调用 callback 函数的第一个参数的值，默认值为数组中的第一个元素，空数组会导致报错。

返回值：

函数累计处理的结果。

### 2. 描述

基本与 reduce 一致，从右到左。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

参考 reduce 即可。



## Array.prototype.reverse

### 1. 语法

::: danger
arr.reverse()
:::

返回值：

改变（颠倒）后的数组。

### 2. 描述

reverse() 方法将数组中元素的位置颠倒，并返回该数组。

> 个人在日常开发中常用场景：
>
> - 颠倒数组。

### 3. 示例

+ reversing the elements in an array-like object

  ```js
  const a = {0: 1, 1: 2, 2: 3, length: 3};
  Array.prototype.reverse.call(a); // {0: 3, 1: 2, 2: 1, length: 3}
  ```




## Array.prototype.shift

### 1. 语法

::: danger
arr.shift()
:::

返回值：

从数组中删除的元素，如果数组为空则返回 undefined。 

### 2. 描述

shift 方法移除索引为 0 的元素（即第一个元素），并返回被移除的元素，其他元素的索引值随之减 1（push、pop 操作不会影响其余元素，所以性能更高）。

shift 方法并不限于数组，可以通过 call、apply 方法作用于类数组的对象上。

> 个人在日常开发中常用场景：
>
> - 需要删除前一部分时。

### 3. 示例

```js
let fish = ['angel', 'clown', 'mandarin', 'surgeon'];
fish.shift(); // "angel"
```



## Array.prototype.slice

### 1. 语法

::: danger
arr.slice([begin[, end]])
:::

参数：

+ begin：提取起始处的索引，默认为 0；
+ end：提取终止处的索引，默认为 length-1。

返回值：

一个含有被提取元素的新数组。

### 2. 描述

slice 不会修改原数组，只会返回一个浅拷贝的新数组。

> 个人在日常开发中常用场景：
>
> - 截取内容。

### 3. 示例

```js
const fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
fruits.slice(1, 3); // ['Orange','Lemon']
```



## Array.prototype.some

### 1. 语法

::: danger
arr.some(callback(element[, index[, array]])[, thisArg])
:::

参数：

- callback：回调函数；
  - element：当前元素；
  - index：当前元素的索引；
  - array：当前数组。
- thisArg：执行 callback 时使用的 this 值。

返回值：

如果回调函数返回至少一个数组元素的 truthy 值，则返回 true；否则为 false。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 常用多种元素判断。

### 3. 示例

+ testing array elements using arrow functions

  ```js
  [2, 5, 8, 1, 4].some(x => x > 10); // false
  [12, 5, 8, 1, 4].some(x => x > 10); // true
  ```

+ checking whether a value exists using an arrow function

  ```js
  const fruits = ['apple', 'banana', 'mango', 'guava'];
  function checkAvailability(arr, val) {
    return arr.some(arrVal => val === arrVal);
  }
  
  checkAvailability(fruits, 'kela');   // false
  checkAvailability(fruits, 'banana'); // true
  ```

+ converting any value to Boolean

  ```js
  const TRUTHY_VALUES = [true, 'true', 1];
  
  function getBoolean(value) {
    'use strict';
    
    if (typeof value === 'string') {
      value = value.toLowerCase().trim();
    }
    
    return TRUTHY_VALUES.some(t => t === value);
  }
  
  getBoolean(false);   // false
  getBoolean('false'); // false
  getBoolean(1);       // true
  getBoolean('true');  // true
  ```



## Array.prototype.sort

### 1. 语法

::: danger
arr.sort([compareFunction])
:::

参数：

- compareFunction：用来指定按某种顺序进行排列的函数。
  - firstEl：第一个用于比较的元素；
  - secondEl：第二个用于比较的元素。

返回值：

排序后的数组。

### 2. 描述

sort 方法用[原地算法](https://en.wikipedia.org/wiki/In-place_algorithm)对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较它们的 UTF-16 代码单元值序列时构建的。

> 个人在日常开发中常用场景：
>
> + 排序。

### 3. 示例

+ sorting non-ASCII characters

  ```js
  const items = ['réservé', 'premier', 'cliché', 'communiqué', 'café', 'adieu'];
  items.sort((a, b) => a.localeCompare(b)); // ['adieu', 'café', 'cliché', 'communiqué', 'premier', 'réservé']
  ```

+ sorting with map

  ```js
  const list = ['Delta', 'alpha', 'CHARLIE', 'bravo'];
  let mapped = list.map((el, i) => ({index: i, value: el.toLowerCase()}));
  
  mapped.sort((a, b) => a.value >= b.value ? 1 : -1);
  mapped.map(el => list[el.index]);
  ```



## Array.prototype.splice

### 1. 语法

::: danger
arr.splice(start[, deleteCount[, item1[, item2[, ...]]]])
:::

参数：

- start：指定修改的开始位置（从0计数），如果超出了数组的长度，则从数组末尾开始添加内容；
- deleteCount：整数，表示要移除的数组元素的个数；
- item1, item2, ...：要添加进数组的元素。

返回值：

由被删除的元素组成的一个数组，若没有删除元素，则返回空数组。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 增删改数组中的元素。

### 3. 示例

+ remove 0(zero) elements from index 2, and insert "drum" and "guitar"

  ```js
  const fish =  ['angel', 'clown', 'mandarin', 'sturgeon'];
  fish.splice(2, 0, 'drum', 'guitar');
  ```

+ remove 1 element from index 3

  ```js
  const fish =  ['angel', 'clown', 'mandarin', 'sturgeon'];
  fish.splice(3, 1);
  ```

+ remove 1 element from index 2, and  insert "trumpet"

  ```js
  const fish =  ['angel', 'clown', 'mandarin', 'sturgeon'];
  fish.splice(2, 1, 'trumpet');
  ```

+ remove 2 elements from index 0, and insert "parrot", "anemone" and "blue"

  ```js
  const fish =  ['angel', 'clown', 'mandarin', 'sturgeon'];
  fish.splice(0, 2, 'parrot', 'anemone', 'blue');
  ```

+ remove 1 element from index-2

  ```js
  const fish =  ['angel', 'clown', 'mandarin', 'sturgeon'];
  fish.splice(-2, 1);
  ```

+ remove all elements after index 2

  ```js
  const fish =  ['angel', 'clown', 'mandarin', 'sturgeon'];
  fish.splice(2);
  ```



## Array.prototype.toLocaleString

### 1. 语法

::: danger
arr.toLocaleString([locales[,options]])
:::

参数：

- locales：带有 BCP 47 语言标记的字符串或字符串数组；
- options：一个可配置属性的对象。

返回值：

表示数组元素的字符串。

### 2. 描述

toLocaleString 返回一个字符串表示数组中的元素。数组中的元素将使用各自的 toLocaleString 方法转为字符串，这些字符串将使用一个特定语言环境的字符串隔开。

> 个人在日常开发中常用场景：
>
> - 早期使用过该方法将时间转换为本地时间，后来使用 dayjs，moment 等时间库替代了。

### 3. 示例

```js
// Object：Object.prototype.toLocaleString()
// Number：Number.prototype.toLocaleString()
// Date：Date.prototype.toLocaleString()

let prices = ['￥7', 500, 8123, 12]; 
prices.toLocaleString('ja-JP', { style: 'currency', currency: 'JPY' }); // "￥7,￥500,￥8,123,￥12"
```



## Array.prototype.toString

### 1. 语法

::: danger
arr.toString()
:::

返回值：

一个表示指定的数组及元素的字符串。

### 2. 描述

Array 对象覆盖了 Object 的 toString 方法，当一个数组被作为文本值或者进行字符串连接操作时，将会自动调用其 toString 方法。

> 个人在日常开发中常用场景：
>
> - 常用于数组转为字符串。

### 3. 示例

```js
const array = [1, 2, 'a', '1a'];
arrar.toString(); // "1,2,a,1a"
```



## Array.prototype.unshift

### 1. 语法

::: danger
arr.unshift(element1, ..., elementN)
:::

参数：

- elementN：要添加到数组开头的元素或多个元素。

返回值：

返回其 length 属性值。

### 2. 描述

unshift 方法将一个或多个元素添加到数组的开头，并返回该数组的新长度。

> 个人在日常开发中常用场景：
>
> - 常用于在数组前添加数据。

### 3. 示例

```js
let arr = [1, 2];
arr.unshift(0); // 3
arr.unshift([-4, -3]); // 4
// arr [[-4, -3], 0, 1, 2]
```



## Array.prototype.values

### 1. 语法

::: danger
arr.values()
:::

返回值：

返回一个新的 Array Iterator 对象。

### 2. 描述

描述即返回值。

> 个人在日常开发中常用场景：
>
> - 尚未使用过。

### 3. 示例

+ iteration using for...of loop

  ```js
  const arr = ['a', 'b', 'c'];
  const iterator = arr.values();
  
  for (let letter of iterator) {
    console.log(letter); // "a" "b" "c"
  }
  ```

---
title: Es6数组
cover: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2842498411,1702404695&fm=26&gp=0.jpg
---
# 扩展运算符

扩展运算符是三个点（...）。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。
```javascript
console.log(...[1, 2, 3]) // 1 2 3
console.log(1, ...[2, 3, 4], 5) // 1 2 3 4 5
[...document.querySelectorAll('div')] // [<div>, <div>, <div>]

```
该运算符主要用于函数调用。
```javascript
  function push(array, ...items) {
    array.push(...items);
  }
  function add(x, y) {
    return x + y;
  }
  var numbers = [4, 38];
  add(...numbers) // 42
```
扩展运算符与正常的函数参数可以结合使用。
```javascript
  function f(v, w, x, y, z) { }
  var args = [0, 1];
  f(-1, ...args, 2, ...[3]);

```
扩展运算符后面还可以放置表达式。
```javascript
const arr = [
  ...(x > 0 ? ['a'] : []),
  'b',
];
```
如果扩展运算符后面是一个空数组，则不产生任何效果。
```javascript
[...[], 1]   // [1]

```
# 替代函数的apply方法
由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了。
```javascript
 //ES5 的写法
  function f(x, y, z) {
    // ...
  }
  var args = [0, 1, 2];
  f.apply(null, args);
  // ES6的写法
  function f(x, y, z) {
    // ...
  }
  var args = [0, 1, 2];
  f(...args);
```
下面是扩展运算符取代apply方法的一个例子。
```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77])
// ES6 的写法
Math.max(...[14, 3, 77])
// 等同于
Math.max(14, 3, 77);
```
上面代码中，由于`JavaScript`不提供求数组最大元素的函数，所以只能套用`Math.max`函数，将数组转为一个参数序列，然后求最大值。有了扩展运算符以后，就可以直接用Math.max了。
另一个例子是通过push函数，将一个数组添加到另一个数组的尾部。
```javascript
// ES5的写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);
// ES6 的写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1.push(...arr2);
```
上面代码的ES5写法中，push方法的参数不能是数组，所以只好通过apply方法变通使用push方法。有了扩展运算符，就可以直接将数组传入push方法。
下面是另外一个例子。
```javascript
// ES5
new (Date.bind.apply(Date, [null, 2015, 1, 1]))
// ES6
new Date(...[2015, 1, 1]);
```
# 扩展运算符的应用
# (1)复制数组
数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。
```javascript
const a1 = [1, 2];
const a2 = a1;
a2[0] = 2;
a1 // [2, 2]
```
上面代码中，a2并不是a1的克隆，而是指向同一份数据的另一个指针。修改a2，会直接导致a1的变化。
ES5只能用变通方法来复制数组。
```javascript
const a1 = [1, 2];
const a2 = a1.concat();
a2[0] = 2;
a1 // [1, 2]
```
上面代码中，a1会返回原数组的克隆，再修改a2就不会对a1产生影响。
扩展运算符提供了复制数组的简便写法。
```javascript
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```
上面的两种写法，a2都是a1的克隆。
# (2)合并数组
```javascript
// ES5
[1, 2].concat(more)
// ES6
[1, 2, ...more]
var arr1 = ['a', 'b'];
var arr2 = ['c'];
var arr3 = ['d', 'e'];
// ES5的合并数组
arr1.concat(arr2, arr3); // [ 'a', 'b', 'c', 'd', 'e' ]
// ES6的合并数组
[...arr1, ...arr2, ...arr3] // [ 'a', 'b', 'c', 'd', 'e' ]

```
不过，这两种方法都是浅拷贝，使用的时候需要注意。
```javascript
const a1 = [{ foo: 1 }];
const a2 = [{ bar: 2 }];
const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];
a3[0] === a1[0] // true
a4[0] === a1[0] // true
```
上面代码中，a3和a4是用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是浅拷贝。如果修改了原数组的成员，会同步反映到新数组。
# (3)与解构赋值结合

扩展运算符可以与解构赋值结合起来，用于生成数组。
```javascript
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list
```
下面是另外一些例子。
```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
const [first, ...rest] = [];
first // undefined
rest  // []
const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```
如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
```javascript
const [...butLast, last] = [1, 2, 3, 4, 5]; // 报错
const [first, ...middle, last] = [1, 2, 3, 4, 5]; // 报错

```
# (4)字符串
扩展运算符还可以将字符串转为真正的数组。
```javascript
[...'hello'] // [ "h", "e", "l", "l", "o" ]

```
# (5)Map和Set结构,Generator函数
扩展运算符内部调用的是数据结构的Iterator接口，因此只要具有Iterator接口的对象，都可以使用扩展运算符，比如Map结构。

```javascript

  let map = new Map([
    [1, 'one'],
    [2, 'two'],
     [3, 'three'],
]);
let arr = [...map.keys()]; // [1, 2, 3]

  ```
  Generator函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。
  ```javascript
  const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};
[...go()] // [1, 2, 3]

  ```
  上面代码中，变量`go`是一个Generator函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。
如果对没有`Iterator`接口的对象，使用扩展运算符，将会报错。
# Array.from()
`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。
下面是一个类似数组的对象，Array.from将它转为真正的数组。
```javascript
let 
arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

```
实际应用中，常见的类似数组的对象是`DOM`操作返回的`NodeList`集合，以及函数内部的`arguments`对象。Array.from都可以将它们转为真正的数组。
```javascript
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});
// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```
上面代码中，`querySelectorAll`方法返回的是一个类似数组的对象，可以将这个对象转为真正的数组，再使用filter方法。
只要是部署了`Iterator`接口的数据结构，Array.from都能将其转为数组。
```javascript
Array.from('hello') // ['h', 'e', 'l', 'l', 'o']
let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```
上面代码中，字符串和`Set`结构都具有`Iterator`接口，因此可以被Array.from转为真正的数组。
如果参数是一个真正的数组，Array.from会返回一个一模一样的新数组。
值得提醒的是，扩展运算符（...）也可以将某些数据结构转为数组。
```javascript
// arguments对象
function foo() {
  const args = [...arguments];
}
// NodeList对象
[...document.querySelectorAll('div')]

```
扩展运算符背后调用的是遍历器接口，如果一个对象没有部署这个接口，就无法转换。Array.from方法还支持类似数组的对象。所谓类似数组的对象，本质特征只有一点，即必须有length属性。因此，任何有length属性的对象，都可以通过Array.from方法转为数组，而此时扩展运算符就无法转换。
```javascript
Array.from({ length: 3 }); // [ undefined, undefined, undefined ]

```
上面代码中，`Array.from`返回了一个具有三个成员的数组，每个位置的值都是`undefined`。扩展运算符转换不了这个对象。
对于还没有部署该方法的浏览器，可以用`Array.prototype.slice`方法替代。
```javascript
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```
`Array.from`还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
```javascript
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);
Array.from([1, 2, 3], (x) => x * x) // [1, 4, 9]

```
下面的例子是取出一组DOM节点的文本内容。
```javascript
let spans = document.querySelectorAll('span.name');
// map()
let names1 = Array.prototype.map.call(spans, s => s.textContent);
// Array.from()
let names2 = Array.from(spans, s => s.textContent)
```
下面的例子将数组中布尔值为false的成员转为0。
```javascript

Array.from([1, , 2, , 3], (n) => n || 0) // [1, 0, 2, 0, 3]

```
# Array.of()
`Array.of`方法用于将一组值，转换为数组。
```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```
这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。
```javascript
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]

```
上面代码中，`Array`方法没有参数、一个参数、三个参数时，返回结果都不一样。只有当参数个数不少于2个时，`Array()`才会返回由参数组成的新数组。参数个数只有一个时，实际上是指定数组的长度。
`Array.of`基本上可以用来替代`Array()`或`new Array()`，并且不存在由于参数不同而导致的重载。它的行为非常统一。
```javascript
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(1) // [1]
Array.of(1, 2) // [1, 2]

```
Array.of总是返回参数值组成的数组。如果没有参数，就返回一个空数组。
Array.of方法可以用下面的代码模拟实现。
```javascript
function ArrayOf(){
  return [].slice.call(arguments);
}
```
# 数组实例的find()和findIndex()
数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。
```javascript
[1, 4, -5, 10].find((n) => n < 0) // -5
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```
上面代码中，`find`方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。
数组实例的`findIndex`方法的用法与 `find ` 方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。
```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```
这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。
```javascript
function f(v) {
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person); // 26

```
另外，这两个方法都可以发现NaN，弥补了数组的indexOf方法的不足。

```javascript
[NaN].indexOf(NaN) // -1
[NaN].findIndex(y => Object.is(NaN, y)) // 0

```
上面代码中，indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到。
# 数组实例的entries(),keys()和values()
`entries()`，`keys()`和`values()`方法用于遍历数组。它们都返回一个遍历器对象，可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。
```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0 1
for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a' 'b'
for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```
如果不使用`for...of`循环，可以手动调用遍历器对象的`next`方法，进行遍历。
```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']

```

# 数组实例的includes()
`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。
```javascript
[1, 2, 3].includes(2) // true
[1, 2, 3].includes(4) // false
[1, 2, NaN].includes(NaN) // true

```
该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。

```javascript
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true

```
没有该方法之前，我们通常使用数组的indexOf方法，检查是否包含某个值。
```javascript
if (arr.indexOf(el) !== -1) {
  // ...
}
```
`indexOf`方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是它内部使用严格相等运算符(===)进行判断，这会导致对NaN的误判。
```javascript
[NaN].indexOf(NaN) // -1

```
`includes`使用的是不一样的判断算法，就没有这个问题。
```javascript
[NaN].includes(NaN) // true

```
下面代码用来检查当前环境是否支持该方法，如果不支持，部署一个简易的替代版本。
```javascript
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(['foo', 'bar'], 'baz'); // => false
```
另外，`Map`和`Set`数据结构有一个`has`方法，需要注意与`includes`区分。
`Map`结构的`has`方法，是用来查找键名的，比如`Map.prototype.has(key)`、`WeakMap.prototype.has(key)`、`Reflect.has(target, propertyKey)`。
`Set`结构的`has`方法，是用来查找值的，比如`Set.prototype.has(value)`、`WeakSet.prototype.has(value)`。
# 数组的空位
数组的空位指，数组的某一个位置没有任何值。比如，Array构造函数返回的数组都是空位。
```javascript
Array(3) // [, , ,]

```
注意，空位不是`undefined`，一个位置的值等于`undefined`，依然是有值的。空位是没有任何值，in运算符可以说明这一点。
```javascript
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```
上面代码说明，第一个数组的`0`号位置是有值的，第二个数组的`0`号位置没有值。
`ES5`对空位的处理，已经很不一致了，大多数情况下会忽略空位。
`forEach()`、`filter()`、`reduce()`、`every()`和`some()`都会跳过空位。
`map()`会跳过空位，但会保留这个值
`join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。
```javascript
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1
// filter方法
['a',,'b'].filter(x => true) // ['a','b']
// every方法
[,'a'].every(x => x==='a') // true
// reduce方法
[1,,2].reduce((x,y) => return x+y) // 3
// some方法
[,'a'].some(x => x !== 'a') // false
// map方法
[,'a'].map(x => 1) // [,1]
// join方法
[,'a', undefined, null].join('#') // "#a##"
// toString方法
[,'a', undefined, null].toString() // ",a,,"

```
`ES6` 则是明确将空位转为`undefined`。
`Array.from`方法会将数组的空位，转为`undefined`，也就是说，这个方法不会忽略空位。
```javascript
Array.from(['a',,'b']) // [ "a", undefined, "b" ]

```

扩展运算符(…)也会将空位转为undefined。
```javascript
[...['a',,'b']] // [ "a", undefined, "b" ]

```
`copyWithin()`会连空位一起拷贝。
```javascript
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
```
`for...of`循环也会遍历空位。
```javascript
let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1
```
上面代码中，数组`arr`有两个空位，`for...of`并没有忽略它们。如果改成`map`方法遍历，空位是会跳过的。
`entries()`、`keys()`、`values()`、`find()`和`findIndex()`会将空位处理成`undefined`。
```javascript
// entries()
[...[,'a'].entries()] // [[0, undefined], [1, "a"]]
// keys()
[...[,'a'].keys()] // [0,1]
// values()
[...[,'a'].values()] // [undefined, "a"]
// find()
[,'a'].find(x => true) // undefined
// findIndex()
[,'a'].findIndex(x => true) // 0
```
# fill

```javascript
console.log('fill-7',[1,'a',undefined].fill(7))  //[7,7,7]
```

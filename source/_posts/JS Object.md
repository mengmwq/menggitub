---
title: JS Object 对象
cover: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=4081892624,1811108460&fm=15&gp=0.jpg
---

JavaScript 原生提供`Object`对象。
JavaScript 的所有其他对象都继承自`Object`对象，即那些对象都是`Object`的实例。

`Object`对象的原生方法分成两类：`Object`本身的方法与`Object`的实例方法。

object对象本身的方法
所谓本身的方法就是直接定义在object对象的方法上
```javascript
object.print = function(0) {console.log(0)}
```
上面代码中，`print`方法就是直接定义在`Object`对象上。
# object的实例方法
所谓实例方法就是定义在object原型对象object。prototype上的方法，它可以被object实例直接使用
```javascript
object.prototype.print =function(){
console.log(this)
}
var obj = new Object()
obj.print() //object
```
上面代码中，```Object.prototype```定义了一个print方法，然后生成一个```Object```的实例```obj```。```obj```直接继承了```Object.prototype```的属性和方法，可以直接使用```obj.print```调用```print```方法。也就是说，obj对象的```print```方法实质上就是调用```Object.prototype.print```方法。

凡是定义在```Object.prototype```对象上面的属性和方法，将被所有实例对象共享。
# Object()

`Object`本身是一个函数，可以当作工具方法使用，将任意值转为对象。这个方法常用于保证某个值一定是对象。

如果参数为空（或者为undefined和null），Object()返回一个空对象。
```javascript
var obj = Object();
// 等同于
var obj = Object(undefined);
var obj = Object(null);

obj instanceof Object // true

```
上面代码的含义，是将undefined和null转为对象，结果得到了一个空对象obj。

instanceof运算符用来验证，一个对象是否为指定的构造函数的实例。obj instanceof Object返回true，就表示obj对象是Object的实例。

如果参数是原始类型的值，Object方法将其转为对应的包装对象的实例。
```javascript
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

var obj = Object('foo');
obj instanceof Object // true
obj instanceof String // true

var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true

```
面代码中，Object函数的参数是各种原始类型的值，转换成对象就是原始类型值对应的包装对象。

如果Object方法的参数是一个对象，它总是返回该对象，即不用转换。
```javascript
var arr = [];
var obj = Object(arr); // 返回原数组
obj === arr // true

var value = {};
var obj = Object(value) // 返回原对象
obj === value // true

var fn = function () {};
var obj = Object(fn); // 返回原函数
obj === fn // true
```
利用这一点，可以写一个判断变量是否为对象的函数。
```javascript
function isObject(value) {
  return value === Object(value);
}

isObject([]) // true
isObject(true) // false
```
# Object 构造函数
Object不仅可以当作工具函数使用，还可以当作构造函数使用，即前面可以使用new命令。

Object构造函数的首要用途，是直接通过它来生成新对象。
```javascript

var obj = new Object();

```
注意，通过var obj = new Object()的写法生成新对象，与字面量的写法var obj = {}是等价的。或者说，后者只是前者的一种简便写法。

Object构造函数的用法与工具方法很相似，几乎一模一样。使用时，可以接受一个参数，如果该参数是一个对象，则直接返回这个对象；如果是一个原始类型的值，则返回该值对应的包装对象。
```javascript
var o1 = {a: 1};
var o2 = new Object(o1);
o1 === o2 // true

var obj = new Object(123);
obj instanceof Number // true

```
虽然用法相似，但是Object(value)与new Object(value)两者的语义是不同的，Object(value)表示将value转成一个对象，new Object(value)则表示新生成一个对象，它的值是value。
# Object 的静态方法
所谓“静态方法”，是指部署在Object对象自身的方法。
# Object.keys()，Object.getOwnPropertyNames()
Object.keys方法和Object.getOwnPropertyNames方法都用来遍历对象的属性。

Object.keys方法的参数是一个对象，返回一个数组。该数组的成员都是该对象自身的（而不是继承的）所有属性名。
```javascript
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj) // ["p1", "p2"]

```
Object.getOwnPropertyNames方法与Object.keys类似，也是接受一个对象作为参数，返回一个数组，包含了该对象自身的所有属性名。
```javascript
var obj = {
  p1: 123,
  p2: 456
};

Object.getOwnPropertyNames(obj) // ["p1", "p2"]

```
对于一般的对象来说，Object.keys()和Object.getOwnPropertyNames()返回的结果是一样的。只有涉及不可枚举属性时，才会有不一样的结果。Object.keys方法只返回可枚举的属性，Object.getOwnPropertyNames方法还返回不可枚举的属性名。
```javascript
var a = ['Hello', 'World'];

Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]

```
上面代码中，数组的length属性是不可枚举的属性，所以只出现在Object.getOwnPropertyNames方法的返回结果中。

由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。
```javascript
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj).length // 2
Object.getOwnPropertyNames(obj).length // 2

```
一般情况下，几乎总是使用Object.keys方法，遍历对象的属性。
# 其他方法
除了上面提到的两个方法，Object还有不少其他静态方法。
1.对象属性模型的相关方法
```Object.getOwnPropertyDescriptor()```：获取某个属性的描述对象。
```Object.defineProperty()```：通过描述对象，定义某个属性。
```Object.defineProperties()```：通过描述对象，定义多个属性。
2.控制对象状态的方法
```Object.preventExtensions()```：防止对象扩展。
```Object.isExtensible()```：判断对象是否可扩展。
```Object.seal()```：禁止对象配置。
```Object.isSealed()```：判断一个对象是否可配置。
```Object.freeze()```：冻结一个对象。
```Object.isFrozen()```：判断一个对象是否被冻结。
3.原型链相关方法
```Object.create()```：该方法可以指定原型对象和属性，返回一个新的对象。
```Object.getPrototypeOf()```：获取对象的Prototype对象。

# Object 的实例方法
除了静态方法，还有不少方法定义在Object.prototype对象。它们称为实例方法，所有Object的实例对象都继承了这些方法。

Object实例对象的方法，主要有以下六个。

```Object.prototype.valueOf()````：返回当前对象对应的值。
```Object.prototype.toString()```：返回当前对象对应的字符串形式。
```Object.prototype.toLocaleString()```：返回当前对象对应的本地字符串形式。
```Object.prototype.hasOwnProperty()```：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。
```Object.prototype.isPrototypeOf()```：判断当前对象是否为另一个对象的原型。
```Object.prototype.propertyIsEnumerable()```：判断某个属性是否可枚举。
```Object.create()```：该方法可以指定原型对象和属性，返回一个新的对象。
```Object.getPrototypeOf()```：获取对象的Prototype对象。
# Object 的实例方法

# Object.prototype.valueOf()
valueOf方法的作用是返回一个对象的“值”，默认情况下返回对象本身。
```javascript

var obj = new Object();
obj.valueOf() === obj // true

```
valueOf方法的主要用途是，JavaScript 自动类型转换时会默认调用这个方法。
```javascript
var obj = new Object();
1 + obj // "1[object Object]"

```
上面代码将对象obj与数字1相加，这时 JavaScript 就会默认调用valueOf()方法，求出obj的值再与1相加。所以，如果自定义valueOf方法，就可以得到想要的结果。
```javascript
var obj = new Object();
obj.valueOf = function () {
  return 2;
};

1 + obj // 3
```
上面代码自定义了obj对象的valueOf方法，于是1 + obj就得到了3。这种方法就相当于用自定义的obj.valueOf，覆盖Object.prototype.valueOf。

# Object.prototype.toString()
toString方法的作用是返回一个对象的字符串形式，默认情况下返回类型字符串。
```javascript
var o1 = new Object();
o1.toString() // "[object Object]"

var o2 = {a:1};
o2.toString() // "[object Object]"

```
上面代码表示，对于一个对象调用toString方法，会返回字符串[object Object]，该字符串说明对象的类型。

字符串[object Object]本身没有太大的用处，但是通过自定义toString方法，可以让对象在自动类型转换时，得到想要的字符串形式。
```javascript
var obj = new Object();

obj.toString = function () {
  return 'hello';
};

obj + ' ' + 'world' // "hello world"

```
上面代码表示，当对象用于字符串加法时，会自动调用toString方法。由于自定义了toString方法，所以返回字符串hello world。

数组、字符串、函数、Date对象都分别部署了自定义的toString方法，覆盖了Object.prototype.toString方法。
```javascript
[1, 2, 3].toString() // "1,2,3"

'123'.toString() // "123"

(function () {
  return 123;
}).toString()
// "function () {
//   return 123;
// }"

(new Date()).toString()
// "Tue May 10 2016 09:11:31 GMT+0800 (CST)"

```
上面代码中，数组、字符串、函数、Date对象调用toString方法，并不会返回[object Object]，因为它们都自定义了toString方法，覆盖原始方法。
#toString() 的应用：判断数据类型
Object.prototype.toString方法返回对象的类型字符串，因此可以用来判断一个值的类型。
```javascript
var obj = {};
obj.toString() // "[object Object]"

```
上面代码调用空对象的toString方法，结果返回一个字符串object Object，其中第二个Object表示该值的构造函数。这是一个十分有用的判断数据类型的方法。

由于实例对象可能会自定义toString方法，覆盖掉Object.prototype.toString方法，所以为了得到类型字符串，最好直接使用Object.prototype.toString方法。通过函数的call方法，可以在任意值上调用这个方法，帮助我们判断这个值的类型。
```javascript
Object.prototype.toString.call(value)

```
上面代码表示对value这个值调用Object.prototype.toString方法。

不同数据类型的Object.prototype.toString方法返回值如下。
数值：返回```[object Number]```。
字符串：返回```[object String]```。
布尔值：返回```[object Boolean]```。
undefined：返回```[object Undefined```]。
null：返回```[object Null]```。
数组：返回```[object Array]```。
arguments对象：返回```[object Arguments]```。
函数：返回```[object Function]```。
Error对象：返回```[object Error]```。
Date对象：返回```[object Date]```。
RegExp对象：返回```[object RegExp]```。
其他对象：返回```[object Object]```。
这就是说，Object.prototype.toString可以看出一个值到底是什么类型。
```javascript
Object.prototype.toString.call(2) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(true) // "[object Boolean]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(Math) // "[object Math]"
Object.prototype.toString.call({}) // "[object Object]"
Object.prototype.toString.call([]) // "[object Array]"
```
利用这个特性，可以写出一个比typeof运算符更准确的类型判断函数。
```javascript
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};

type({}); // "object"
type([]); // "array"
type(5); // "number"
type(null); // "null"
type(); // "undefined"
type(/abcd/); // "regex"
type(new Date()); // "date"
```
在上面这个type函数的基础上，还可以加上专门判断某种类型数据的方法。
```javascript
var type = function (o){
  var s = Object.prototype.toString.call(o);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};

['Null',
 'Undefined',
 'Object',
 'Array',
 'String',
 'Number',
 'Boolean',
 'Function',
 'RegExp'
].forEach(function (t) {
  type['is' + t] = function (o) {
    return type(o) === t.toLowerCase();
  };
});

type.isObject({}) // true
type.isNumber(NaN) // true
type.isRegExp(/abc/) // true
```
# Object.prototype.toLocaleString()
Object.prototype.toLocaleString方法与toString的返回结果相同，也是返回一个值的字符串形式。
```javascript
var obj = {};
obj.toString(obj) // "[object Object]"
obj.toLocaleString(obj) // "[object Object]"

```
这个方法的主要作用是留出一个接口，让各种不同的对象实现自己版本的toLocaleString，用来返回针对某些地域的特定的值

```javascript
var person = {
  toString: function () {
    return 'Henry Norman Bethune';
  },
  toLocaleString: function () {
    return '白求恩';
  }
};

person.toString() // Henry Norman Bethune
person.toLocaleString() // 白求恩
```
上面代码中，toString()方法返回对象的一般字符串形式，toLocaleString()方法返回本地的字符串形式。

目前，主要有三个对象自定义了toLocaleString方法。

Array.prototype.toLocaleString()
Number.prototype.toLocaleString()
Date.prototype.toLocaleString()
举例来说，日期的实例对象的toString和toLocaleString返回值就不一样，而且toLocaleString的返回值跟用户设定的所在地域相关。
```javascript
var date = new Date();
date.toString() // "Tue Jan 01 2018 12:01:33 GMT+0800 (CST)"
date.toLocaleString() // "1/01/2018, 12:01:33 PM"
```
#Object.prototype.hasOwnProperty()
Object.prototype.hasOwnProperty方法接受一个字符串作为参数，返回一个布尔值，表示该实例对象自身是否具有该属性。
```javascript
var obj = {
  p: 123
};

obj.hasOwnProperty('p') // true
obj.hasOwnProperty('toString') // false

```
上面代码中，对象obj自身具有p属性，所以返回true。toString属性是继承的，所以返回false。
# 实例
```javascript

  var archivedList = [{
    time: '2020-11-27'
  }, {
    time: '2020-11-15'
  }, {
    time: '2020-10-15'
  }, {
    time: '2020-9-20'
  }]

  var newMyList = {};

  archivedList.forEach((item) => {
    const time = item.time.split('-');
    console.log(time)

    const b_time = `${time[0]}-${time[1]}`;
    console.log(newMyList.hasOwnProperty(b_time));
    console.log(b_time)
    if (!newMyList[b_time]) {
      newMyList[b_time] = { ym: b_time };
      newMyList[b_time].children = [];
    }
    newMyList[b_time].children.push({
      d: time[3]
    })

  })

  console.log(Object.values(newMyList));
```
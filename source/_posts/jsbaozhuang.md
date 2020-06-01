---
title: JS包装对象
cover : https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1497111760,1419881157&fm=26&gp=0.jpg
---
# 包装对象

对象是 JavaScript 语言最主要的数据类型，三种原始类型的值——数值、字符串、布尔值——在一定条件下，也会自动转为对象，也就是原始类型的“包装对象”。

所谓“包装对象”，指的是与数值、字符串、布尔值分别相对应的```Number```、```String```、```Boolean```三个原生对象。这三个原生对象可以把原始类型的值变成（包装成）对象。、

```javascript
var v1 = new Number(123);
var v2 = new String('abc');
var v3 = new Boolean(true);

typeof v1 // "object"
typeof v2 // "object"
typeof v3 // "object"

v1 === 123 // false
v2 === 'abc' // false
v3 === true // false
```
上面代码中，基于原始类型的值，生成了三个对应的包装对象。可以看到，v1、v2、v3都是对象，且与对应的简单类型值不相等。

包装对象的设计目的，首先是使得“对象”这种类型可以覆盖 JavaScript 所有的值，整门语言有一个通用的数据模型，其次是使得原始类型的值也有办法调用自己的方法。
`Number`、`String`和`Boolean`这三个原生对象，如果不作为构造函数调用（即调用时不加new），而是作为普通函数调用，常常用于将任意类型的值转为数值、字符串和布尔值。

```javascript
/ 字符串转为数值
Number('123') // 123

// 数值转为字符串
String(123) // "123"

// 数值转为布尔值
Boolean(123) // true

```
总结一下，这三个对象作为构造函数使用（带有new）时，可以将原始类型的值转为对象；作为普通函数使用时（不带有new），可以将任意类型的值，转为原始类型的值。

# 实例方法

三种包装对象各自提供了许多实例方法。这里介绍两种它们共同具有、从Object对象继承的方法：valueOf()和toString()。

**valueOf()**

valueOf()方法返回包装对象实例对应的原始类型的值。

```javascript
new Number(123).valueOf()  // 123
new String('abc').valueOf() // "abc"
new Boolean(true).valueOf() // true

```
**toString()**
toString()方法返回对应的字符串形式。

```javascript
new Number(123).toString() // "123"
new String('abc').toString() // "abc"
new Boolean(true).toString() // "true"
```
# 原始类型与实例对象的自动转换

某些场合，原始类型的值会自动当作包装对象调用，即调用包装对象的属性和方法。这时，JavaScript 引擎会自动将原始类型的值转为包装对象实例，并在使用后立刻销毁实例。

比如，字符串可以调用length属性，返回字符串的长度。

```javascript
'abc'.length // 3

```

上面代码中，abc是一个字符串，本身不是对象，不能调用length属性。JavaScript 引擎自动将其转为包装对象，在这个对象上调用length属性。调用结束后，这个临时对象就会被销毁。这就叫原始类型与实例对象的自动转换。

```javascript
var str = 'abc';
str.length // 3

// 等同于
var strObj = new String(str)
// String {
//   0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"
// }
strObj.length // 3
```
自动转换生成的包装对象是只读的，无法修改。所以，字符串无法添加新属性。

```javascript
var s = 'Hello World';
s.x = 123;
s.x // undefined
```
上面代码为字符串s添加了一个x属性，结果无效，总是返回undefined。

另一方面，调用结束后，包装对象实例会自动销毁。这意味着，下一次调用字符串的属性时，实际是调用一个新生成的对象，而不是上一次调用时生成的那个对象，所以取不到赋值在上一个对象的属性。如果要为字符串添加属性，只有在它的原型对象String.prototype上定义。
#自定义方法
除了原生的实例方法，包装对象还可以自定义方法和属性，供原始类型的值直接调用。

比如，我们可以新增一个`double`方法，使得字符串和数字翻倍。

```javascript
String.prototype.double = function () {
  return this.valueOf() + this.valueOf();
};

'abc'.double() // abcabc

Number.prototype.double = function () {
  return this.valueOf() + this.valueOf();
};

(123).double() // 246
```
上面代码在String和Number这两个对象的原型上面，分别自定义了一个方法，从而可以在所有实例对象上调用。注意，最后的123外面必须要加上圆括号，否则后面的点运算符（.）会被解释成小数点。
#Boolean 对象

Boolean对象是 JavaScript 的三个包装对象之一。作为构造函数，它主要用于生成布尔值的包装对象实例。
```javascript
var b = new Boolean(true);

typeof b // "object"
b.valueOf() // true
```
上面代码的变量b是一个Boolean对象的实例，它的类型是对象，值为布尔值true。
注意，false对应的包装对象实例，布尔运算结果也是true
```javascript
if (new Boolean(false)) {
  console.log('true');
} // true

if (new Boolean(false).valueOf()) {
  console.log('true');
} // 无输出
```
上面代码的第一个例子之所以得到true，是因为false对应的包装对象实例是一个对象，进行逻辑运算时，被自动转化成布尔值true（因为所有对象对应的布尔值都是true）。而实例的valueOf方法，则返回实例对应的原始值，本例为false。
#Boolean 函数的类型转换作用
Boolean对象除了可以作为构造函数，还可以单独使用，将任意值转为布尔值。这时Boolean就是一个单纯的工具方法
```javascript
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean('') // false
Boolean(NaN) // false

Boolean(1) // true
Boolean('false') // true
Boolean([]) // true
Boolean({}) // true
Boolean(function () {}) // true
Boolean(/foo/) // true

```
上面代码中几种得到true的情况，都值得认真记住。
顺便提一下，使用双重的否运算符（!）也可以将任意值转为对应的布尔值。
```javascript
!!undefined // false
!!null // false
!!0 // false
!!'' // false
!!NaN // false

!!1 // true
!!'false' // true
!![] // true
!!{} // true
!!function(){} // true
!!/foo/ // true
```
最后，对于一些特殊值，Boolean对象前面加不加new，会得到完全相反的结果，必须小心。
```javascript
if (Boolean(false)) {
  console.log('true');
} // 无输出

if (new Boolean(false)) {
  console.log('true');
} // true

if (Boolean(null)) {
  console.log('true');
} // 无输出

if (new Boolean(null)) {
  console.log('true');
} // true
```

# Number 对象

Number对象是数值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。
作为工具函数时，它可以将任何类型的值转为数值。
```javascript

Number(true) // 1

```
# 静态属性

Number对象拥有以下一些静态属性（即直接定义在Number对象上的属性，而不是定义在实例上的属性）。

Number.POSITIVE_INFINITY：正的无限，指向Infinity。
Number.NEGATIVE_INFINITY：负的无限，指向-Infinity。
Number.NaN：表示非数值，指向NaN。
Number.MIN_VALUE：表示最小的正数（即最接近0的正数，在64位浮点数体系中为5e-324），相应的，最接近0的负数为-Number.MIN_VALUE。
Number.MAX_SAFE_INTEGER：表示能够精确表示的最大整数，即9007199254740991。
Number.MIN_SAFE_INTEGER：表示能够精确表示的最小整数，即-9007199254740991。

```javascript


Number.POSITIVE_INFINITY // Infinity
Number.NEGATIVE_INFINITY // -Infinity
Number.NaN // NaN

Number.MAX_VALUE // 1.7976931348623157e+308
Number.MAX_VALUE < Infinity // true

Number.MIN_VALUE // 5e-324
Number.MIN_VALUE > 0 // true

Number.MAX_SAFE_INTEGER // 9007199254740991
Number.MIN_SAFE_INTEGER // -9007199254740991

```
# 实例方法
Number对象有4个实例方法，都跟将数值转换成指定格式有关。
Number对象部署了自己的toString方法，用来将一个数值转为字符串形式。
```javascript
(10).toString() // "10"

```
`toString`方法可以接受一个参数，表示输出的进制。如果省略这个参数，默认将数值先转为十进制，再输出字符串；否则，就根据参数指定的进制，将一个数字转化成某个进制的字符串。
```javascript

(10).toString(2) // "1010"
(10).toString(8) // "12"
(10).toString(16) // "a"

```
这实际上意味着，可以直接对一个小数使用toString方法。
```javascript
10.5.toString() // "10.5"
10.5.toString(2) // "1010.1"
10.5.toString(8) // "12.4"
10.5.toString(16) // "a.8"
```
通过方括号运算符也可以调用toString方法。
```javascript
10['toString'](2) // "1010"
```
toString方法只能将十进制的数，转为其他进制的字符串。如果要将其他进制的数，转回十进制，需要使用parseInt方法。
# Number.prototype.toFixed()
toFixed()方法先将一个数转为指定位数的小数，然后返回这个小数对应的字符串。
```javascript
(10).toFixed(2) // "10.00"
10.005.toFixed(2) // "10.01"

```
上面代码中，10和10.005先转成2位小数，然后转成字符串。其中10必须放在括号里，否则后面的点会被处理成小数点。
toFixed()方法的参数为小数位数，有效范围为0到20，超出这个范围将抛出RangeError错误。

由于浮点数的原因，小数5的四舍五入是不确定的，使用的时候必须小心。
```javascript
(10.055).toFixed(2) // 10.05
(10.005).toFixed(2) // 10.01

```
#自定义方法
与其他对象一样，Number.prototype对象上面可以自定义方法，被Number的实例继承。
```javascript
Number.prototype.add = function (x) {
  return this + x;
};

8['add'](2) // 10
```
上面代码为Number对象实例定义了一个add方法。在数值上调用某个方法，数值会自动转为Number的实例对象，所以就可以调用add方法了。由于add方法返回的还是数值，所以可以链式运算。
```javascript
Number.prototype.subtract = function (x) {
  return this - x;
};

(8).add(2).subtract(4) // 6
```
我们还可以部署更复杂的方法。
```javascript
Number.prototype.iterate = function () {
  var result = [];
  for (var i = 0; i <= this; i++) {
    result.push(i);
  }
  return result;
};

(8).iterate() // [0, 1, 2, 3, 4, 5, 6, 7, 8]

```
面代码在Number对象的原型上部署了iterate方法，将一个数值自动遍历为一个数组。

注意，数值的自定义方法，只能定义在它的原型对象Number.prototype上面，数值本身是无法自定义属性的。

```javascript

var n = 1;
n.x = 1;
n.x // undefined

```
上面代码中，n是一个原始类型的数值。直接在它上面新增一个属性x，不会报错，但毫无作用，总是返回undefined。这是因为一旦被调用属性，n就自动转为Number的实例对象，调用结束后，该对象自动销毁。所以，下一次调用n的属性时，实际取到的是另一个对象，属性x当然就读不出来。
#String 对象
String对象是 JavaScript 原生提供的三个包装对象之一，用来生成字符串对象。

```javascript

var s1 = 'abc';
var s2 = new String('abc');

typeof s1 // "string"
typeof s2 // "object"

s2.valueOf() // "abc"
```
字符串对象是一个类似数组的对象（很像数组，但不是数组）。
```javascript
new String('abc')
// String {0: "a", 1: "b", 2: "c", length: 3}

(new String('abc'))[1] // "b"
```
上面代码中，字符串abc对应的字符串对象，有数值键（0、1、2）和length属性，所以可以像数组那样取值。

除了用作构造函数，String对象还可以当作工具方法使用，将任意类型的值转为字符串。
```javascript
String(true) // "true"
String(5) // "5"
```
# 实例属性

String.prototype.length
字符串实例的length属性返回字符串的长度。
```javascript
'abc'.length // 3

```
# 实例方法
# String.prototype.charAt()
`charAt`方法返回指定位置的字符，参数是从0开始编号的位置。
```javascript
var s = new String('abc');

s.charAt(1) // "b"
s.charAt(s.length - 1) // "c"

```
这个方法完全可以用数组下标替代。
```javascript
'abc'.charAt(1) // "b"
'abc'[1] // "b"
```
如果参数为负数，或大于等于字符串的长度，charAt返回空字符串。
```javascript
'abc'.charAt(-1) // ""
'abc'.charAt(3) // ""

```
# String.prototype.concat()
concat方法用于连接两个字符串，返回一个新字符串，**不改变原字符串。**

```javascript
var s1 = 'abc';
var s2 = 'def';

s1.concat(s2) // "abcdef"
s1 // "abc"
```
该方法可以接受多个参数。
```javascript

'a'.concat('b', 'c') // "abc"

```
如果参数不是字符串，concat方法会将其先转为字符串，然后再连接。
```javascript
var one = 1;
var two = 2;
var three = '3';

''.concat(one, two, three) // "123"
one + two + three // "33"
```
上面代码中，concat方法将参数先转成字符串再连接，所以返回的是一个三个字符的字符串。作为对比，加号运算符在两个运算数都是数值时，不会转换类型，所以返回的是一个两个字符的字符串。
# String.prototype.slice()
slice方法用于从原字符串取出子字符串并返回，不改变原字符串。它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）。
```javascript

'JavaScript'.slice(0, 4) // "Java"

```
如果省略第二个参数，则表示子字符串一直到原字符串结束。
```javascript
'JavaScript'.slice(4) // "Script"

```
如果参数是负值，表示从结尾开始倒数计算的位置，即该负值加上字符串长度。
```javascript
'JavaScript'.slice(-6) // "Script"
'JavaScript'.slice(0, -6) // "Java"
'JavaScript'.slice(-2, -1) // "p"
```
如果第一个参数大于第二个参数，slice方法返回一个空字符串。
```javascript
'JavaScript'.slice(2, 1) // ""

```
# String.prototype.substring()
substring方法用于从原字符串取出子字符串并返回，不改变原字符串，跟slice方法很相像。它的第一个参数表示子字符串的开始位置，第二个位置表示结束位置（返回结果不含该位置）。
```javascript
JavaScript'.substring(0, 4) // "Java"

```
如果省略第二个参数，则表示子字符串一直到原字符串的结束。

```javascript
JavaScript'.substring(4) // "Script"

```
如果第一个参数大于第二个参数，substring方法会自动更换两个参数的位置。
```javascript
'JavaScript'.substring(10, 4) // "Script"
// 等同于
'JavaScript'.substring(4, 10) // "Script"

```
上面代码中，调换substring方法的两个参数，都得到同样的结果。

如果参数是负数，substring方法会自动将负数转为0。
```javascript
'JavaScript'.substring(-3) // "JavaScript"
'JavaScript'.substring(4, -3) // "Java"
```
由于这些规则违反直觉，因此不建议使用substring方法，应该优先使用slice。
# String.prototype.substr()
substr方法用于从原字符串取出子字符串并返回，不改变原字符串，跟slice和substring方法的作用相同。


substr方法的第一个参数是子字符串的开始位置（从0开始计算），第二个参数是子字符串的长度。
```javascript

'JavaScript'.substr(4, 6) // "Script"

```
如果省略第二个参数，则表示子字符串一直到原字符串的结束。
```javascript
'JavaScript'.substr(4) // "Script"

```
如果第一个参数是负数，表示倒数计算的字符位置。如果第二个参数是负数，将被自动转为0，因此会返回空字符串。
```javascript
'JavaScript'.substr(-6) // "Script"
'JavaScript'.substr(4, -1) // ""

```
上面代码中，第二个例子的参数-1自动转为0，表示子字符串长度为0，所以返回空字符串。
# String.prototype.indexOf()，String.prototype.lastIndexOf()
indexOf方法用于确定一个字符串在另一个字符串中第一次出现的位置，返回结果是匹配开始的位置。如果返回-1，就表示不匹配。

```javascript

'hello world'.indexOf('o') // 4
'JavaScript'.indexOf('script') // -1

```
indexOf方法还可以接受第二个参数，表示从该位置开始向后匹配。
```javascript
'hello world'.indexOf('o', 6) // 7

```
另外，lastIndexOf的第二个参数表示从该位置起向前匹配。
```javascript
'hello world'.lastIndexOf('o', 6) // 4

```
# String.prototype.trim()
trim方法用于去除字符串两端的空格，返回一个新字符串，不改变原字符串。
```javascript

'  hello world  '.trim() // "hello world"

```
该方法去除的不仅是空格，还包括制表符（\t、\v）、换行符（\n）和回车符（\r）。
```javascript

'\r\nabc \t'.trim() // 'abc'

```
# String.prototype.toLowerCase()，String.prototype.toUpperCase()

toLowerCase方法用于将一个字符串全部转为小写，toUpperCase则是全部转为大写。它们都返回一个新字符串，不改变原字符串。
```javascript
'Hello World'.toLowerCase() // "hello world"

'Hello World'.toUpperCase() // "HELLO WORLD"

```
# String.prototype.match()

match方法用于确定原字符串是否匹配某个子字符串，返回一个数组，成员为匹配的第一个字符串。如果没有找到匹配，则返回null
```javascript

'cat, bat, sat, fat'.match('at') // ["at"]
'cat, bat, sat, fat'.match('xt') // null

```
# String.prototype.search()，String.prototype.replace()
search方法的用法基本等同于match，但是返回值为匹配的第一个位置。如果没有找到匹配，则返回-1。
```javascript
'cat, bat, sat, fat'.search('at') // 1

```
replace方法用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有g修饰符的正则表达式）。
```javascript

'aaa'.replace('a', 'b') // "baa"

```
# String.prototype.split()
split方法按照给定规则分割字符串，返回一个由分割出来的子字符串组成的**数组**。
```javascript
'a|b|c'.split('') // ["a", "|", "b", "|", "c"]

```
如果省略参数，则返回数组的唯一成员就是原字符串。
```javascript

'a|b|c'.split() // ["a|b|c"]

```
如果满足分割规则的两个部分紧邻着（即两个分割符中间没有其他字符），则返回数组之中会有一个空字符串。
```javascript
'a||c'.split('|') // ['a', '', 'c']

```
如果满足分割规则的部分处于字符串的开头或结尾（即它的前面或后面没有其他字符），则返回数组的第一个或最后一个成员是一个空字符串。
```javascript
'|b|c'.split('|') // ["", "b", "c"]
'a|b|'.split('|') // ["a", "b", ""]

```
split方法还可以接受第二个参数，限定返回数组的最大成员数。
```javascript
'a|b|c'.split('|', 0) // []
'a|b|c'.split('|', 1) // ["a"]
'a|b|c'.split('|', 2) // ["a", "b"]
'a|b|c'.split('|', 3) // ["a", "b", "c"]
'a|b|c'.split('|', 4) // ["a", "b", "c"]

```

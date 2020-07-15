---
title: Es6对象
cover : https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2120883675,1158188894&fm=11&gp=0.jpg
---


# 属性的简洁表示法

Es6允许直接写入变量和函数，作为对象的属性和方法。
```javascript
 const foo = 'bar'
 const baz = {foo}
 baz // {foo:"bar"}
 //等同于
 const baz = {foo:foo}
```
上面代码表明。Es6 允许在对象之中，直接写入变量，这时，属性名为变量名，属性值为变量值
```javascript
function f(x,y){
  return {x,y}
}

//等同于
function f (x,y){
  return {x:x,y:y}
}
f(1,2)  //object {x:1,y:2}
```
 除了属性简写，方法也可简写
 ```javascript
 const o = {
   method(){
     return "hello!"
   }
 }
 //等同于
 const o = {
   method:function(){
     return "hello!"
   }
 }
  


 ```
 下边是一个实际例子
 ```javascript
 let birth = "200/01/01"
 const Person = {
   name:"张三",
   birth,
   hello()
   console.log('我的名字是'，this.name)

 }



 ```
 这种写法用于函数的返回值，将会非常方便
 ```javascript

 function getPoint(){
   const x=1;
   const y=2;
   return {x,y}
 }
 getPoint()  //{x:1.y:2}
 ```
 
 `CommonJS`模块输出一组变量，就非常合适使用简洁写法
 ```javascript
 let ms = {};
function getItem (key) {
  return key in ms ? ms[key] : null;
}
function setItem (key, value) {
  ms[key] = value;
}
function clear () {
  ms = {};
}
module.exports = { getItem, setItem, clear };
// 等同于
module.exports = {
  getItem: getItem,
  setItem: setItem,
  clear: clear
};

 ```
属性的赋值器`（setter）`和取值器`（getter）`，事实上也是采用这种写法。

```javascript
const cart = {
  _wheels :4,
  get wheels (){
    return this._wheels；
  }，
  set wheels(value){
    if(value < this._wheels){
      throw new Error('数值小')
    }
    this._wheels = value
  }
}

```
# 属性名表达式
`javascript`定义对象的属性有两种方法·
```javascript
//方法一
obj.foo =true,
//方法二
obj['a'+'bc'] =123;
console.log(obj)
```
上面代码的方法一是直接用标识符作为属性名，方法二是用表达式作为属性名，这时要将表达式放在方括号之内。
但是，如果使用字面量方式定义对象（使用大括号），在 ES5中只能使用方法一（标识符）定义属性。
```javascript
var obj ={
  foo:true,
  abc:123
}
```
下面是另一个例子。
```javascript
let lastworld = 'last world'
const a = {
  'first world':'hello'
  [lastworls]:'word'
}
a['first word'] // "hello"
a[lastWord] // "world"
a['last word'] // "world"

```
注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串[object Object]，这一点要特别小心
```javascript
const keyA = {a: 1};
const keyB = {b: 2};
const myObject = {
  [keyA]: 'valueA',
  [keyB]: 'valueB'
};
myObject // Object {[object Object]: "valueB"}

```
上面代码中，`[keyA]`和`[keyB]`得到的都是[object Object]，所以[keyB]会把[keyA]覆盖掉，而myObject最后只有一个[object Object]属性。

# 方法的name属性
函数的  `name`属性，返回函数名。对象方法也是函数，因此也有`name`属性。\  
```javascript

const person = {
  sayname (){
    console.log('hello')
  }

}
person.sayname.name //"sayname
```
如果对象的方法使用了取值函数`（getter）`和存值函数`（setter）`，则`name`属性不是在该方法上边，而是该方法的属性的描述对象的`get `和`set`属性上面，返回值是方法名前加`get` 和`set`
```javascript
const obj = {
  get foo() {},
  set foo(x) {}
};
obj.foo.name
// TypeError: Cannot read property 'name' of undefined
const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');
descriptor.get.name // "get foo"
descriptor.set.name // "set foo"

```
有两种特殊情况：bind方法创造的函数，name属性返回bound加上原函数的名字；Function构造函数创造的函数，name属性返回anonymous。
```javascript
(new Function()).name // "anonymous"
var doSomething = function() {
  // ...
};
doSomething.bind().name // "bound doSomething"


```

# Object.is()
`ES5`比较两个值是否相等，只有两个运算符'=='和'==='前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。JavaScript缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。
ES6提出同值相等算法，用来解决这个问题。Object.is就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

```javascript

Object.is('foo', 'foo') // true
Object.is({}, {}) // false

```
不同之处有两个   一个+0 和-0，一个NaN等于自身
```javascript
+0 === -0 //true
NaN === NaN // false
Object.is(+0, -0) // false
Object.is(NaN, NaN) // true

```
`ES5`可以通过下面的代码，部署Object.is。
```javascript
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true
});
```
# Objext.assign()
# 基本用法
`object.assign`方法用于对象的合并，将源对象的所有可妹举属性，复制到目标对象
```javascript
  const target= {a:1};
  const source1 = {b:2};
  const source2 = {c:3};
  Obeject.assign(target,source1,source2)
  target  //{a:1,b:2,c:3}
```
`Object.assign`方法的第一个参数是目标对象，后面的参数都是源对象。
注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

```javascript
const target = { a: 1, b: 1 };
const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}

```
如果只有一个参数，`Object.assign`会直接返回该参数。

```javascript
const obj = {a: 1};
Object.assign(obj) === obj // true

```
如果该参数不是对象，则会先转成对象，然后返回。
```javascript
typeOf Object.assign(2) //object
```
由于`undefined`和`null`无法转成对象，所以如果它们作为参数，就会报错。
```javascript
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```
如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果`undefined`和`null`不在首参数，就不会报错。
```javascript
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true`
```

其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。
```javascript
const v1 = 'abc';
const v2 = true;
const v3 = 10;
const obj = Object.assign({}, v1, v2, v3);
console.log(obj); // { "0": "a", "1": "b", "2": "c" }

```
上面代码中，`v1`、`v2`、`v3`分别是字符串、布尔值和数值，结果只有字符串合入目标对象（以字符数组的形式），数值和布尔值都会被忽略。这是因为只有字符串的包装对象，会产生可枚举属性。
```javascript
Object(true) // {[[PrimitiveValue]]: true}
Object(10)  //  {[[PrimitiveValue]]: 10}
Object('abc') // {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}

```
上面代码中，布尔值、数值、字符串分别转成对应的包装对象，可以看到它们的原始值都在包装对象的内部属性[[PrimitiveValue]]上面，这个属性是不会被`Object.assign`拷贝的。只有字符串的包装对象，会产生可枚举的实义属性，那些属性则会被拷贝。
`Object.assign`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）。
```javascript
Object.assign({b: 'c'},
  Object.defineProperty({}, 'invisible', {
    enumerable: false,
    value: 'hello'
  })
)
// { b: 'c' }

```
# 注意点
# （1）浅拷贝
`Object.assign`方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。
```javascript
const obj1 = {a: {b: 1}};
const obj2 = Object.assign({}, obj1);
obj1.a.b = 2;
obj2.a.b // 2
```
面代码中，源对象obj1的a属性的值是一个对象，Object.assign拷贝得到的是这个对象的引用。这个对象的任何变化，都会反映到目标对象上面。

(2)同名属性的替换
对于这种嵌套的对象，一旦遇到同名属性，`Object.assign`的处理方法是替换，而不是添加。
```javascript
const target = { a: { b: 'c', d: 'e' } }
const source = { a: { b: 'hello' } }
Object.assign(target, source) // { a: { b: 'hello' } }

```
上面代码中，`target`对象的a属性被source对象的a属性整个替换掉了，而不会得到{a:{b:'hello',d:'e' }}的结果。这通常不是开发者想要的，需要特别小心。
一些函数库提供Object.assign的定制版本（比如Lodash的_.defaultsDeep方法），可以得到深拷贝的合并。

# (3)数组的处理
`Object.assign`可以用来处理数组，但是会把数组视为对象。
```javascript
Object.assign([1, 2, 3], [4, 5]) // [4, 5, 3]


```
上面代码中，`Object.assign`把数组视为属性名为0、1、2的对象，因此源数组的0号属性4覆盖了目标数组的0号属性1。


# (4)取值函数的处理

`Object.assign`只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

```javascript
const source = {
  get foo() { return 1 }
};
const target = {};
Object.assign(target, source) // { foo: 1 }

```
上面代码中，`source`对象的`foo`属性是一个取值函数，`Object.assign`不会复制这个取值函数，只会拿到值以后，将这个值复制过去。
# 常见用途

 `Object.assign`方法有很多用处。
 # (1)为对象添加属性
 ```javascript
 class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
 ```
 上面方法通过`Object.assign`方法，将x属性和y属性添加到Point类的对象实例。
 # (2)为对象添加方法
 ```javascript
 Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
});
// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};
 ```
上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用assign方法添加到SomeClass.prototype之中。
# (3)克隆对象
```javascript
function clone(origin) {
  return Object.assign({}, origin);
}
```
上面代码将原始对象拷贝到一个空对象，就得到了原始对象的克隆。
不过，采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值。如果想要保持继承链，可以采用下面的代码。
```javascript
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```
# (4)合并多个对象
将多个对象合并到某个对象。
```javascript
const merge =(target, ...sources) => Object.assign(target, ...sources);

```
如果希望合并后返回一个新对象，可以改写上面函数，对一个空对象合并。
```javascript

const merge =(...sources) => Object.assign({}, ...sources);

```
# (5)为属性指定默认值
```javascript
const DEFAULTS = {
  logLevel: 0,
  outputFormat: 'html'
};
function processContent(options) {
  options = Object.assign({}, DEFAULTS, options);
  console.log(options);
  // ...
}
```
上面代码中，`DEFAULTS`对象是默认值，`options`对象是用户提供的参数。`Object.assign`方法将`DEFAULTS`和`options`合并成一个新对象，如果两者有同名属性，则`option`的属性值会覆盖`DEFAULTS`的属性值。
注意，由于存在浅拷贝的问题，`DEFAULTS`对象和`options`对象的所有属性的值，最好都是简单类型，不要指向另一个对象。否则，`DEFAULTS`对象的该属性很可能不起作用。
```javascript
const DEFAULTS = {
  url: {
    host: 'example.com',
    port: 7070
  },
};
processContent({ url: {port: 8000} })
// {
//   url: {port: 8000}
// }
```
上面代码的原意是将url.port改成 8000，url.host不变。实际结果却是options.url覆盖掉DEFAULTS.url，所以url.host就不存在了。
# 属性的可枚举性和遍历
# 可枚举型
对象的每个属性都有一个描述对象，用来控制该属性的行为。`Object.getOwnPropertyDescriptor`方法可以获取该属性的描述对象。
```javascript
let obj = { foo: 123 };
Object.getOwnPropertyDescriptor(obj, 'foo')
//  {
//    value: 123,
//    writable: true,
//    enumerable: true,
//    configurable: true
//  }

```
描述对象的`enumerable`属性，称为”可枚举性“，如果该属性为false，就表示某些操作会忽略当前属性。
目前，有四个操作会忽略`enumerable`为`false`的属性。

`for...in`循环：只遍历对象自身的和继承的可枚举的属性。
`Object.keys()`：返回对象自身的所有可枚举的属性的键名。
`JSON.stringify()`：只串行化对象自身的可枚举的属性。
`Object.assign()`： 忽略`enumerable为false`的属性，只拷贝对象自身的可枚举的属性。
这四个操作之中，前三个是`ES5`就有的，最后一个`Object.assign()`是`ES6`新增的。其中，只有`for...in`会返回继承的属性，其他三个方法都会忽略继承的属性，只处理对象自身的属性。实际上，引入“可枚举”这个概念的最初目的，就是让某些属性可以规避掉`for...in`操作，不然所有内部属性和方法都会被遍历到。比如，对象原型的`toString`方法，以及数组的`length`属性，就通过“可枚举性”，从而避免被`for...in`遍历到。

```javascript
Object.getOwnPropertyDescriptor(Object.prototype, 'toString').enumerable // false
Object.getOwnPropertyDescriptor([], 'length').enumerable // false
```
上面代码中，`toString`和length属性的`enumerable`都是false，因此`for...in`不会遍历到这两个继承自原型的属性。
另外，`ES6` 规定，所有`Class`的原型的方法都是不可枚举的。
```javascript
Object.getOwnPropertyDescriptor(class {foo() {}}.prototype, 'foo').enumerable
// false
```
总的来说，操作中引入继承的属性会让问题复杂化，大多数时候，我们只关心对象自身的属性。所以，尽量不要用for...in循环，而用Object.keys()代替。
# 属性的遍历
`ES6`一共有5种方法可以遍历对象的属性。
# (1)for…in
`for...in`循环遍历对象自身的和继承的可枚举型属性（不含symbol属性）
# (2)Object.keys(obj)
`Object.keys`返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）的键名。

# (3)Object.getOwnPropertyNames(obj)
`Object.getOwnPropertyNames`返回一个数组，包含对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）的键名。
# (4)Object.getOwnPropertySymbols(obj)
`Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有Symbol属性的键名。


# (5)Reflect.ownKeys(obj)

`Reflect.ownKeys`返回一个数组，包含对象自身的所有键名，不管键名是Symbol或字符串，也不管是否可枚举。
以上的5种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。
首先遍历所有数值键，按照数值升序排列。
其次遍历所有字符串键，按照加入时间升序排列。
最后遍历所有Symbol键，按照加入时间升序排列。
```javascript
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]

```
上面代码中，`Reflect.ownKeys`方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性2和10，其次是字符串属性b和a，最后是Symbol属性。
`##Object.getOwnPropertyDescriptors()`
`Object.getOwnPropertyDescriptor`方法会返回某个对象属性的描述对象。Object.getOwnPropertyDescriptors方法返回指定对象所有自身属性（非继承属性）的描述对象。


```javascript
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};
Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }

```
上面代码中，`Object.getOwnPropertyDescriptors`方法返回一个对象，所有原对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象。
# Object.keys()

`Object.keys()`方法返回一个数组，成员是参数对象自身的(不含继承的)所有可遍历属性的键名。
```javascript
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj) // ["foo", "baz"]

```

`ES2017`引入了跟`Object.keys`配套的`Object.values`和`Object.entries`，作为遍历一个对象的补充手段，供for...of循环使用。

````javascript
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };
for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}
for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}
for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```
## Object.values()
`Object.values`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值。
```javascript
const obj = {foo:'bar',baz:42};
Object.valus(obj)//["bar",42]
const obj = { 100: 'a', 2: 'b', 7: 'c' };
Object.values(obj) // ["b", "c", "a"]


```
上面代码中，属性名为数值的属性，是按照数值大小，从小到大遍历的，因此返回的顺序是b、c、a。
`Object.values`只返回对象自身的可遍历属性。

```javascript
const obj = Object.create({}, {p: {value: 42}});
Object.values(obj) // []
```
上面代码中，·Object.create·方法的第二个参数添加的对象属性（属性p），如果不显式声明，默认是不可遍历的，因为p的属性描述对象的enumerable默认是false，Object.values不会返回这个属性。只要把enumerable改成true，Object.values就会返回属性p的值。

# Object.entries
Object.entries方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值对数组。
```javascript
const obj = {f00:'bar',baz:42};
object.entries(obj) // [ ["foo", "bar"], ["baz", 42] ]

```
除了返回值不一样，该方法的行为与`Object.values`基本一致。
如果原对象的属性名是一个`Symbol`值，该属性会被忽略。
`Object.entries`的基本用途是遍历对象的属性。
```javascript
et obj = { one: 1, two: 2 };
for (let [k, v] of Object.entries(obj)) {
  console.log(
    `${JSON.stringify(k)}: ${JSON.stringify(v)}`
  );
}
// "one": 1
// "two": 2
```
`Object.entries`方法的另一个用处是，将对象转为真正的Map结构。
```javascript
const obj = { foo: 'bar', baz: 42 };
const map = new Map(Object.entries(obj));
map // Map { foo: "bar", baz: 42 }
```
实现Object.entries方法，非常简单。
```javascript
// Generator函数的版本
function* entries(obj) {
  for (let key of Object.keys(obj)) {
    yield [key, obj[key]];
  }
}
// 非Generator函数的版本
function entries(obj) {
  let arr = [];
  for (let key of Object.keys(obj)) {
    arr.push([key, obj[key]]);
  }
  return arr;
}
```
# 对象的扩展运算符

ES2018将这个运算符引入了对象。
```javascript
const [a, ...b] = [1, 2, 3];
a // 1
b // [2, 3]
```
# 解构赋值
对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和它们的值，都会拷贝到新对象上面。
```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }

```
上面代码中，变量z是解构赋值所在的对象。它获取等号右边的所有尚未读取的键（a和b），将它们连同值一起拷贝过来。
由于解构赋值要求等号右边是一个对象，所以如果等号右边是undefined或null，就会报错，因为它们无法转为对象。

```javascript
let { x, y, ...z } = null; // 运行时错误
let { x, y, ...z } = undefined; // 运行时错误

```
# 扩展运算符

对象的扩展运算符(...)用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。

```javascript
let z = { a: 3, b: 4 };
let n = { ...z };
n // { a: 3, b: 4 }

```
这等同于使用Object.assign方法。
```javascript
let aClone = { ...a };
// 等同于
let aClone = Object.assign({}, a);

```
上面的例子只是拷贝了对象实例的属性，如果想完整克隆一个对象，还拷贝对象原型的属性，可以采用下面的写法。


```javascript
// 写法一
const clone1 = {
  __proto__: Object.getPrototypeOf(obj),
  ...obj
};
// 写法二
const clone2 = Object.assign(
  Object.create(Object.getPrototypeOf(obj)),
  obj
);
// 写法三
const clone3 = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
)

```
上面代码中，写法一的__proto__属性在非浏览器的环境不一定部署，因此推荐使用写法二和写法三。
扩展运算符可以用于合并两个对象。

```javascript
et ab = { ...a, ...b };
// 等同于
let ab = Object.assign({}, a, b);

```



---
title: Es6箭头函数
cover: http://img2.imgtn.bdimg.com/it/u=2256515711,1252630197&fm=15&gp=0.jpg
---
# 函数参数的默认值
# 基本用法
Es6之前，不能直接为函数的参数指定默认值，只能采用变通的方法。
```javascript
  function（x,y）{
    y=y ||'world';
    console.log(x,y);

  }
 log('hello')// hello world
 log('Hello', 'China') // Hello China
 log('Hello', '') // Hello World
  

```
以上代码检查函数`log`的参数`Y`有没有赋值，如果没有则指定默认值`world`这种写法的缺点在于，如果参数y赋值了，但是对应的布尔值为`false`，则该赋值不起作用。就像上面代码的最后一行，参数`y`等于空字符，结果被改为默认值。
为了避免这个问题，通常需要先判断一下参数y是否被赋值，如果没有，再等于默认值。
```javascript
if(typeof y === "undefined"){
  y="world"
}
```
es6允许为函数的参数设置默认值，既直接写在函数参数定义的后面
```javascript
function log(x,y = "world"){
  console.log(x,y)
}
log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
//例2
function point (x=0,y=0){
  this.x =x
  this.y = y
}
const  p = new point();
p //{x:0,y:0}

```
除了简洁，`ES6`的写法还有两个好处：首先，阅读代码的人，可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档；其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。
参数变量是默认声明的，所以不能用`let`或`const`再次声明，否则会报错。
```javascript

function foo(x=5){
  let x= 1 //error
  const x =2 //error

}
```
使用函数默认值时，函数不能有同名参数
```javascript
//不报错
function foo(x,x,y){
  //....
}
//报错
function foo(x,x,y=1){
  //....
}
```
另外，一个容易忽略的地方是，参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值是惰性求值的。
```javascript
let x =99;
function foo(p =x+1){
  console.log(p)
}
foo()//100
x=100
foo() //101

```

#  与结构赋值默认结合使用

```javascript

function foo({x,y=5}{
  console.log(x,y)
})
foo({})  //undefinded 5
foo({x:1})  // // 1  5
foo({x:1,y:2})    //1 2 
foo()   //error
```
上面代码只使用了对象的解构赋值默认值，没有使用函数参数的默认值。只有当函数`foo`的参数是一个对象时，变量`x`和`y`才会通过解构赋值生成。如果函数`foo`调用时没提供参数，变量`x`和`y`就不会生成，从而报错。通过提供函数参数的默认值，就可以避免这种情况。
```javascript
function foo ({x,y=5} ={}){
  console.log(x,y)
}
foo()   //undefined   5
```
上面代码指定，如果没有提供参数，函数foo的参数默认为一个空对象。

```javascript
function fetch(url, { body = '', method = 'GET', headers = {} }) {
  console.log(method);
}
fetch('http://example.com', {}) // "GET"
fetch('http://example.com') // 报错


```
上面代码中，如果函数`fetch`的第二个参数是一个对象，就可以为它的三个属性设置默认值。这种写法不能省略第二个参数，如果结合函数参数的默认值，就可以省略第二个参数。这时，就出现了双重默认值。
```javascript
function fetch(url, { body = '', method = 'GET', headers = {} } = {}) {
  console.log(method);
}
fetch('http://example.com') // "GET"
```
上面代码中，函数fetch没有第二个参数时，函数参数的默认值就会生效，然后才是解构赋值的默认值生效，变量method才会取到默认值GET。
```javascript
/ 写法一
function m1({x = 0, y = 0} = {}) {
  return [x, y];
}
// 写法二
function m2({x, y} = { x: 0, y: 0 }) {
  return [x, y];

```
上面两种写法都对函数的参数设定了默认值，区别是写法一函数参数的默认值是空对象，但是设置了对象解构赋值的默认值；写法二函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值。
```javascript
// 函数没有参数的情况
m1() // [0, 0]
m2() // [0, 0]
// x和y都有值的情况
m1({x: 3, y: 8}) // [3, 8]
m2({x: 3, y: 8}) // [3, 8]
// x有值，y无值的情况
m1({x: 3}) // [3, 0]
m2({x: 3}) // [3, undefined]
// x和y都无值的情况
m1({}) // [0, 0];
m2({}) // [undefined, undefined]
m1({z: 3}) // [0, 0]
m2({z: 3}) // [undefined, undefined]

```
# 参数默认值的位置
通常情况下，定义了默认值的参数，应该是函数的尾参数。因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。
```javascript
// 例一
function f(x = 1, y) {
  return [x, y];
}
f() // [1, undefined]
f(2) // [2, undefined])
f(, 1) // 报错
f(undefined, 1) // [1, 1]
// 例二
function f(x, y = 5, z) {
  return [x, y, z];
}
f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // 报错
f(1, undefined, 2) // [1, 5, 2]


```
上面代码中，有默认值的参数都不是尾参数。这时，无法只省略该参数，而不省略它后面的参数，除非显式输入`undefined`。
如果传入`undefined`，将触发该参数等于默认值，`null`则没有这个效果。
```javascript
function foo(x=5,y=6){
  console.log(x,y)
}
foo(undefined,null)   //5  null
```
# 函数的length属性
指定了默认值以后，函数的`length`属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，`length`属性将失真。
```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2


```
`length`属性的返回值，等于函数的参数个数减去指定了默认值的参数个数。这是因为length属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。
# 箭头函数
ES6允许使用“箭头”（`=>`）定义函数。
```javascript
var f = v =>v 
//等同于
var f = function(v){
  return v
}
```
如果箭头函数不需要参数或者需要多个参数就使用一个圆括号代表参数部分
```javascript
var f = ()=>5;
//等于
var f = function(){
  return 5
}
var sum = (num1,num2)=>num1+num2;
//等同于
var sum = function(num1,num2){
  return num1+num2
}
```
如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。

```javascript
var sum = (num1,num2) =>{
  return num1 +num2
}

```
由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
```javascript
// 报错
let getTempItem = id => { id: id, name: "Temp" };
// 不报错
let getTempItem = id => ({ id: id, name: "Temp" });


```
下面是一种特殊情况
```javascript
let foo = ( )=>{
  a:1
}
foo() //undefined
```
上面代码中，`a`可以被解释为语句的标签，因此实际执行的语句是`1`;，然后函数就结束了，没有返回值。
如果箭头函数只有一行语句，且不需要返回值，可以采用下面的写法，就不用写大括号了。
```javascript
let fn = ()=> void doesNotReturn();
```
箭头函数可以与变量解构结合使用。
```javascript
const full = ({frist,last})=> first + ' ' + last;
//等同于
function full (person){
  return person.first + ' ' +person.last
}
```
箭头函数使得表达更加简洁。
```javascript
const isEven  = n => n%2 ==0
const sauer = n => n*n
```
上面代码只用了两行，就定义了两个简单的工具函数。如果不用箭头函数，可能就要占用多行，而且还不如现在这样写醒目。
箭头函数的一个用处是简化回调函数。
```javascript
//正常函数
[1,2.3].map(function(x){
 return x*x
})
//箭头函数
[1，2，3].map(x => x*x)
```
下面是`rest`参数与箭头函数结合的例子。
```javascript
const numbers = (...nums) => nums;
numbers(1, 2, 3, 4, 5) // [1,2,3,4,5]
const headAndTail = (head, ...tail) => [head, tail];
headAndTail(1, 2, 3, 4, 5) // [1,[2,3,4,5]]
```
# 使用注意点

箭头函数有几个使用注意点。

函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。
不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。
不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用`rest`参数代替。
不可以使用`yield`命令，因此箭头函数不能用作Generator函数。
上面四点中，第一点尤其值得注意。this对象的指向是可变的，但是在箭头函数中，它是固定的。
```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}
var id = 21;
foo.call({ id: 42 });
// id: 42

```
上面代码中，`setTimeout`的参数是一个箭头函数，这个箭头函数的定义生效是在`foo`函数生成时，而它的真正执行要等到100毫秒后。如果是普通函数，执行时this应该指向全局对象`window`，这时应该输出`21`。但是，箭头函数导致this总是指向函数定义生效时所在的对象（本例是{id: 42}），所以输出的是42。
箭头函数可以让`setTimeout`里面的`this`，绑定定义时所在的作用域，而不是指向运行时所在的作用域。
```javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}
var timer = new Timer();
setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0

```
上面代码中，`Timer`函数内部设置了两个定时器，分别使用了箭头函数和普通函数。前者的`this`绑定定义时所在的作用域（即Timer函数），后者的this指向运行时所在的作用域（即全局对象）。所以，`3100`毫秒之后，timer.s1被更新了3次，而timer.s2一次都没更新。
箭头函数可以让this指向固定化，这种特性很有利于封装回调函数。下面是一个例子，DOM事件的回调函数封装在一个对象里面。
```javascript
var handler = {
  id: '123456',
  init: function() {
    document.addEventListener('click',
      event => this.doSomething(event.type), false);
  },
  doSomething: function(type) {
    console.log('Handling ' + type  + ' for ' + this.id);
  }
};
```
上面代码的init方法中，使用了箭头函数，这导致这个箭头函数里面的`this`，总是指向`handler`对象。否则，回调函数运行时，this.doSomething这一行会报错，因为此时this指向document对象。
`this`指向的固定化，并不是因为箭头函数内部有绑定`this`的机制，实际原因是箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`。正是因为它没有`this`，所以也就不能用作构造函数。
所以，箭头函数转成ES5的代码如下。
```javascript
// ES6
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}
// ES5
function foo() {
  var _this = this;
  setTimeout(function () {
    console.log('id:', _this.id);
  }, 100);
}
```

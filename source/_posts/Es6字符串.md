---
title: Es6字符串
cover: http://img5.imgtn.bdimg.com/it/u=1300146589,158997046&fm=11&gp=0.jpg
---
# 字符串的遍历器接口
ES6为字符串添加了遍历器接口，使得字符串可以被for...of循环
```javascript
for (let codePoint of 'foo'){
  console.log(codePoint)
} 
//'f' 
//'o'
//'o'
```
# includes() startsWith(),endswith()
 `includes()`:返回值，表示是否找到了参数字符串
 `startWith()`:返回布尔值，表示参数字符串是否再源字符串的头部
 `endWith()`:返回布尔值，表示参数字符串是否在元字符串的尾部
 ```javascript
  var s= 'hello world'
  s.startWith('Hello') //true
  s.endwith('d')  //true
  s.includes('o') //true

 ```
 这仨个参数都支持第二个参数，表示开始搜索的位置
 ```javascript
 var s = 'hello world'
 s.startWith('world',6)  //true

 ```

 # repeat()
` repeat`方法返回一个新字符串，表示将原字符串重复n次。
```javascript
'x'.repeat(3) //xxx
'na'.repeat(0) //""
```
参数如果是小数。会被取整
```javascript
'na'.repeat(2.9) //'nana'
```
 但是，如果参数是0到-1之间的小数，则等同于0，这是因为会先进行取整运算。0到-1之间的小数，取整以后等于-0，repeat视同为0。
 ```javascript
 
'na'.repeat(-0.9) // ""


 ```
如果repeat的参数是字符串，则会先转换成数字。
```javascript
'na'.repeat('na') // ""
'na'.repeat('3') // "nanana"

```
# padStart()，padEnd()

`padStart()`用于头部补全
`padEnd() `用于尾部补全

```javascript

'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
````
`padStart`和`padEnd`一共接受两个参数，第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。
如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。
```javascript

'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```
如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会截去超出位数的补全字符串。
```javascript
'abc'.padStart(10,'0123456789') //'0123456abc'
```
# matchAll() 
matchAll方法返回一个正则表达式在当前字符串的所有匹配。
# 模板字符串
模板字符串是增强版的字符串，用反引号(`)标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
```javascript
// 普通字符串
`In JavaScript '\n' is a line-feed.`
// 多行字符串
`In JavaScript this is
 not legal.`
console.log(`string text line 1
string text line 2`);
// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

```
如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。
```javascript

let greeting = `\`Yo\` World!`;

````
如果不想要这个换行，可以使用trim方法消除它。
```javascript
$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`.trim());
```
模板字符串中嵌入变量，需要将变量名写在${}之中。
```javascript
function authorize(user, action) {
  if (!user.hasPrivilege(action)) {
    throw new Error(
      // 传统写法为
      // 'User '+user.name+' is not authorized to do '+action+'.'
      `User ${user.name} is not authorized to do ${action}.`);
  }
}
```
大括号内部可以放入任意的JavaScript表达式，可以进行运算，以及引用对象属性。
```javascript
let x = 1;
let y = 2;
`${x}+${y}=${x+y}` // "1+2=3"
`${x}+${y*2}=${x+y*2}` // "1+4=5"
let obj = {x: 1, y: 2};
`${obj.x + obj.y}` // "3"
```
模板字符串之中还能调用函数。
```javascript
function fn(){
  return "hello world"
}
`foo ${(fn())} bar`
```
模板字符串可以嵌套
```javascript
const tmpl  addrs=>`
<table>
${addres.map(addr =>'
<tr>
<td>${addr.first}</td>
<td>${addr.last}</td>
</tr>').join('')


)}
</table>
`
```
# String.raw()
String.raw方法，往往用来充当模板字符串的处理函数，返回一个斜杆都被转义
```javascript
string.raw`hi\n${2+3}！` 返回"hi\\n5!"

```
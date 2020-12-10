<!--
 * @Author: your name
 * @Date: 2020-11-23 13:34:37
 * @LastEditTime: 2020-11-24 11:27:39
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \mengmwq.github.io\source\_posts\js拼接字符串.md
-->
#    js 拼接字符串

```javascript
   var str = 'mjk';
   var str1 = 'xuexi'
cosole.log(str+str1)

```
```javascript
for(var i=1;i<10;i++){
  for(var j=1;j<=i;j++){
    console.log(j)
     document.write(('<span style="color:red;padding-right:30px">'+ j +'x' + i + '=' + j*i +'</span>'))
   
     document.write("<br>")

}
```
# 模板字符串
```javascript
console.log(`${str}${str1}`)
```
```javascript
for(var i=1;i<10;i++){
  for(var j=1;j<=i;j++){
    console.log(j)
     
    document.write(`<span style="color:red;padding-right:30px">${j}*${i}=${j*i}</span>`)
  }
  document.write("<br>")
```
# 字符串拼接   外单内双   或者  外双内单    
```javascript
var str3 = ' " ';
var str4 = " '' ";  
document.write("<h2 style='color:red' > 你好</h2>"+ 1)

document.write("<h2 style="+"color:red"+">你好</h2>")
```
# 转译字符
```javascript
document.write("<h2 style=\"color:red\">你好呀</h2>")
console.log("hello"+"\n"+"worle")


 
  
```
# 字符串转义  使用

```html
<textarea name="" id="cont" cols="30" rows="10">

 

  </textarea>
```

```javascript
  document.getElementById("cont").value =("你好 \n 中国")
```
# 字符串方法
```javascript
var str = "abbcalgldkjgldgehlgjldf";
1，indexOf()，返回指定字符创第一次出现的位置（索引）。没有返回-1
str.indexOf('a') //结果是0

2，lastIndexOf()，返回指定字符串最后一次出现的位置。
str.lastIndexOf('f') //结果是22
 
3，substring()，提取字符串中两个指定索引号之间的字符（两个索引不能为负值）
str.substring(0,5) //结果是abbca

4，slice()， 提取字符串中两个指定索引号之间的字符（索引可以为负值，-1就是倒数第二位）留头不留尾，原数组不改变
str.slice(0,-5) //结果是abbcalgldkjgldgehl

5，charAt()，返回指定索引的字符
str.charAt(1) //结果是b    chartCodeAt() 返回指定位置的unicode编码

6，concat()，连接两个或多个字符串，返回连接后的新字符串。
var str2 = "1";
str.concat(str2) //结果是abbcalgldkjgldgehlgjldf1

7，split()，把字符串分割为子字符串数组
str.split() //结果是["abbcalgldkjgldgehlgjldf"]



8，toString()，返回字符串对象，比如把数字转换成字符串对象。


9，trim()，移除字符串首位空格，经常在对input和textarea的值做判断时用到。



10，toLowerCase()，把字符串转换成小写的。



11，toUpperCase()，把字符串转换成大写的。



12，match()，找到一个或者多个子串或者正则表达式的匹配。



13，replace()，替换指定子串或者与正则表达式匹配的子串。（两个参数，（被替换的字符，新的字符）



14，search()，检索指定子串或者与正则表达式匹配的值，返回的值是数字，第一次出现的索引。


15， includes(), 查找字符串中是否包含指定字符



16， substr(起始索引，提取的长度),从起始索引提取字符串中指定数目的字符，原数组不变

17， substring(起始索引，结束的索引),提取字符串中两个指定索引之间的字符



```
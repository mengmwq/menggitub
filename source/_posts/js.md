<!--
 * @Author: your name
 * @Date: 2020-11-05 13:26:50
 * @LastEditTime: 2020-11-08 12:48:54
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \mengmwq.github.io\source\_posts\js.md
-->
### js 函数
# 函数实参和形参
 实参大于形参不会报错    小于形参  
```javascript
function sum (){
  var a =0;
  for(var i = 0;i<arguments;i++){
    a+=arguments[i]
  }
  console.log(a)
}
sum(1,3,4,5,7)
```
实参未传值  赋值  得到undefined 
```javascript
function test(a,b){
  b=3;
  console.log(argument[1]) //undefined
}
test(1)
```





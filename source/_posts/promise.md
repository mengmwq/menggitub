---
title: promise
---
# 区别实例对象和函数对象

1.  函数对象:函数作为对象使用，简称函数对象
2.  实例对象：new 函数产生的对象，简称实例对象
```javascript
function Fn(){  //fn函数
}
const fn = new Fn()  //Fn是构造函数  fn是 实例对象
Fn.prototype  //Fn是函数对象
Fn.bind({})
``` 
# promise是什么？
是js中进行异步编程的新的解决方案。从语法上说promise是一个构造函数，从功能上说，promise对象用来封装一个异步操作，并得到结果数据
# promise的状态改变
pending（等待）变为resoved （成功）    pending变为rejected（拒绝)
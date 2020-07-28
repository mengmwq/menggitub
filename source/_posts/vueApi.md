---
title: vueApi
cover: https://t7.baidu.com/it/u=1967400525,3328405026&fm=193
---
# vue api
vue 指令
# 1.v-text 更新元素的textcontent 如果要更新部分的texecontent 需要使用{{Mustache}}插槽
```javascript
<span v-text="msg"></span>
/* 和下面的一样 */
<span>{{msg}}</span>

```
# 2.v-html
更新元素的innerHtml插入
```javascript
<div v-html="html"></div>
```
# v-show
根据表达式的真假值，切换`dispaly ``css ``property`
当条件变化时改指令触发过渡效果

# v-if
根据表达式的值的 truthiness 来有条件地渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 <template>，将提出它的内容作为条件块。
当条件变化时该指令触发过渡效果
当v-if和v-for一起使用时，v-for的优先级比v-if更高

# v-else
前一兄弟元素必须有 v-if 或 v-else-if
```javascript
<div v-if="Math.random()>0.5">
now you see me
</div>
<div v-else>
 now you don't
</div>
```
# v-for
基于源数据多次渲染元素或模板块。此指令之值，必须使用特定语法 alias in expression，为当前遍历的元素提供别名：
```javascript
<div v-for="item in items">
 {{item.text}}
</div>
```
另外也可以为数组索引指定别名 (或者用于对象的键)：
```javascript
<div  v-for="(item, index) in items"></div>
<div  v-for="(val, key) in object"></div>
<div  v-for="(val, name,index) in object"></div>

```
v-for 的默认行为会尝试原地修改元素而不是移动它们。要强制其重新排序元素，你需要用特殊 attribute key 来提供一个排序提示：

```javascript
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

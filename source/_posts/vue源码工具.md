---
title: vue 源码分析 
cover: http://img5.imgtn.bdimg.com/it/u=1300146589,158997046&fm=11&gp=0.jpg
---

# 什么周期钩子
1. `beforeCreate`
在实例和初始化之后，数据观测（data observer)和（event/watcher）事件之前被调用
2. `created`
   在实例创建完成后立即调用，在这一步，实例已完成以下的配置：数据观测（data observer） property和方法的运算，watch/event事件回调，然而挂载阶段还灭开始，$el property目前尚不可用
3. beforeMount  在挂载开始之前被调用，相关的render函数首次被调用
4. mounted  实例被挂载之后调用，这时`el`被创建的`vm.$el`替换了，如果实例挂载到一个文档的元素上，当mounted被调用时vm.$el也在文档内的元素上，不会保证所有的子组件也一起被挂载，如果你希望等到整个视图都渲染完毕，可在mounted内部使用  vm.$nextTick
```javascript
mounted:function(){
  this.$nextTick(function(){
    //
  })
}
```
5. beforeUpdate   数据更新时调用，发生在虚拟dom打补丁之前，这里适合更新之前访问现有的dom 比如手动移除已经添加的事件监听器
6. updated  由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用计算属性或 watcher 取而代之。
7. activated 被 keep-alive 缓存的组件激活时调用。
8. deactivated  被 keep-alive 缓存的组件停用时调用。
9. beforeDestroy   实例销毁之前使用
10. destroyed  实例销毁之后被调用  对应vue实例的所有指令被解绑  所有事件监听被移除  所有的子实例也都被销毁
11. errorCaptured 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。
---
   title: vue  常见面试题  总结
   cover: https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=546984791,2625567179&fm=26&gp=0.jpg
---
# 对于MVVM的理解？

`MVVM `是 `Model-View-ViewModel` 的缩写。
`Model`代表数据模型，也可以在`Model`中定义数据修改和操作的业务逻辑。

`View`代表`UI` 组件，它负责将数据模型转化成`UI` 展现出来。

`ViewModel`监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步`View` 和` Model`的对象，连接`Model`和`View`。
在`MVVM`架构下，`view`和`Model`之间并没有直接的联系，而是通过`ViewModel`进行交互，`Model` 和 `ViewModel` 之间的交互是双向的， 因此`View` 数据的变化会同步到`Model`中，而`Model `数据的变化也会立即反应到`View` 上。

`ViewModel`通过双向数据绑定把 `View` 层和 `Model` 层连接了起来，而`View` 和 `Model` 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作`DOM`, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由` MVVM `来统一管理。

# Vue的生命周期

8个阶段创建前/后, 载入前/后,更新前/后,销毁前/销毁后。

生命周期：`Vue`实例从开始创建、初始化数据、编译模板、挂载`Dom`→渲染、更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期，各个阶段有相对应的事件钩子。

`beforeCreate`（创建前）    在数据观测和初始化事件还未开始

`created`（创建后）    完成数据观测，属性和方法的运算，初始化事件，`$el`属性还没有显示出来

`beforeMount`（载入前）    在挂载开始之前被调用，相关的`render`函数首次被调用。实例已完成以下的配置：编译模板，把`data`里面的数据和模板生成`html`。注意此时还没有挂载`html`到页面上。

`mounted`（载入后）    在`el` 被新创建的 `vm.$el` 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的`html`内容替换`el`属性指向的`DOM`对象。完成模板中的`html`渲染到`html`页面中。此过程中进行`ajax交互。

`beforeUpdate`（更新前）    在数据更新之前调用，发生在虚拟`DOM`重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。

`updated`（更新后）    在由于数据更改导致的虚拟`DOM`重新渲染和打补丁之后调用。调用时，组件`DOM`已经更新，所以可以执行依赖于`DOM`的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。

`beforeDestroy`（销毁前）    在实例销毁之前调用。实例仍然完全可用。

`destroyed`（销毁后）    在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

它的生命周期中有多个事件钩子，让我们在控制整个`Vue`实例的过程时更容易形成好的逻辑。
 
第一次加载页面  会触发 下面这几个`beforeCreate`, `created`, `beforeMount`, `mounted` 。`DOM `渲染在 `mounted` 中就已经完成了。

#  Vue实现数据双向绑定的原理：Object.defineProperty（）

`vue`实现数据双向绑定主要是：采用数据劫持结合发布者-订阅者模式的方式，通过`Object.defineProperty（）`来劫持各个属性的`setter`，`getter`，在数据变动时发布消息给订阅者，触发相应监听回调。当把一个普通 `Javascript` 对象传给` Vue `实例来作为它的 `data `选项时，`Vue` 将遍历它的属性，用 `Object.defineProperty` 将它们转为 `getter`/`setter`。用户看不到 `getter` `setter`，但是在内部它们让 `Vue`追踪依赖，在属性被访问和修改时通知变化。

# vue的数据双向绑定

将`MVVM`作为数据绑定的入口，整合`Observer，Compile`和`Watcher`三者，通过`Observer`来监听自己的`model`的数据变化，通过`Compile`来解析编译模板指令，最终利用`watcher`搭起`observe`r和`Compile`之间的通信桥梁，达到数据变化—>视图更新


# js实现简单的双向绑定
```javascript

  var obj = {}
  obj.defineProperty(obj,'txt',{get:function(){
     return obj
  }
  set:function(newValue){
    document.getElementById('txt').value = newValuedocument.getElementById('show').innerHTML = newValue       

  }
  document.addEventListener('keyup',function(e){        obj.txt = e.target.value    })
  
  
  })
```

# Vue组件间的参数传递

父组件传给子组件：子组件通过`props`方法接受数据;


```javascript
// 父组件
<template>

<center-template :form='userinfo'></center-template>
</template>
<script>
import CenterTemplate from '../../components/admin/userCenterTemplate'

export default {
    components: {
        CenterTemplate
    },
    data () {
        return {
            userinfo: {name: 'jack'}
        }
    },    
}
</script>
// 子组件

export default {
    props: {
    // 接收
        form: {
            type: Object,//类型是一个对象或者数组时，默认值必须是函数
            default: {
                name: '',
            }， // 默认值
           required：true,//是否必传
        }
    },
}
```

子组件传给父组件：`$emit`方法传递参数
# 直接在子元素中调用父元素的方法
```javascript
  //子元素
  <el-input  @input="changeInput"  class="search-input"  v-model="searchvalue" palcehoder="请输入"  ></el-input>

  changeInput(){
    //z子元素调用父元素的方法
    if(this.$parent.nullSerarchResult){
        // 新增标签
        this.$parent.nullSerarchResult()
    }
  }
//父元素
<v-search ></v-search> 
nullSerarchResult(val){
  this.pageNum = 1
  this.init({
    searchKey:val
  })
}
```
 # 子元素使用$emit调用父元素方法

```javascript
 //子元素
<el-button  @click="addNewFun"   >新增</el-button>
addNewFun(){
  this.$emit(''addTagFun，1)
}

//父元素
 <v-search  @addTagFun="addTagFun"></v-search>
 addTagFun(){
   this.$router.push('/index)

 }


```

# 子元素中绑定父元素的方法名
```javascript
//子元素
<el-button @click="searchResultChild" size='small' class="search-btn" type="primary">搜索</el-button>


  props:{
    searchResult:{
      type:function,
      default:null
    }

  },
  searchResultChild(){
    //调用方法2，这种需要再props申明
    searchResult:{
    if(this.searchResult){
      this.searchResult(this.serachValue)
    }

    //父元素
    <v-search :searchResult="searchResult"></v-search>
    searchResult(val){
      this.pageNum = 1
        this.init({
            searchKey: val
        })
    }
```

# 使用event bus来实现组件间的通讯

```javascript
// common/bus.js

import Vue from 'vue';

// 使用 Event Bus
const bus = new Vue();

export default bus;

// 组件1(接收通知信号)

import bus from '@/common/bus.js'

export default{
    data(){
        return {
               collapseData: ''     
        }
    },
    created() {
      
        // 监听collapse，有变动就会收到通知，并改变collapseData值
        bus.$on('collapse', msg => {
            this.collapseData = msg
        })
    }
}

// 组件2 发布信号

import bus from '@/common/bus.js'

export default {
    methods: {
        sendData(){
            // 发布信号，触发这个函数，其他的接收函数都会收到相应的信息
            bus.$emit('collapse', '信息')
        }
    }
}

```

# 还有一种就是借用vuex传递信息
```javascript
// store/index.js
import Vue from 'vue'
import Vuex from 'vuex';


Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        userinfo: {},
    },
    
    mutations: {
        'SET_ROLES':(state,userinfo) => {
            state.userinfo = userinfo
        },
    },
    actions: {
        getUser({commit, state},userinfo){
            commit('SET_ROLES',userinfo)
        },
    }
})


export default store

// views/slider.vue



import { mapState, mapAcions } from 'vuex'

export default{
    computed: {
        // 解构赋值
        ...mapState({
            userinfo: state => state.userinfo
        }) 
    },
    methods: {
        ...mapActions({
            getUser: 'getUser'
        }),
        handleUserInfo(){
            // 调用
            this.$store.dispatch('getUser','对象')
        }
    }
}

```

# vue路由的钩子函数
首页可以控制导航跳转，`beforeEach`，`afterEach`等，一般用于页面title的修改，一般需要登录才能调整页面的重定向功能。
`beforeEach`主要有3个参数  `to` `from` `next`
`to` `router`即将进入的目标路由对象
`from` `router`当前导航正要离开的路由
`next` `function`一定要调用该方法的`resolve`这个钩子,执行效果依赖next方法的调用参数，可以控制网页的调转

# vuex 是什么？怎么使用？那种功能场景需要它?


只用来读取的状态集中放在`store`中； 改变状态的方式是提交`mutations`，这是个同步的事物； 异步逻辑应该封装在`action`中。

在`main.js`引入`store`，注入。新建了一个目录·store`，….. export 。

场景有：单页应用中，组件之间的状态、音乐播放、登录状态、加入购物车

# state
`Vuex` 使用单一状态树,即每个应用将仅仅包含一个`store` 实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据。

# mutations
 `mutations`定义 的方法动态修改`Vuex`的`store`中的状态或数据

 # getters
 类似vue的计算属性主要用于过滤数据

 # action
 `actions`可以理解为通过将`mutations`里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。`view `层通过` store.dispath` 来分发` action`。

```javascript
conststore = new Vuex.Store({ //store实例state:{count:0},mutations:{                increment(state) {state.count++}},actions:{ increment(context) {context.commit('increment')}}})
```
# modules

项目特别复杂的时候，可以让每一个模块拥有自己的`state`、`mutation`、`action`、`getters`,使得结构非常清晰，方便管理。
```javascript
constmoduleA = {state:{ ... },mutations:{ ... },actions:{ ... },getters:{ ... }}constmoduleB = {state:{ ... },mutations:{ ... },actions:{ ... }}conststore = new Vuex.Store({modules:{a:moduleA,b:moduleB})

```
# vue-cli如何新增自定义指令？
# 创建局部指令
```javascript

varapp =newVue({    el:'#app',    data: {        },// 创建指令(可以多个)directives: {// 指令名称dir1: {            inserted(el) {// 指令中第一个参数是当前使用指令的DOMconsole.log(el);console.log(arguments);// 对DOM进行操作el.style.width ='200px';                el.style.height ='200px';                el.style.background ='#000';            }        }    }})

```

# 全局指令
```javascipt
Vue.directive('dir2', {    inserted(el) {console.log(el);    }})
```

# 对keep-alive 的了解？

`keep-alive`是 `Vue` 内置的一个组件，可以使被包避免重含的组件保留状态，或新渲染。

在`vue 2.1.0 `版本之后，`keep-alive`新加入了两个属性:` include`(包含的组件缓存) 与 `exclude`(排除的组件不缓存，优先级大于`include`) 。

# v-if 和 v-show 区别

v-if按照条件是否渲染，v-show是display的block或none；

# $route和$router的区别

`$route`是“路由信息对象”，包括`path`，`params`，`hash`，`query`，`fullPath`，`matched`，`name`等路由信息参数。而`$router`是“路由实例”对象包括了路由的跳转方法，钩子函数等。

# vue.js的两个核心是什么？

数据驱动、组件系统

# .vue几种常用的指令
 `v-for`  ` v-if`  `v-bind ` `v-on` ` v-show ` ` v-else`
# vue常用的修饰符？
`prevent`: 提交事件不再重载页面；.`stop`: 阻止单击事件冒泡；.`self`: 当事件发生在该元素本身而不是子元素的时候会触发；`.capture`: 事件侦听，事件发生的时候会调用

# .v-on 可以绑定多个方法吗？
 可以 
```javascript
<p v-on="{click:dbClick,mousemove:MouseClick}"></p>
```
# vue中 key 值的作用？

当 `Vue.js`用` v-for `正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，`Vue `将不会移动` DOM `元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。`key`的作用主要是为了高效的更新虚拟

# 什么是vue的计算属性？
在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。好处：①使得数据处理结构清晰；②依赖于数据，数据更新，处理结果自动更新；③计算属性内部this指向vm实例；④在template调用时，直接写计算属性名即可；⑤常用的是`getter`方法，获取数据，也可以使用`set`方法改变数据；⑥相较于`methods`，不管依赖的数据变不变，`methods`都会重新计算，但是依赖数据不变的时候`computed`从缓存中获取，不会重新计算。


# vue等单页面应用及其优缺点
优点：`Vue` 的目标是通过尽可能简单的 `API` 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。

缺点：不支持低版本的浏览器，最低只支持到IE9；不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；第一次加载首页耗时相对长一些；不可以使用浏览器的导航按钮需要自行实现前进、后退。


# active-class是哪个组件的属性？嵌套路由怎么定义？
vue-router模块的router-link组件。

# 怎么定义`vue-router`的动态路由？怎么获取传过来的动态参数？ 
在`router`目录下的`index.js`文件中，对`path`属性加上`/:id`。  使用`router`对象的`params.id`

# scss是什么？安装使用的步骤是？有哪几大特性？

预处理`css`，把`css`当前函数编写，定义变量,嵌套。 先装`css-loader`、`node-loader`、`sass-loader`等加载器模块，在`webpack-base.config.js`配置文件中加多一个拓展:`extenstion`，再加多一个模块：`module`里面`test`、`loader`


# 路由的传参和取参
1.查询参
配置（传参） 
```javascript
:to = "{name：'login',query:{id:loginid}}"
取参  this.$route.query.id
```


2.路由参

配置
```javascript
  ：to ="{name:'resgister',params:{id:degisterid}"\
 // 配置路由的规则
  {name：'details',path:'/details/:id'}
  //获取
  this.$route.params.id

```
```javascript
var router = new VueRouter({
    routers:[
        {path:'/login',name:'login',components:Login},
        {path:'/restiger/:foo',name:'restiger',components:Restiger}
         {path:'/restiger/:foo',name:'restiger',props：true，components:Restiger}
    ]
})
<router-link  
:to="{name:'login',query:{id:'123'}}">去登陆 </router-link>
```
```javascript
<router-link :to="{name:'restiger',params:{foo:'bar'}}">去注册</router-link>


```
#获取
```javascript 
this.$route.query.id
```
```javascript
//路由参数
this.restigerfoo = this.$route,params.foo
prop:[foo]

```

# 父子组件的访问方式
子访问父  $parents

# slot  插槽
1. 匿名插槽
```javascript
<div>
<slot></slot>
</div>
```















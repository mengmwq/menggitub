---
title: vue Router 进阶
cover: http://img5.imgtn.bdimg.com/it/u=1300146589,158997046&fm=11&gp=0.jpg
---
# 导航守卫

导航表示路由正在发生改变。`vue-router`提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的，单个路由独享的，或者组件级的。
参数或查询的改变并不会触发进入/离开的导航守卫。你可以通过观察`$route`对象来应对这些变化，或使用`beforeRouteUpdate`的组件内守卫。

# 全局前置守卫
可以使用`router.beforeEach`注册一个全局前置守卫。
```javascript
const router = new VueRouter({...})
router.beforeEach((to,form,next)=>{
  //....
})
```
当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫resolve完之前一直处于等待中。
每个守卫方法接收三个参数：
`to:Route`：即将要进入的目标路由对象
`from:Route`：当前导航正要离开的路由
`next:Function`：一定要调用该方法来`resolve`这个钩子。执行效果依赖`next`方法的调用参数。
`next()`: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是confirmed（确认的）。
`next(false)`：中断当前的导航。如果浏览器的URL改变了（可能是用户手动或者浏览器后退按钮），那么URL地址会重置到from路由对应的地址。
`next('/')`或者`next({path:'/'})`：跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向next传递任意位置对象，且允许设置诸如`replace:true、name:'home'`之类的选项以及任何用在`router-link`的`to prop`或`router.push`中的选项。
`next(error)`：如果传入next的参数是一个`Error`实例，则导航会被终止且该错误会被传递给`router.onError()`注册过的回调。
确保要调用`next`方法，否则钩子就不会被`resolved`。

# 全局解析守卫
可以用`router.beforeResolve`注册一个全局守卫。这和`router.beforeEach`类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。

# 完整的导航解析流程
导航被触发。
在失活的组件里调用离开守卫。
调用全局的beforeEach守卫。
在重用的组件里调用beforeRouteUpdate守卫。
在路由配置里调用beforeEnter。
解析异步路由组件。
在被激活的组件里调用beforeRouteEnter。
调用全局的beforeResolve守卫。
导航被确认。
调用全局的afterEach钩子。
触发DOM更新。
用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。


# 路由元信息
定义路由的时候可以配置meta字段。
```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```
我们称呼`routes`配置中的每个路由对象为路由记录。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录。
例如，根据上面的路由配置，`/foo/bar`这个URL将会匹配父路由记录以及子路由记录。
一个路由匹配到的所有路由记录会暴露为`$route`对象（还有在导航守卫中的路由对象）的`$route.matched`数组。因此，我们需要遍历$route.matched来检查路由记录中的meta字段。
下面例子展示在全局导航守卫中检查元字段。
```javascript
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

# 过渡动效

<router-view>是基本的动态组件，所以我们可以用<transition>组件给它添加一些过渡效果。

```javascript
<transition>
  <router-view></router-view>
</transition>

```

# 单个路由的过渡
上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用<transition>并设置不同的name。
```javascript
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}
const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}


```

# 基于路由的动态过渡
还可以基于当前路由与目标路由的变化关系，动态设置过渡效果：
```javascipt
<!-- 使用动态的 transition name -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
```

```javascript
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
```
# 路由懒加载
当打包构建应用时，Javascript包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。
结合Vue的异步组件和Webpack的代码分割功能，轻松实现路由组件的懒加载。
首先，可以将异步组件定义为返回一个Promise的工厂函数 (该函数返回的Promise应该resolve组件本身)：
```javascript
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })

```
第二，在 Webpack中，我们可以使用动态import语法来定义代码分块点：
```javascript
import('./Foo.vue') // 返回 Promise

```
注意：如果您使用的是Babel，你将需要添加syntax-dynamic-import插件，才能使Babel可以正确地解析语法。

结合这两者，这就是如何定义一个能够被Webpack自动代码分割的异步组件。
```javascript
const Foo = () => import('./Foo.vue')

```
在路由配置中什么都不需要改变，只需要像往常一样使用Foo：
```javascript
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```


# 把组件按组分块
有时候我们想把某个路由下的所有组件都打包在同个异步块(chunk)中。只需要使用命名chunk，一个特殊的注释语法来提供chunk name。

```javascript

const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
Webpack会将任何一个异步模块与相同的块名称组合到相同的异步块中。
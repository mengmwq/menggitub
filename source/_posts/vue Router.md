---
title: vue router基础
cover: http://img5.imgtn.bdimg.com/it/u=1300146589,158997046&fm=11&gp=0.jpg
---
# 安裝
```javascript
 npm   install vue-router
```

```javascript
  import Vue from 'vue'
  import VueRouter from 'vue-router'

  Vue.use(VueRouter)

```

# 介紹
 
嵌套的路由/視圖表 
模块化的、基于组件的路由配置
路由参数、查询、通配符
基于Vue.js过渡系统的视图过渡效果
带有自动激活的CSSclass的链接
HTML5历史模式或hash模式，在IE9中自动降级
自定义的滚动条行为
基于Vue.js过渡系统的视图过渡效果
# 起步

用Vue.js + Vue Router创建单页应用，是非常简单的。使用Vue.js，我们已经可以通过组合组件来组成应用程序，当你要把Vue Router添加进来，我们需要做的是，将组件映射到路由，然后告诉Vue Router在哪里渲染它们。下面是个基本例子：
```javascript
  <div id="app">
    <h1>Hello App!</h1>
    <p>
      <!--使用router-link组件来导航-->
      <!--通过传入to属性指定链接-->
      <!--<router-link>默认会被渲染成一个<a>标-->
      <router-link to="/foo">Go to Foo</router-link>
      <router-link to="/bar">Go to Bar</router-link>
    </p>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
```
```javascript
    // 0.如果使用模块化机制编程，导入Vue和VueRouter，要调用Vue.use(VueRouter)
    // 1.定义（路由）组件
    // 可以从其他文件import进来
    const Foo = { template: '<div>foo</div>' }
    const Bar = { template: '<div>bar</div>' }
    // 2.定义路由
    const routes = [
      { path: '/foo', component: Foo },
      { path: '/bar', component: Bar }
    ]
    // 3.创建router实例，然后传routes配置
    // 你还可以传别的配置参数
    const router = new VueRouter({
      routes //（缩写）相当于routes:routes
    })
    // 4.创建和挂载根实例。
    // 记得要通过router配置参数注入路由，从而让整个应用都有路由功能
    const app = new Vue({
      router
    }).$mount('#app')
    // 现在，应用已经启动了！

```

通过注入路由器，我们可以在任何组件内通过`this.$router`访问路由器，也可以通过`this.$route`访问当前路由
```javascript
// Home.vue
export default {
  computed: {
    username () {
      return this.$route.params.username
    }
  },
  methods: {
    goBack () {
      window.history.length > 1
        ? this.$router.go(-1)
        : this.$router.push('/')
    }
  }
}

```
　
当`<router-link>`对应的路由匹配成功，将自动设置`class`属性值`.router-link-active`。

# 动态路由匹配
我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个`User`组件，对于所有`ID`各不相同的用户，都要使用这个组件来渲染。那么，我们可以在`vue-router`的路由路径中使用动态路径参数来达到这个效果。
```javascript
    const User = {
      template: '<div>User</div>'
    }
    const router = new VueRouter({
      routes: [
        // 动态路径参数 以冒号开头
        { path: '/user/:id', component: User }
      ]
    })
```
现在像`/user/foo`和`/user/bar`都将映射到相同的路由。
一个路径参数使用冒号:标记。当匹配到一个路由时，参数值会被设置到`this.$route.params`，可以在每个组件内使用。于是，我们可以更新`User`的模板，输出当前用户的`ID`。
```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

模式 | 匹配路徑 |  $router.params  
-|-|-
/user/:username | 	/user/evan | { username: 'evan' }|
/user/:username/post/:post_id |/user/evan/post/123 |{ username: 'evan', post_id: 123 }|

# 响应路由参数的变化
当使用路由参数时，例如从/user/foo导航到/user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。
复用组件时，想对路由参数的变化作出响应的话，你可以简单地watch（监测变化）$route对象。
```javascript
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```
或者使用`beforeRouteUpdate`守卫。
```javascript

 const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```
# 捕获所有路由或 404 Not found 路由
常规参数只会匹配被/分隔的URL片段中的字符。如果想匹配任意路径，我们可以使用通配符(*)：
```javascript
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
```
当使用通配符路由时，请确保路由的顺序是正确的，也就是说含有通配符的路由应该放在最后。路由{ path: '*' }通常用于客户端404错误。如果你使用了History模式，请确保正确配置你的服务器。

当使用一个通配符时，```$route.params```内会自动添加一个名为```pathMatch```参数。它包含了URL通过通配符被匹配的部分：

```javascript
// 给出一个路由 { path: '/user-*' }
this.$router.push('/user-admin')
this.$route.params.pathMatch // 'admin'
// 给出一个路由 { path: '*' }
this.$router.push('/non-existing')
this.$route.params.pathMatch // '/non-existing'

```

# 高级匹配模式
`vue-router`使用`path-to-regexp`作为路径匹配引擎，所以支持很多高级的匹配模式，例如：可选的动态路径参数、匹配零个或多个、一个或多个，甚至是自定义正则匹配。

# 嵌套路由
实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL中各段动态路径也按某种结构对应嵌套的各层组件。

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```
这里的`<router-view>`是最顶层的出口，渲染最高级路由匹配到的组件。同样地，一个被渲染组件同样可以包含自己的嵌套`<router-view>`。例如，在User组件的模板添加一个`<router-view>`。


```javascript
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
```
要在嵌套的出口中渲染组件，需要在VueRouter的参数中使用children配置。
```javascript

const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        {
          // 当 /user/:id/profile匹配成功，
          // UserProfile会被渲染在User的<router-view>中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts匹配成功
          // UserPosts会被渲染在User的<router-view>中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```
要注意，以/开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。

`children`配置就是像`routes`配置一样的路由配置数组，所以可以嵌套多层路由。

此时，基于上面的配置，当你访问`/user/foo`时，`User`的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个空的子路由。
```javascript

const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome },
        // ...其他子路由
      ]
    }
  ]
})
```

# 命名路由
有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。可以在创建`Router`实例的时候，在`routes`配置中给某个路由设置名称。


```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由，可以给router-link的to属性传一个对象。

```javascript
<router-link :to="{name:'user',params: { userId:123 }}">User</router-link>

```
这跟代码调用router.push()是一回事。
```javascript
router.push({ name:'user',params: { userId:123 }})

```
这两种方式都会把路由导航到/user/123路径。

# 重定向和别名
重定向也是通过routes配置来完成，下面例子是从/a重定向到/b。
```javascript
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```
重定向的目标也可以是一个命名的路由。
```javascript
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```
注意导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上。在下面这个例子中，为/a路由添加一个beforeEach或beforeLeave守卫并不会有任何效果。


# 路由组件传参
在组件中使用`$route`会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的URL上使用，限制了其灵活性。
使用`props`将组件和路由解耦：
# 取代与$route的耦合
```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```
# 通过props解耦
```javascript
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },
    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})

```

# 布尔模式
如果`props`被设置为`true`，`route.params`将会被设置为组件属性。

# 对象模式
如果`props`是一个对象，它会被按原样设置为组件属性。当props是静态的时候有用。
```javascript
const router = new VueRouter({
  routes: [
    { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]
})
```
# HTML5 History模式
`vue-router`默认`hash`模式——使用URL的hash来模拟一个完整的URL，于是当URL改变时，页面不会重新加载。
如果不想要`hash`，我们可以用路由的`history`模式，这种模式充分利用`history.pushState API`来完成URL跳转而无须重新加载页面。`



当你使用`history`模式时，URL就像正常的`url`，例如`http://yoursite.com/user/id`。

不过这种模式还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问`http://oursite.com/user/id`就会返回404，这就不好看了。
所以要在服务端增加一个覆盖所有情况的候选资源：如果URL匹配不到任何静态资源，则应该返回同一个`index.html`页面，这个页面就是你`app`依赖的页面。
这么做以后，你的服务器就不再返回`404`错误页面，因为对于所有路径都会返回`index.html`文件。为了避免这种情况，你应该在Vue应用里面覆盖所有的路由情况，然后在给出一个404页面。
```javascript
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```
---
title: 从  0    搭理vue后台管理系统
cover : https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=3809780941,1211248538&fm=26&gp=0.jpg
---
# 从  0    搭理后台管理系统

# 初始化项目

```javascript
  vue create vue-admin
```
提示选择一个preset （预置项）

{% asset_img vue1.png %}

选择自自定义选项
{% asset_img vue2.png %}
路由是否使用History模式？Y
{% asset_img vue3.png %}
选择一种CSS预处理：Less （如果安装Sass/SCSS，需要本地预先安装python）
{% asset_img vue4.png %}
选择一种代码格式化规范类型：ESLint + Standard config
{% asset_img vue5.png %}
保存时检测 还是 提交时检测并修复：Lint on save
{% asset_img vue6.png %}
将以上配置存放到哪里？In dedicated config files （单独文件中）
{% asset_img vue7.png %}
是否保存为preset（预设项，以后可以使用的,就不用像这样再一步一步来）：Y

{% asset_img vue8.png %}
保存preset为（给预置项个名字）：All
{% asset_img vue9.png %}
之后系统自动安装项目并生成preset（预置项）
{% asset_img vue10.png %}
# 修改项目目录
{% asset_img vue11.png %}
## 说明
{% asset_img vue12.png %}
# 路由配置
路由配置承担着重要的作用，它影响着如下内容：

左侧菜单内容
是否缓存该页
该页面图标（显示在菜单、面包屑和Tab标签）
接下来来看如何配置路由。
路由的配置是在```'./src/router'```文件夹下，```'./src/router/index.js'```文件中定义路由拦截的逻辑，```'./src/router/routers.js'```文件中定义页面路由信息。
##路由可配项
路由中的```meta```除了原生参数外可配置的参数:
```javascript
meta:{
  hideInMenu:(default:false)设置为true 再左侧栏不会显示该页面选项
  showAlways: (default: false) 设为true后如果该路由只有一个子路由，在菜单中也会显示该父级菜单
  notCache: (default: false) 设为true后页面不会缓存
  access: (default: null) 可访问该页面的权限数组，当前路由设置的权限会影响子路由
  icon: (default: -) 该页面在左侧菜单、面包屑和标签导航处显示的图标，如果是自定义图标，需要在图标名称前加下划线'_'
  href: 'https://xxx' (default: null) 用于跳转到外部连接
}
```
# 跳转到其他网页，在新窗口打开
你可以在菜单栏中显示一个任意连接，并且点击该菜单项会打开新窗口展示链接到的页面。在路由中需要如下定义：
```javascript
export default [
  {
    path: '',
    name: 'lison16',
    meta: {
      href: 'https://wsq01.github.com/',
      icon: '_love',
      title: 'WSQ的github首页'
    }
  }
]
```
# 在独立页面展示的页面
如登录页、错误页这种独立的页面，无需使用Layout组件，则直接定义在routers.js中export default []的数组中，如登录页的定义如下：
```javascript
export default [
  {
    path: '/login', // 必须项
    name: 'login', // 必须项，后面缓存页面需要用到，且这个name是唯一的
    meta: {
      title: 'Login - 登录',
      hideInMenu: true // 是否在左侧菜单中隐藏，默认为false
    },
    component: () => import('@/view/login/login.vue')
  },
  /* 其他路由 */
]
```
# 在Main组件展示区域中展示的页面
页面作为多Tab页展示，在Layout组件的视图区域显示，则需要在用到子路由，定义如下：
```javascript
xport default [
    {
    path: '/',
    name: '_home',
    redirect: '/home',
    component: Main,
    meta: {
      hideInMenu: true,
      notCache: true
    },
    children: [
      {
        path: '/home',
        name: 'home',
        meta: {
          hideInMenu: true,
          title: '首页',
          notCache: true,
          icon: 'md-home'
        },
        component: () => import('@/view/single-page/home')
      }
    ]
  },
  {
    path: '/multilevel',
    name: 'multilevel', // 一级目录
    meta: {
      icon: 'arrow-graph-up-right',
      title: '多级菜单'
    },
    component: Main, // 一级目录必须使用Main组件作为component
    children: [
      {
        path: 'level_2_1',
        name: 'level_2_1', // 一级目录下的二级页面
        meta: {
          icon: 'arrow-graph-up-right',
          title: '二级-1'
        },
        component: () => import('@/view/multilevel/level-1.vue') // 这引入的是页面单文件
      },
      {
        path: 'level_2_2',
        name: 'level_2_2',
        meta: {
          access: ['super_admin'], // 权限控制<Array>，包含可访问该页面的用户权限
          icon: 'arrow-graph-up-right',
          title: '二级-2'
        },
        component: parentView, // 如果该路由不是页面，而是二级即更多级目录，需要用parentView组件
        children: [
          {
            path: 'level_2_2_1',
            name: 'level_2_2_1',
            meta: {
              icon: 'arrow-graph-up-right',
              title: '三级'
            },
            component: () => import('@/view/multilevel/level-2/level-2-1.vue')
          }
        ]
      }
    ]
  },
  {
    path: '/401',
    name: 'error_401',
    meta: {
      hideInMenu: true
    },
    component: () => import('@/view/error-page/401.vue')
  },
  {
    path: '/500',
    name: 'error_500',
    meta: {
      hideInMenu: true
    },
    component: () => import('@/view/error-page/500.vue')
  },
  {
    path: '*',
    name: 'error_404',
    meta: {
      hideInMenu: true
    },
    component: () => import('@/view/error-page/404.vue')
  }
]
```
# 路由拦截
在```./src/router/index.js```文件中，你可以配置路由跳转时的路由守卫。你可以设置文件中的```LOGIN_PAGE_NAME```常量，其默认值是```login```，这个就是路由列表中你的登录页面的```name```值，如果你的登录页面```name```值为```login```，则无需修改。









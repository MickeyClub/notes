# 路由

官网: https://router.vuejs.org/zh/

[TOC]

官网：https://router.vuejs.org/zh/

## 什么是路由

1. 后端路由: 对于普通的网站,所有超链接都是URL地址,所有的URL地址都对于服务器上对于的资源
2. 前端路由: 对于单页面应用程序来说.主要通过URL中的hash(#号)来实现不同页面之间的切换,同时hash有一个特点: HTTP请求中不会包含hash相关的内容;所以单页面程序中的页面跳转主要用hash实现;
3. 在单页面应用程序中,这种通过hash改变来切换页面的方式,称作前端路由(区别于后端路由)
4. 前端路由两种实现方式
   - HTML5 的 `History `模式，它使 url 看起来像普通网站那样，以“／”分剖，没有＃，但页面并没有跳转，不过使用这种模式需要服务端支持
   - 主要通过URL中的hash(#号)来实现

## URL中的hash

地址: https://www.cnblogs.com/joyho/articles/4430148.html

## SPA（单页面应用）

Single Page Application

- 只有一张Web页面的应用，是一种从Web服务器加载的富客户端，单页面跳转仅刷新局部资源 ，公共资源(js、css等)仅需加载一次，常用于PC端官网、购物等网站 
- Vue中一般是结合 Vue-router 来实现
- 其实就是在前后端分离的基础上，加一层前端路由。  

## 在Vue中使用vue-router

1. 导入vue-router组件类库:

   ```javascript
   <!-- 路由组件类库 -->
   <script src="./lib/vue-router-3.0.1.js"></script>
   ```

2. 使用roter-link组件来导航

   ```javascript
   <router-link to="/login'">登录</router-link>
   <router-link to="/register">注册</router-link>
   ```

3. 使用router-view组件显示匹配到的组件

   ```javascript
   <router-view></router-view>
   ```

4. 基本使用

   ```javascript
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <meta http-equiv="X-UA-Compatible" content="ie=edge">
       <title>Document</title>
       <script src="./lib/vue.js"></script>
       <!-- 路由组件类库 -->
       <script src="./lib/vue-router-3.0.1.js"></script>
       <style>
           .router-link-active,
           .myactive {
               text-decoration: none;
               color: red;
               font-size: 20px;
               font-weight: 700;
           }
   
           /* 动画效果 */
           .v-enter,
           .v-leave-to {
               opacity: 0;
               transform: translateX(140px)
           }
   
           .v-enter-active,
           .v-leave-active {
               transition: all .5s ease;
           }
       </style>
   </head>
   
   <body>
       <div id="app">
           <!-- 如果在路由中,使用查询字符串给路由传递参数,则不需要修改路由规则的path属性 -->
   
           <!-- 路由传参: 使用query方式   this.$route.query.id -->
           <router-link to="/login?id=10&name='mickey'" tag="a">登录</router-link>
           <!-- 路由传参: 使用params   this.$route.params.id -->
           <router-link to="/register/12/mickey">注册</router-link>
   
           <!-- router-view标签是vue-router路由提供的元素,专门用来当做占位符,根据路由规则匹配展示对应的组件
               所有可以把router-view当做是一个占位符
           -->
           <!-- 添加动画 -->
           <transition mode="out-in">
               <router-view></router-view>
           </transition>
       </div>
   
       <script>
           // 组件模板对象
           var login = {
               template: '<h1>登录组件---参数id:{{$route.query.id}}---参数name:{{$route.query.name}} </h1>',
               created() {
                   console.log(this.$route)
                   console.log(this.$route.query.id)
               },
           }
   
           var register = {
               template: '<h1>注册组件</h1>',
               created() {
                   console.log(this.$route)  // 路由规则的对象
                   console.log(this.$route.params)
                   console.log(this.$route.params.id)
               }
           }
   
           // 创建一个路由对象 
           // nwe Router中 可以传递一个配置对象
           let routerObj = new VueRouter({
               // 注意: route是路由匹配规则
               routes: [
                   /*
                       每个路由匹配规则都是一个对象, 必须有两个属性
                       属性一: path  表示监听那个路由链接地址
                       属性二: component 表示如果匹配到path,则展示component对应的组件
                       注意: component的属性值,必须是一个组件的模板对象,不能是组件的引用名称
                   */
                   {path: '/',redirect: '/login'},  // 重定向
                   {path: '/login',component: login},
                   {path: '/register/:id/:name',component: register}, //获取params传递的参数
               ],
               // 设置连接激活时使用css类,默认通过路由钩选项 linkActiveCalss 来全局匹配
               linkActiveCalss: 'myactive'
           })
   
           // vm实例
           let vm = new Vue({
               el: "#app",
               // 将路由规则对象,注册到vm实例上,用来监听URL地址的变化,展示对应的组件
               router: routerObj
           })
       </script>
   </body>
   </html>
   ```

## 设置路由重定向

重定向: `redirect`

例如: **{path: '/', redirect: '/login'}**

```javascript
const router = new VueRouter({
    // 路由匹配规则
    routes: [
        {path: '/', redirect: '/login'},
        {path: '/login', component: login},
        {path: '/register/:post', component: register}
    ],
    linkActiveClass: 'myactive'
})
```

## 路由传参（路由规则params）

1. 使用query方式   this.$route.query.id 
2. 使用params   this.$route.params.id 

```javascript
<!-- 路由传参: 使用query方式   this.$route.query.id -->
<router-link to="/login?id=10&name=mickey" tag="a">登录</router-link>
<!-- 路由传参: 使用params   this.$route.params.id -->
<router-link to="/register/12/mickey">注册</router-link>

const router = new VueRouter({
    // 路由匹配规则
    routes: [
        {path: '/', redirect: '/login'},
        // 如果在路由中,使用查询字符串给路由传递参数,则不需要修改路由规则的path属性 query方式就不用修改path
        {path: '/login', component: login},
		// params传参
        {path: '/register/:post', component: register}
    ],
    linkActiveClass: 'myactive'
})
```

## 嵌套路由

官网地址: https://router.vuejs.org/zh/guide/essentials/nested-routes.html

设置 `children`  配置,注意, `children` 下的path前面不要带 `/`  否则**以 / 开头的嵌套路径会被当作根路径开始请求,这样不方面用力理解url地址**  

基本使用: 

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .header {
            width: 100%;
            height: 50px;
            line-height: 50px;
        }

        .header a {
            padding: 10px;
            background-color: skyblue;
            color: white;
            font-weight: 700;
        }

        .index {
            height: 400px;
            background-color: hotpink;
        }

        .userInfo {
            height: 400px;
            background-color: yellowgreen;
            display: flex;
        }
        .userInfo .left{
            height: 100%;
            width: 100px;
            background-color: yellow;
        }
        .userInfo .left ul{
            padding: 0;
            list-style: none;
            text-align: center;
        }
        .userInfo .left li{
            padding:20px 10px;
        }
        .userInfo .left ul a{
            background-color: yellowgreen;
            color:white;
            font-weight: 700;
        }

        .userInfo .right{
            flex:1;
            height: 100%;
            background-color: orange;
        }
        .userInfo .right {
            font-size: 300px;
            text-align: center;
        }
    </style>
</head>

<body>
    <div id="app">
        <!-- 顶部通栏 -->
        <div class="header">
            <router-link to="/index">首页</router-link>
            <router-link to="/userInfo">用户中心</router-link>
        </div>
        <!-- 视图的出口 -->
        <router-view></router-view>
    </div>
</body>

</html>
<!-- 导入Vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 导入VueRouter.js -->
<script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>
<script id='userInfo' type="text/html">
    <div class="userInfo">
        <!-- 左 -->
        <div class="left">
            <ul>
                <li><router-link to="/userInfo/info">账户资料</router-link></li>
                <li><router-link to="/userInfo/icon">用户头像</router-link></li>
                <li><router-link to="/userInfo/charge">充值</router-link></li>
            </ul>
        </div>
        <!-- 右 -->
        <div class="right">
            <!-- 视图出口 -->
            <router-view></router-view>
        </div>
    </div>
</script>
<script>
    // 首页组件
    let index = {
        template: '<div class="index">我是首页</div>'
    }
    let userInfo = {
        template: '#userInfo'
    }

    // 嵌套路由中的子组件
    let info = {
        template: '<div>🎅</div>'
    }
    let icon = {
        template: '<div>😱</div>'
    }
    let charge = {
        template: '<div>🔨</div>'
    }

    // 用户中心组件
    // 创建路由规则
    let routes = [{
            path: '/index',
            component: index
        },
        {
            path: '/userInfo',
            component: userInfo,
            // 写children
            children: [{
                    path: '',
                    component: info
                }, {
                    path: 'info',
                    component: info
                },
                {
                    path: 'icon',
                    component: icon
                },
                {
                    path: 'charge',
                    component: charge
                },
            ]
        },
    ]

    // 实例化路由对象
    let router = new VueRouter({
        // 挂载路由规则
        routes
    })

    // 挂载到Vue实例上
    let app = new Vue({
        el: "#app",
        // 挂载路由
        router
    })
</script>
```



###  导航守卫-----vue-router 导航钩子功能

 vue-router 提供了导航钩子

路由(页面)跳转时触发,方便实现一些全局的功能，而且便于维护    

导航钩子有 3 个参数：

- to 即将要进入的目标的路由对象 。
- from 当前导航即将要离开的路由对象 。
- next 调用该方法后，才能进入下一个钩子,next的参数设置为 false 时，可以取消导航    

beforeEach 和 afterEach ， 它们会在路由即将改变前和改变后触发.

#### 路由元信息

路由中 设置一个 meta属性
	这个属性名不能乱写
meta属性设置的值
	根据自己的需求 可以进行修改

使用场景: 

1. 设置不同页面设置页面标题 (路由元信息)

```javascript
const routes = [{
    path: "/",
    component: index,
    meta: {
      title: '首页'
    }
  },
  {
    path: "/find",
    component: find,
    meta: {
      title: '发现'
    }
  }]

router.beforeEach((to, from, next) => {
  window.document.title = to.meta.title;
  next();
})
```



2. **后置钩子函数功能**(返回顶部):一个页面较长，滚动到某个位置，再跳转到另一个页面，滚动条默认是在上一个页面停留的位置，而好的体验肯定是能返回顶端

``` javascript
// 放回顶部  // 后置钩子函数
router.afterEach((to, from, next) => {
    // 跳转完毕之后执行
  window.scrollTo(0, 0)
  next();
})    				
```

3. 校验页面是否登录

``` javascript
// 放回顶部
router.beforeEach((to, from, next) => {
    if(window.localStorage.getItem('token')){
        next();
    }else{
        next('/login')
    }
})    
```

4. 从一个页面过渡到另一个页面时，可以出现一个全局的 Loading 动画，等 到新页面加载完后再结束动画。    



### router的this.$router其他方法

$router 还有其他一些方法:

\- replace

类似于<router-link>的 replace 功能 ，它不会向 history 添加新记录，而是替换掉当前的

history 记 录，如 this.$router.replace （'/user/123'）

\- go

类似于 window.history.go（），在 history 记录中向前或者后退多少步，参数是整数，例如:

// 后退 1 页

this.$router.go(-1);

// 前进 2 页

this.$router.go(2);



axios用过没,

用过,当时做登录功能模块的时候搞死我了,

发送请求


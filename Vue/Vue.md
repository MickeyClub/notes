# Vue

[TOC]

## 1. 数据常驻(localStorage)

```javascript
// 读取本地localStorage数据
let todoList = JSON.parse(localStorage.getItem("todoList")) || []
// 注册页面关闭时间,存取localStorage数据
window.onunload = function () {
    localStorage.setItem("todoList",JSON.stringify(todoList))
}
```

## 2. axios&vue-resource

### 2.1 axios

文档: https://www.kancloud.cn/yunye/axios/234845

是现在更流行的 在vue中 发ajax请求的js库

具备兼容性 错误处理

资源: https://unpkg.com/axios/dist/axios.min.js 

```javascript
axios.get('http://wthrcdn.etouch.cn/weather_mini?city=' + this.city).then((response) => {
    // 保存数据
    console.log(response)
    this.weatherList = response.data.data.forecast
})
```

- 设置axios基路径 
  - 默认就会在请求接口是自动添加基路径
  - 一般添加到vue原型中 `Vue.prototype.$axios = axios`
  - axios.defaults.baseURL = 'http://111.230.232.110:8899'; 

### 2.2 vue-resource

> vue-resource 是一个 vue的插件 往vue示例中 增加一个 $http属性
>
> ​    所有的网络请求 都是通过$http来使用
>
> ​    先导入vue 再倒入 vue-resource
>
> ​    早期 vue中用来调用接口的 js库
>
> ​    vue的作者直接取消 对于这个库的推荐了
>
> ​        只能结合 vue使用

文档: https://github.com/pagekit/vue-resource

请求方式: 

- `get(url, [config])`
- `head(url, [config])`
- `delete(url, [config])`
- `jsonp(url, [config])`
- `post(url, [body], [config])`
- `put(url, [body], [config])`
- `patch(url, [body], [config])`

基本使用: 

```javascript
{
  // GET /someUrl
  this.$http.get('/someUrl').then(response => {

    // get body data
    this.someData = response.body;

  }, response => {
    // error callback
  });
}
```

### 2.3 测试用接口(豆瓣&天气预报)

开放的接口测试: http://www.k780.com/api

天气预报(国家气象局 ): http://wthrcdn.etouch.cn/weather_mini?city="深圳"

1、豆瓣热映

接口：https://api.douban.com/v2/movie/in_theaters

参数：

start : 数据的开始项

count：单页条数

city：城市

如：获取“北京”热映电影“第二页”每页“25条”数据：

https://api.douban.com/v2/movie/in_theaters?city=北京&start=25&count=25

2、电影top250

接口：http://api.douban.com/v2/movie/top250

参数：

start : 数据的开始项

count：单页条数

如：获取电影Top250 第二页 25条数据：

http://api.douban.com/v2/movie/top250?start=25&count=25

3、电影条目检索

接口：http://api.douban.com/v2/movie/search

访问参数：

start : 数据的开始项

count：单页条数

q：要搜索的电影关键字

tag：要搜索的电影的标签

如：第二页每页25条

搜索电影《战狼》：

https://api.douban.com/v2/movie/search?q=战狼&start=25&count=25

搜索喜剧类型的电影：

https://api.douban.com/v2/movie/search?tag=喜剧&start=25&count=25

4、条目详情


接口：http://api.douban.com/v2/movie/subject

参数：

电影id

如：电影《神秘巨星》的电影id为：26942674，搜索此电影的详细信息

http://api.douban.com/v2/movie/subject/26942674

### 2.4 CDN

常用的js库 放到了他的服务器 免费提供给我们使用

通常用于性能优化处理,降低服务器压力

地址: `https://cdn.jsdelivr.net/npm/vue-resource@1.5.1 `

#### 2.5 带Cookie的跨域Ajax请求

- 客户端

```
$.ajax({
        url : 'http://remote.domain.com/corsrequest',
        data : data,
        dataType: 'json',
        type : 'POST',
        xhrFields: {
            withCredentials: true
        },
        crossDomain: true,
        contentType: "application/json",
        ...
```

通过设置 `withCredentials: true` ，发送Ajax时，Request header中便会带上 Cookie 信息。

- 服务器端

相应的，对于客户端的参数，服务器端也需要进行设置：

* ```
  /**
  * Spring Controller中的方法：
  */
      @RequestMapping(value = "/corsrequest")
      @ResponseBody
      public Map<String, Object> getUserBaseInfo(HttpServletResponse response) {
          if(request.getHeader("Origin").contains("woego.cn")) {
              response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));
          }
          response.setHeader("Access-Control-Allow-Credentials", "true");
          ...
  }
  
  ```

  对应客户端的 xhrFields.withCredentials: true 参数，服务器端通过在响应 header 中设置 Access-Control-Allow-Credentials = true 来运行客户端携带证书式访问。通过对 Credentials 参数的设置，就可以保持跨域 Ajax 时的 Cookie。这里需要注意的是：

注意:

服务器端 Access-Control-Allow-Credentials = true时，参数Access-Control-Allow-Origin 的值不能为 '*' 



## 3. Vue组件

### 3.1 作用

**组件是可复用的 Vue 实例** 

- 组件的出现,就是为了拆分Vue实例的代码量,能够让我们以不同的组件,来划分不同的功能模块,我们需要声明功能,就调用对应的组件即可

### 3.2 组件化和模块化的区别

- 组件化(前端): 从UI界面角度进行划分的 ,方便UI组件的重用
- 模块化: 从代码逻辑角度进行划分;方便代码分层开发,保证每个功能模块的职能单一

### 3.3 基本使用(组件切换)

```javascript
<body>
    <div id="app">
        <a href="#" @click.parent="flag=true">登录</a>
        <a href="#" @click.parent="flag=false">注册</a>
        <login-component v-if="flag"></login-component>
        <register-component v-else="flag"></register-component>
    </div>

    <script>
        // 组件
        Vue.component('login-component', {
            template: '<h3>登录组件</h3>'
        })
        Vue.component('register-component', {
            template: '<h3>注册组件</h3>'
        })

        let vm = new Vue({
            el: "#app",
            data: {
                flag: false
            }
        })
    </script>
</body>
```

### 3.4 父组件与子组件之间传递数据的方式

1. 父组件=>子组件 通过**v-bind**绑定传递数据 **props**属性
2. 子组件=>父组件 通过**v-on**是事件绑定机制 **$emit**触发方发

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- 导入vue -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        <!-- 组件传值的属性不能使用大写 -->
        <son :parentmsg="msg" @func='getson'></son>
        <input type="text" v-model="msg">
        <br>
        <input type="text" v-model="sonmsg">
    </div>
    <!-- 组件模板 -->
    <template id="son">
        <div>
            <h1>我是子组件 ---- {{parentmsg}}</h1>
            <input type="button" value="我是子组件的按钮,点击我传递数据给父组件" @click='sendMsg'>
        </div>
    </template>
    <script>
        // 子组件
        const son = {
            template: '#son',
            // 父组件=>子组件 通过v-bind绑定传递数据
            props: ['parentmsg'],
            data(){
                return{
                    son: '我是儿子,给你传个话'
                }
            },
            methods:{
                sendMsg(){
                    // 子组件=>父组件  通过v-on是事件绑定机制 $emit触发方法
                    this.$emit('func',this.son)
                }
            }
        }

        let vm = new Vue({
            el: "#app",
            data: {
               msg: "我是父组件的值",
               sonmsg: null
            },
            // 子组件
            components: {
                son
            },
            methods: {
                // 获取子组件的值
                getson(data){
                    this.sonmsg = data;
                }
            }
        })
    </script>
</body>
</html>
```

### 3.5 组件里的data为什么要返回一个对象

- 原因:
  - 如果你直接返回对象, 如 return {count: 1};  因为当前的对象是引用类型,如果使用多个同样的组件,数据会共享
  - 如果你直接返回对象的对象 如 return { {count: 1} }, 那么当前会开辟一个单独的内存空间,数据就不会共享了



## 4. vue-router 路由

官网: https://router.vuejs.org/zh/

**vue**-router**是Vue**.js官方的**路由**插件，它和**vue**.js**是**深度集成的，适合用于构建单页面应用。 

### 4.1 什么是路由

1. 后端路由: 对于普通的网站,所有超链接都是URL地址,所有的URL地址都对应服务器上对于的资源
2. 前端路由: 对于单页面应用程序来说.主要通过URL中的hash(#号)来实现不同页面之间的切换,同时hash有一个特点: HTTP请求中不会包含hash相关的内容;所以单页面程序中的页面跳转主要用hash实现;
3. 在单页面应用程序中,这种通过hash改变来切换页面的方式,称作前端路由(区别于后端路由)
4. 前端路由两种实现方式
   - HTML5 的 `History `模式，它使 url 看起来像普通网站那样，以“／”分剖，没有＃，但页面并没有跳转，不过使用这种模式需要服务端支持
   - 主要通过URL中的hash(#号)来实现
5. URL中的hash  地址: https://www.cnblogs.com/joyho/articles/4430148.html

### 4.2 单页面应用(SPA)

Single Page Application

- 只有一张Web页面的应用，是一种从Web服务器加载的富客户端，单页面跳转仅刷新局部资源 ，公共资源(js、css等)仅需加载一次，常用于PC端官网、购物等网站 
- Vue中一般是结合 Vue-router 来实现
- 其实就是在前后端分离的基础上，加一层前端路由。  

### 4.3 在Vue中使用vue-router

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

### 4.4 设置路由重定向

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
    // router-link添加类名
    linkActiveClass: 'myactive'
})
```

### 4.5 路由传参（路由规则params）

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
    // router-link添加类名
    linkActiveClass: 'myactive'
})
```

### 4.6 嵌套路由

官网地址: https://router.vuejs.org/zh/guide/essentials/nested-routes.html

设置 `children`  配置,注意, `children` 下的path前面不要带 `/`  否则**以 / 开头的嵌套路径会被当作根路径开始请求,这样不方面用于理解url地址**  

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



### 4.7 导航守卫-----vue-router 导航钩子功能

 vue-router 提供了导航钩子

路由(页面)跳转时触发,方便实现一些全局的功能，而且便于维护    

导航钩子有 3 个参数：

- to 即将要进入的目标的路由对象 。
- from 当前导航即将要离开的路由对象 。
- next 调用该方法后，才能进入下一个钩子,next的参数设置为 false 时，可以取消导航    

beforeEach 和 afterEach ， 它们会在路由即将改变前和改变后触发.

### 4.8 路由元信息

路由中 设置一个 meta属性
	这个属性名不能乱写
meta属性设置的值
	根据自己的需求 可以进行修改

使用场景: 

- 根据不同页面设置页面标题 (路由元信息)

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
  // 页面标题设置为对应的mate属性
  window.document.title = to.meta.title;
  next();
})
```

- **后置钩子函数功能**(返回顶部):一个页面较长，滚动到某个位置，再跳转到另一个页面，滚动条默认是在上一个页面停留的位置，而好的体验肯定是能**返回顶端**

```javascript
// 放回顶部  后置钩子函数
router.afterEach((to, from, next) => {
    // 跳转完毕之后执行
  window.scrollTo(0, 0)
})    				
```

- 校验页面是否登录

```javascript
// 放回顶部
router.beforeEach((to, from, next) => {
    if(window.localStorage.getItem('token')){
        next();
    }else{
        next('/login')
    }
})    
```

- 从一个页面过渡到另一个页面时，可以出现一个全局的 Loading 动画，等 到新页面加载完后再结束动画。    

### 4.9 编程式的导航

官网: https://router.vuejs.org/zh/guide/essentials/navigation.html

| 声明式                    | 编程式             |
| ------------------------- | ------------------ |
| `<router-link :to="...">` | `router.push(...)` |

**在 Vue 实例内部，你可以通过 $router 访问路由实例。因此你可以调用 this.$router.push**

返回首页:  

` this.$router.push('/index')`



### 4.10 router的this.$router其他方法

$router 还有其他一些方法:

- replace
  - 类似于<router-link>的 replace 功能 ，它不会向 history 添加新记录,而是替换掉当前的history 记 录，如 this.$router.replace （'/user/123'）

-  go

  - 类似于 window.history.go（），在 history 记录中向前或者后退多少步，参数是整数，例如:

    ```
    // 后退 1 页
    this.$router.go(-1);
    
    // 前进 2 页
    this.$router.go(2);
    ```



## 5. webpack

官网: https://webpack.docschina.org/

> 前端工程化工具 webpack
> webpack主要使用场景是单页面富应用（ SPA )    

### 5.1 前端自动化（半自动化〉工程主要解决以下问题：

- JavaScript 、 css 代码的合井和压缩 。
- css 预处理 ： Less 、 Sass 、 Stylus 的编译。
- 生成雪碧图（ CSS Sprite ）。
- ES 6 转 ES 5.
- 模块化。

归根到底， webpack 就是一个.js 配置文件，你的架构好或差都体现在这个配置里，随着需求 的不断出现，工程配置也是逐渐完善的    

### 5.2 webpack的核心

​	默认只能打包 JavaScript
	要解析其他文件 需要安装 对应的 loader配置规则即可

### 5.3 安装loader(加载器)解析不同资源

npm install css-loader --save-dev    

在 webpack 的世界里，每个文件都是一个模块，比如css 、 .js 、 .h恤l、 .jpg、 .less 等 。 对于不 同的模块，需要用不同的加载器（ Loaders ）来处理，而加载器就是 webpack 最重要的功能 。    

### 5.4 单文件组件与 vue-loader 的基本结构

vue-loader 在编译 .vue 文件时， 会对＜template＞、＜scrip t> 、＜style＞分别处理，所以在 vue-loader 选项里多了 一项 options 来进一步对不同语言进行配置。    

```html
<template> 

</template> 

<script> 

</script> 

<style>

</style>
```

### 5.5 vue.cli脚手架

官网: https://cli.vuejs.org/zh/

1. 全局安装脚手架工具 
2. 初始化项目 
3. 切换到项目根目录中
 4. 安装依赖包 
 5. npm run dev运行项目

## 6. Vue.filter过滤器

过滤器其实就管道符,一般用来处理数据格式,比如时间日期等

### 6.1 全局过滤器基本使用

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./lib/vue.js"></script>
</head>

<body>

    <div id="app">
        {{ msg | msgFormat("善良") }}
    </div>


    <script>
        // 定义一个 Vue全局过滤器 名字叫msgFormat        function(参数1: 过滤的对象, 参数2: 替换的内容)
        Vue.filter('msgFormat', function (msg, auguments) {
            // 定义正则    
            return msg.replace(/单纯/g, auguments)
        })

        let app = new Vue({
            el: "#app",
            data: {
                msg: '曾经,我也是一个单纯的少年,单纯的我,傻傻的文,谁是世界上最单纯的男人'
            }
        })
    </script>
</body>

</html>
```

### 6.2 日期格式化(轮子)

```javascript
// 自定义过滤器 进行日期格式改为 yyyy-mm-dd
Vue.filter('dateFormat', function (dateStr, pattern) {
    // 根据传递过来的参数 放回特定的时间戳
    let dt = new Date(dateStr);
    // 年月日
    let y = dt.getFullYear();
    let m = dt.getMonth() + 1;
    let d = dt.getDate().toString().padStart(2,0);
    // 自定义时间戳 转小写   如果没参数 默认 yyyy-mm-dd-hh-mm-ss 
    if (pattern && pattern.toLowerCase == "yyyy-mm-dd") {
        return `${y}-${m}-${d}`
    } else {
        // hh = hh < 10 ? '0' + hh : hh;
        // 时分秒      padStart(maxLength, string))
        let hh = dt.getHours().toString().padStart(2,0);
        let mm = dt.getMinutes().toString().padStart(2,0);
        let ss = dt.getSeconds().toString().padStart(2,0);
        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
    }
})
```

### 6.3 使用Moment插件实现自定义日期格式

官网: http://momentjs.cn/

```javascript
// 导入moment.js时间插件
import moment from 'moment';
// 全局过滤器
Vue.filter('formatTime',(data,format='YYYY-MM-DD')=>{
    // 不传参数默认是(YYYY-MM-DD)
  return moment(data).format(format)
})

// 管道符  (自定义传参)
<span>{{item.add_time | formatTime('YYYY-MM-DD hh:mm:ss')}}</span>
```

## 7. Vuex

> **vuex简单来说就是处理组件与组件的数据共享**

官网: https://vuex.vuejs.org/zh/

   Vuex就是在vue中保存数据的一种机制(方法),

### 7,1 作用

概念: vuex是Vue配套的公共数据管理工具,它可以把一些共享的数据,保存到vuex中,方便整个程序的任何组件直接获取或修改我们的公共数据

作用: **统一管理路由组件,让vue中的组件进行通讯,在全局最高环境下保存数据,使所有子组件都可以访问到**

### 7.2 基本使用

```javascript
// 导入Vuex模块
import Vuex from 'vuex'
// Vue实例使用Vuex模块
Vue.use(Vuex)
// 实例化Vuex的store仓库
const store = new Vuex.Store({
  // 数据设置到state中 里面的数据只能读取,不能手动修改,只能通过mutations提交
  state: {    //  this.$store.state  使用方法
    count: 0
  },
  // 修改数据的方式
  mutations: {  // 在方法中使用 this.$store.commit("increment");
    increment (state) {
      state.count++
    }
  },
   // getter 类似于Vue的计算属性(当属性用)  只负责对外提供数据,不负责修改数据
  getters: {   //  模板中调用改方法{{$store.getters.cartDataCount}}  
    cartDataCount(state){
      state.totalCount ++
      return state.totalCount;
    }
  }
})

// 实例化vue
new Vue({
  // 渲染主组件
  render: h => h(App),
  // 挂载路由
  router,
  // 挂载 Vuex
  store
}).$mount('#app') // 挂载到index页面#app的div上

<!-- 模板中取值vuex -->
<input type="button" value="我是App.vue的按钮" @click="add"> 

// 点击事件调用方法
methods: {
    add() {
        // 获取$store中的数据
        console.log(this.$store.state)  
        // 调用vuex的increment的方法  commit 触发状态变更
        this.$store.commit("increment");
    }
},
```

![1539498632254](E:\CodeSettle\notes\Vue\assets\1539498632254.png)

### 7.3 Mutation 需遵守 Vue 的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`

### 7.4 Vuex提交载荷遇到的坑(Vue.set)

写购物车模块时,发现页面的数据没有同步,当时找了很久,后来通过查阅文档发现没有使用Vue.set

### 7.5 Vuex删除对应的数据坑点(Vue.delete)

如果直接删除对象的属性 delete =>  数据改变页面不同步

要用Vue.delete

### 7.6 如何在vue中自己传递参数,并获取元素的原始参数($event)

如果我们要动态的往数据里添加字段数据	

**$event 目的是获取原始的参数**

![1539498603126](E:\CodeSettle\notes\Vue\assets\1539498603126.png)

vue动态添加属性必须要设置Vue.set属性

否则vue就不会帮你设置get和set属性

![1539498540371](E:\CodeSettle\notes\Vue\assets\1539498540371.png)

### 7.7 mutations 、 actions的区别

涉及改变数据的，就使用 mutations，存在业务逻辑的，就用 actions。至于将业务 逻辑放在 action 里还是 Vue 组件里完成，就需要根据实际场景拿捏了    

### 7.8 modules

作用: 将 store 分割到不同模块    

每个 module 拥有自己的 state、 getters 、 mutations 、 actions，而且可以 多层嵌套。    



## 8. Vue性能优化方案

### 8.1 keep-alive(缓存组件)

文档: https://cn.vuejs.org/v2/api/#keep-alive

在vue单文件开发中,路由组件切换时,某些页面有大量的ajax请求,如果重新打开 组件中的所有数据都会重新加载,但是有些数据获取之后不会频繁更改,为了性能优化,降低服务器压力,可以通过`keep-alive`正则表达式`include` ,值缓存某一些页面,比如

- `include` - 字符串或正则表达式。只有名称匹配的组件会被缓存。
- `exclude` - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。

基本使用

```html
// App.vue
<!-- 组件出口 -->
<!-- include: 字符串或正则表达式。只有名称匹配的组件会被缓存。 -->
<keep-alive include="index">
	<router-view></router-view>
</keep-alive>
```



### 8.2 按需导入ui框架

文档: http://element-cn.eleme.io/#/zh-CN/component/quickstart

#### 8.2.1 element-ui  (iview) 步骤一样

步骤

1. 装包: `npm install babel-plugin-component -D  `  

2. 配置根目录`babel.config.js`

   ```javascript
   {
     "presets": [["es2015", { "modules": false }]],
     "plugins": [
       [
         "component",
         {
           "libraryName": "element-ui",
           "styleLibraryName": "theme-chalk"
         }
       ]
     ]
   }
   ```

   ![1540051070411](E:\CodeSettle\notes\Vue\assets\1540051070411.png)

3. `mian,js`

   ```javascript
   import Vue from 'vue';
   import { Button, Select } from 'element-ui'; // {}
   import App from './App.vue';
   
   // Vue.component(Button.name, Button);
   // Vue.component(Select.name, Select);
   
   Vue.use(Button)
   Vue.use(Select)
   
   
   new Vue({
     el: '#app',
     render: h => h(App)
   });
   ```

   ![1540051105318](E:\CodeSettle\notes\Vue\assets\1540051105318.png)



### 8.3 路由懒加载(按需加载组件)

文档: https://router.vuejs.org/zh/guide/advanced/lazy-loading.html

**当打包构建应用时**，Javascript 包会变得非常大，影响页面加载。如果我们**能把不同路由对应的组件分割成不同的代码块**，然后当**路由被访问的时候才加载对应组件**，这样就更加高效了。 

定义一个能够被 Webpack 自动代码分割的异步组件:

`const Foo = () => import('./Foo.vue')`

基本使用: 

```javascript
// 导入首页组件
const index = () => import('../components/index.vue')
// 导入商品详情组件
const detail = () => import('../components/detail.vue')
// 导入(自己写)购物车模块
const cart = () => import('../components/cart.vue')
// 导入购物车模块
const shopcart = () => import('../components/shopcart.vue')
// 导入登录页模块
const login = () => import('../components/login.vue')
// 导入订单模块
const checkOrder = () => import('../components/checkOrder.vue')
```



### 8.4 使用CDN优化第三方js库

Vue-cli的webpack 相关配置

文档: https://cli.vuejs.org/zh/guide/webpack.html

搜索关键词: 1. cdn vue cli     2. vuecli webpack config

1. 资源引入 pubilc里的index

   ```html
   <div id="app"></div>
   <!-- 按需加载jQuery -->
   <script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
   <!-- 按需加载axios -->
   <script src="https://cdn.bootcss.com/axios/0.18.0/axios.min.js"></script>
   ```

   ![1540053712981](E:\CodeSettle\notes\Vue\assets\1540053712981.png)

2. 根目录下新建文件`vue.config.js` 

   ```javascript
   // 暴露出去
   module.exports = {
       // 在vue中修改webpack的配置  配置之后,webpack不会去node_modules打包对应的库,忽略打包编译
       configureWebpack: {
           // plugins: [
           //     // new MyAwesomeWebpackPlugin()
           // ]
           // 把原本需要些在webpack.config.js中的配置代码,写在这里,会自动合并
           externals:{
               // 使用cdn 需要在public里的index引入相关资源
               'jquery': '$',
               'axios':'axios'
           }        
       }
   }
   ```

   ![1540054438955](E:\CodeSettle\notes\Vue\assets\1540054438955.png)



### 8.5 压缩格式

1. 使用 Gzipped 进行压缩 服务器配置的
2. 后端设置

### 8.6 修改图片格式

1. **webp:** 谷歌开发的一种旨在加快图片加载速度的图片格式 
2. jpg:  全名是JPEG，是图片的一种格式 使用24位真彩色 
3. png: 一种无损压缩的位图片形格式  使用32位真彩色 

### 8.7 静态资源服务器

把静态资源丢到那个服务器 降低 网站服务器的压力

### 8.8 web服务器购买带宽
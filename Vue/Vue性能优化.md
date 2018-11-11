## Vue性能优化方案

## keep-alive(缓存组件)

传送门: https://cn.vuejs.org/v2/api/#keep-alive

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



### 按需导入ui框架

文档: http://element-cn.eleme.io/#/zh-CN/component/quickstart

#### element-ui

步骤

1.  装包: `npm install babel-plugin-component -D  `  

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
   import { Button, Select } from 'element-ui';
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



### 路由懒加载(按需加载组件)

文档: https://router.vuejs.org/zh/guide/advanced/lazy-loading.html

当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。 

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



### 使用CDN优化第三方js库

Vue-cli的webpack 相关配置

文档: https://cli.vuejs.org/zh/guide/webpack.html

搜索关键词: 1. cdn vue cli     2. vuecli webpack config

1. 资源引入

   ```
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



### 压缩格式

1. 使用 Gzipped 进行压缩 服务器配置的
2. 后端设置

### 修改图片格式

1. **webp:** 谷歌开发的一种旨在加快图片加载速度的图片格式 
2. jpg:  全名是JPEG，是图片的一种格式 使用24位真彩色 
3. png: 一种无损压缩的位图片形格式  使用32位真彩色 

### 静态资源服务器

把静态资源丢到那个服务器 降低 网站服务器的压力

### web服务器购买带宽
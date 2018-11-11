## keep-alive

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


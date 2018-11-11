#   Vue请求接口方式

[TOC]



### axios

https://www.kancloud.cn/yunye/axios/234845

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
  - axios.defaults.baseURL = 'http://111.230.232.110:8899'; 



## vue-resource

>  vue-resource 是一个 vue的插件 往vue示例中 增加一个 $http属性
>
>​    所有的网络请求 都是通过$http来使用
>
>​    先导入vue 再倒入 vue-resource
>
>​    早期 vue中用来调用接口的 js库
>
>​    vue的作者直接取消 对于这个库的推荐了
>
>​        只能结合 vue使用

文档: https://github.com/pagekit/vue-resource

请求方式: 

- `get(url, [config])`
- `head(url, [config])`
- `delete(url, [config])`
- `jsonp(url, [config])`
- `post(url, [body], [config])`
- `put(url, [body], [config])`
- `patch(url, [body], [config])`

> 

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





### 测试用接口

开放的接口测试: http://www.k780.com/api

天气预报(国家气象局 ): http://wthrcdn.etouch.cn/weather_mini?city="深圳"



### CDN

常用的js库 放到了他的服务器 免费提供给我们使用

地址: https://cdn.jsdelivr.net/npm/vue-resource@1.5.1 
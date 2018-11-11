# Express框架

## 01. Express简介

> 	Express是1个基于Node.js的Web开发框架. 其对原生的Node.js的Web开发进行了封装, 提供了更加方便开发Web程序的方式。

`express官网:`  http://expressjs.com

`express中文:`  http://www.expressjs.com.cn

`Express`的特点

* 实现了路由功能
* 中间件功能
* 对`req`和`res`进行了丰富的扩展
* 可以集成其他模板引擎

## 02 使用Express

### 2.1 下载Express

```
npm install express --save
```

### 2.2 Hello,Express

```javascript
const express = require("express");

const app = express();

//通过中间件 监听指定路由的请求.
app.get("/index",(req,res) => {
    //get请求 并且请求url为 /index
    //这个回调就会被执行.
})

app.listen(8080,()=>console.log("服务启动成功"));
```

### 2.3 res.send()

Express为res对象扩展了1个send方法 发送并结束响应， 相对于res.end()

* end只支持Buffer和string,  而send方法支持的参数类型要多，还可以是数组、对象......

* 可以快速设置响应码，和响应消息

  ```
  res.status(404).send("Not Found");
  ```

* send方法底层帮助我们设定了响应头指定编码为utf-8

## 03. 注册路由

### 3.1 get

 app.get() 方法

* 必须是以get方式请求
* url 完全匹配

```javascript
app.get(url,(req, res) => {
    
})
```



### 3.2 post

 app.post()方法.

* 必须是以post方式请求
* url 完全匹配.

```javascript
app.post(url, (req, res) => {
    
})
```



### 3.3 use

 app.use()方法

* 对get、post没有要求 不区分请求方法
* 只要以url,开头就会被匹配.
  * app.use("/index", (req,res)=>{})
    * "/index" 会被匹配
    * "/index/aaa" 会被匹配
    * "/index/aaa/cc/csc/11.html" 会被匹配
    * ”indexaaa“ 不会被匹配.

```javascript
app.use(url, (req, res) => {
    
})
```

### 3.4 all

 app.all() 方法

* 不限定请求方法
* url 完全匹配.

```javascript
app.all(url, (req, res) => {
    
})
```



### 3.5 正则表达式匹配

 所有注册路由的方法，url可以使用正则表达式。

 ```javascript
app.get(/^\/index(\/.+)*$/, (req, res) => { 
	//必须是以get请求
    //url以 /index 开头 
});
 ```



## 04. 静态资源托管

> express提供了1个static方法, 这个方法接受静态资源的目录，并返回1个中间件，这个中间件用来处理静态资源

```javascript
//以 /  开头的请求 都会被当做是在请求静态资源，会去读取public文件夹下的文件返回.
app.use("/", express.static(path.join("public")));
//如果请求以 /public 开始，则认为是静态资源
app.use("/public", express.static(path.join("public")))

```

## 05. 对req的扩展

建议查看文档

### 5.1 req.params

	  可以取到路由参数.

```javascript
http://xuanwen.wang/news/12341

app.get("/news/:id", (req, res) => {
    req.params.id
})

```



### 5.2 req.query

	取的querystring参数.

```javascript
http://xuanwen.wang/news?id=12312
app.get("/news", (req, res) => {
    req.query.id
});
```



 



## 06. 对res的扩展

建议查看文档

* res.json()
* res.redirect()
* res.send()
* res.sendFile()
* res.status().end()





## 07. 用express改造hacker-news

 ### 7.1 模块构成

* 主模块 - 负责启动模块
* 路由模块
* 业务模块
* 配置模块
* 数据库模块



### 7.2 配置模块



### 7.3 路由模块

在路由模块中，把路由规则挂载到router对象上，把router对象，暴露出去.

使用`app.use(router)` 就可以挂载路由.

router.js

```javascript

```

### 7.4 业务模块



`sendFile()`

## 8. 模板引擎

`res.render()`

模板引擎，渲染。

虽然有这个方法，但是默认无法使用。必须要为express设置模板引擎才可以使用.                                                                                                     

许多模板都对express做了兼容, 比如Pug、ejs、Mustache等.



### 8.1 ejs模板引擎

#### 8.1.1 安装ejs

```
npm install ejs --save
```




















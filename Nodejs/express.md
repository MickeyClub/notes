一、Express简介
   基于Nodejs的Web开发框架.


   **对Nodejs做了封装，让我们开发Web程序更加方便和快速.**
   > 对res和req做了丰富的扩展.
   >  > 实现了中间件.
   >  > > 实现了路由功能.
   >  > > 可以集成其它模板引擎.

   网站
   www.expressjs.com   	官网
   www.expressjs.com.cn	中文官网



二、 Express使用

1. 安装express
      npm install express

   2. 使用express

      ```javascript
      //加载express模块
      const express = require("express");
      //创建1个app对象，类似与http的server对象.
      const app = express();
      
      //路由中间件
      app.get("/index",(req,res)=>{ 
           //get方式请求 url是 /index
      })
      //监听
      app.listen(80);
      ```

3. res.send 与 res.end

      > 基于Nodejs在end的基础上,自动设置Content-Type和编码.
      > end只支持字符串和Buffer  send所有类型几乎都支持。
      > 快速设置响应码和响应消息.
      > res.status(404).send("Not Found");



二、注册路由

      1. app.get()
      以get方式请求 url完全匹配.
      2. app.post()
      以post方式请求 url完全匹配.
      3. app.use()
      不限定请求方式. 以url开头就可以匹配.
      4. app.all()
      不限定请求方式，url完全匹配.
      5. 正则表达式匹配.
      /^\/index(\/.+)*$/
      app.get(/^\/index(\/.+)*$/, (req, res) => { })

三、 静态资源托管.

以/public开头的url请求,会直接从public目录中读取响应.
`app.use("/public", express.static(path.join(__dirname,"public")))`



四、 对req的部分扩展(查看文档)http://www.expressjs.com.cn/4x/api.html#req
    

    1. req.query  //一个对象,获取get提交过来的数据,如item?id=190&xx=yy 类似于querystring
    
    2. req.params	// 获取路由参数


五、 对res的部分扩展(查看文档)http://www.expressjs.com.cn/4x/api.html#res

    1. res.send()  
    //express为res新增了send方法.
    //end方法: 字符串 Buffer
    //send: 参数几乎是任何类型的. 转换为json字符串.
    //send: 自动的在底层帮助我们设置了Content-Type 和 编码.
    
    2. res.status().send()  //快速设置响应吗
    
    3. res.sendFile()  //给1个文件的路径，会将这个文件读取出来直接响应给浏览器.
    
    4. res.json()   //转换为JSON字符串的参数。
    
    5. res.redirect()	//重定向
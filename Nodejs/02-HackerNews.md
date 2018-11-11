# HackerNews项目

## 00. request对象和response对象

`req.headers ` 返回1个对象，这个对象中包含了所有的请求报文头.

`req.rawHeaders` 返回1个数组，保存的都是请求报文头的字符串

`req.httpVersion` 使用的http版本

`req.method ` 浏览器的请求方式

`req.url`  浏览器的请求url

`req.protocol`  是http还是https请求



`res.write()` 向服务器返回的数据，将会被放到响应体中，直到调用`res.end()` 才会响应

`res.end()`

`res.setHeader() `

`res.statusCode `   ` res.statusMessage ` 设置 http 响应状态码 

`res.writeHead()  ` 设置响应码和响应消息，并设置响应头

```javascript
res.writeHead(200,'ok',{
    'Content-Type': 'text/html;charset=utf-8'
})
```





## 01. 项目演示





## 02. 搭建静态资源服务器

### 2.1 设置状态码

`res.statusCode = 404` 设置响应状态码

`res.statusMessage = "Not Found" 设置状态码对应的消息`

### 2.2 设置响应头

`res.writeHead();`方法用来设置响应头，其它设置响应头的方法底层最终都会调用这个方法.

`参数1:` 响应状态码

`参数2:` 响应码对应的消息

`参数3:` 可省略 对象, 以键值对的方式设置响应头

```javascript
res.writeHead(200,"ok",{
    "Content-Type": "text/html;charset=utf-8"
});
```



### 2.3 为res集成render



## 03. 渲染新闻列表

### 3.1 underscore

服务端渲染





## 04. 新闻详情页

### 4.1  服务器得到querystring类型参数

使用内置的`url`模块，可以将以get方式提交到服务器的数据解析成对象，方便我们使用。

```javascript
//假设 req.url = "/item?id=1&a=2&c=3"
const params = url.parse(req.url,true);
params.query是1个对象  {id: 1, a: 2, c: 3}
```

### 4.2  同步执行也有用

```javascript

if(req.url.startsWith("/item?")){
        const params = url.parse(req.url,true);
        const id = params.query.id;
        const data = fs.readFileSync(path.join(__dirname,"data","news.json"),"utf-8");
        const news = JSON.parse(data);
        let model = null;
        for(let index in news) {
            if(news[index].id == id) {
                model = news[index];
                break;
            }
        }
        fs.readFile(path.join(__dirname,"views","details.html"), "utf-8", (err,data)=>{
            if(err) throw err;
            res.setHeader("Content-Type","text/html;charset=utf-8");
            const html = _.template(data)({news:model});
            res.end(html);
        });
}
```

当然也可以用异步



## 05. 新增新闻

### 5.1 服务器得到post过来的数据

当浏览器提交给服务器的数据是以post方式提交的，服务器应该以如下方式接收.

浏览器是以流的方式向服务器发送数据的。

```javascript
let array = [];
req.on("data",chunk => array.push(chunk));
req.on("end",()=>{
    const body = Buffer.concat(array).toString("utf-8");
    console.log(body);
    res.end();
});
```

使用`querystring`模块将post数据解析成对象.



## 06. 封装读写json





## 07. 模块加载

### 7.1 node中require加载模块是同步的

### 7.2 模块的分类

* 内置模块 核心模块 原生模块 由Nodejs提供  
  * http fs path url ......
* 文件模块
  * .js
  * .json
  * .node (C/C++编写的模块)
* 自定义模块(第三方模块)
  * mime
  * ......

### 7.2 require加载模块的过程

* require的是一个路径.  require的时候 如果是路径必须以 ./开头
  *  `require("./database.js");` 这种情况，给的是1个文件路径，并且带了后缀名，那么就会根据这个相对路径拼出绝对路径，去加载这个文件，如果有就加载，如果没有就加载失败	。
  * `require("./database");` 是1个相对路径，但是没有写后缀名. 加载顺序:
    * 先找database.js
    * 再找database.json
    * 再找database.node
    * 再找database这个文件夹 认为它是1个第三方模块
      * 找 package.json
      * 找main字段，入口文件.
      * 查找入口文件，加载。
      * 如果入口文件不存在，找index.js -> index.json -> index.node
* require的是一个模块名称
  * `require("mime")` 
    * 先查找是否是内置模块.
    * 当前js文件目录下，查找`node_modules`目录
      * 如果有。
        * 查找是否有这个模块。
      * 如果没有
        * 找父目录是否有这个文件夹。
        * 直到找到根目录 如果没有 就报错

### 7.3 加载细节

* 一旦被加载过1次，那么第2次，直接从缓存中读取.
* 每次加载模块的时候，优先从缓存中加载 没有再走流程.
* 核心模块一开始被编译成了二进制文件，所以加载速度极快.
* 试图加载1个和 核心模块 同名的自定义模块是不会成功的
  * 自定义模块的名字不能与核心模块重名.
  * 使用路径加载.
* 核心模块只能通过模块名称来加载 使用路径的方式加载是不行的 require("./http") 是错误的
* require加载模块时使用 ./ 相对路径 永远指的是相对于当前js文件.
* 建议加载文件模块的时候，写上后缀名.



## 07. 模块化

### 7.1 CommonJS规范

`CommonJS`规范， http://www.commonjs.org

nodejs的模块遵循了CommonJS规范



### 7.2 自定义文件模块

当使用`require`加载1个文件模块，默认情况下，返回的是一个空对象`{ }`

 **a.js**

```javascript
const add = (a, b) => a + b;
const result = add(3,4);
console.log(result);
```

 **b.js**

```javascript
const a = require("./a.js");
//首先，a.js会被执行.
//其次返回的a是1个空对象 { }
```

如果期望被加载的模块返回一些数据，可以在模块中使用 `module.exports`来返回.

* 实际上，当我们加载1个文件模块后，先执行这个文件模块，然后将这个模块module.exorts的值返回
* module.exorts的值默认是1个空对象.
* 所以，将需要返回的值赋值给 module.exorts 就可以. 可以设置为任意的数据，对象 函数 等等

```javascript
const add = (a, b) => a + b;
const result = add(3,4);
console.log(result);
module.exports = "jack";
//加载后返回的就是字符串"jack"
```

```javascript
const add = (a, b) => a + b;
const result = add(3,4);
console.log(result);
module.exports.name = "jack";
module.exports.age = 18;
module.exports.sayHi = () => console.log("hi..");
```

### 7.3 module.exports与exports

首先你需要明白的是，模块最终返回的是 `module.exports`， 它的默认值是1个空对象.

一开始的时候，exports 指向的是 `module.exports`指向的对象.

如果为`module.exports`重新赋值1个对象，那么这个时候exports 指的就是不同的对象了。






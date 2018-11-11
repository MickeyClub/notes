### EJS模板引擎

官网:  https://ejs.bootcss.com/

1. express里的res对象有1个render方法. 但是这个render方法无法使用.

​       必须要配置 express 的模板引擎才可以使用.

    2. 下载ejs
       npm install ejs
    
    3. 单独使用.
       <%%>
       ejs.render(str,data);
       ejs.renderFile();
    
    4. 集成到express使用.
       //设置模板引擎的目录
       app.set("views",path.join(__dirname,"views"));
       //设置express使用的模板引擎
       app.set("view engine","ejs")
    
       注意的是，ejs模板文件的后缀名必须为.ejs
    
       res.render("view",data);
    
    5. 使用后缀名.html
    
       //自定义模板引擎. html	
       app.engine("html",ejs.renderFile)
       app.set("views")
       app.set("view engine", "html")
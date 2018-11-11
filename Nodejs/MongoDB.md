##  MongoDB

[TOC]

### MongoDB基本操作

1. 简介.
    文档性数据库.       非关系型数据库

  数据库里面是1个1个的集合。集合里存储的是1个1个的对象.
  没有表,只有集合，并且集合与集合之间没有关系.

2. 安装.
     打开安装包,默认安装下一步(注意可以取消图形化安装) 

     配置MongoDB的环境变量=>mongo执行文件

     C:\Program Files\MongoDB\Server\4.0\data

3. 连接MongoDB
     mongo --host 127.0.0.1 --port 27017
       如果要连接本机: mongo

4. 新增 删除 查询 修改

   shell 兼容 ECMAScript.

5. 查询数据库
     show dbs;

6. 进入某个数据库
     use dbName;

7. 确定当前处于哪一个数据库下.
     db

8. 查看数据库下有哪些集合.
     show collections;

### 常用shell命令

1. 创建数据库

​       use dbName;      
       如果你没有在下面创建集合，退出后这个数据库就没有了。
       你只要在下面创建集合，然后才这个数据库才会保存.

2. 创建集合. 

   db.students

3. 存入文档.
   db.students.insert({}).
   如果集合不存在 则会创建这个集合.

4. 查询.
   db.students.find(); 查询students集合中的所有数据.
   db.students.find().pretty();  格式化查询

5. 条件查询.
   db.students.find({age:20}) 查询年龄为20岁的.

   db.students.find({age:{$gt:50}})    //查找小于等于50的age

   - **lt：less than** 小于
   - **gt：greater than** 大于
   - **eq：equal to** 等于
   - **ne：not equal to** 不等于

6. 删除数据

  db.students.remove({}) 将集合清空.
  db.students.remove({name:"jack"})

7. 更新数据.

   db.students.update({age:18},{name:"jack"})   //替换.
   db.students.update({age:18},{$set:{name:"rose"}})  //更新
   db.students.update({age:18},{$set:{name:"rose"}},{multi:true})  //多行更新



### nodejs操作MongoDB数据库增删查改文档

https://www.npmjs.com/package/mongodb
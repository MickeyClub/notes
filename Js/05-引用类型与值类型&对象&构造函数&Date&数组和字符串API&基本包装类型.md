# JavaScript基础总结05(引用类型与值类型&对象&构造函数&Date&数组和字符串API&基本包装类型)
[TOC]

### 1. 引用类型与值类型区别
- 引用类型(复杂数据类型): Array Object
	- 数据存储在堆中,地址存储在栈中
- 值类型(基本数据类型): String Number Boolean Undefined Null
	- 数据存储在栈中

### 2. 对象
> 用于描述生活中的某个具体的事物的特征和行为

属性:  用于描述对象的特征
方法:  用于描述对象的行为

#### 2.1 基本使用
- 语法:  
	- 构造函数声明: `var 对象名 = new Object();`
	- 直面量方式: `var 对象名 = {};`

- 对象取值与赋值
	- 点语法:  对象名.属性名
	- 字符串语法: 对象名['属性名]'
#### 2,2 遍历对象的属性
语法:  `for(var key in 对象名) {  }`
```javascript
    for(var key in teacher){
        //key是一个属性名的字符串
        console.log ( key );
        /*注意：只能通过字符串语法来取值*/
        console.log ( teacher[key] );
    }
```

### 3. 使用构造函数创建对象
> 如果调用一个函数的时候使用了new关键字,那么这个函数就是构造函数

new关键字原理
1. 在内存中开辟空间,创建一个空对象
2. 将this指向这个对象
3. 进行对象赋值
4. 返回对象

使用构造函数需要注意的点
1. 通常函数名首字母大写，用于区分普通函数
2. 必须要使用new关键字来调用
3. .手动在函数内return造成的影响
	4. 如果 return 基本数据类型 ： 无效，还是会返回默认对象
	5. 如果 return 复杂数据类型 ： 有效，会覆盖默认的对象

### 4. Date对象
> 获取当前时间

```javascript
<script>
    //Date对象：获取时间
    //1.声明Date对象
    var date = new Date();
    console.log ( date );//date对象在打印的时候会自动转成字符串打印

    //2.转成本地时间
    console.log ( date.toLocaleTimeString () );
    console.log ( date.toTimeString () );

    //3.获取时间中的年月日，时分秒
    console.log ( date.getFullYear () );//2018
    //月范围  0-11  表示1-12月
    console.log ( date.getMonth () );//5   6月
    console.log ( date.getDate () );//24
    //day 星期   0-6   星期天-星期六
    console.log ( date.getDay () );//0
    console.log ( date.getHours () );//时
    console.log ( date.getMinutes () );//分
    console.log ( date.getSeconds () );//秒
    console.log ( date.getMilliseconds () );//毫秒
    
    //4.手动创建一个日期
    var date1 = new Date(2020,2,22,18,30,35);
    console.log ( date1 );
    console.log ( new Date ( "2022-05-06 12:03:35" ) );
</script>
```

### 5. 数组对象常用的API
1. 连接数组:concat()
2. 将数组元素拼接成字符串:join()
3. 删除数组最后一个元素:pop()
4. 往数组后面添加元素:push()
5. 翻转数组:reverse()
6. 删除数组第一个元素:shift()
7. 查找数组元素:slice()
8. 数组排序:sort()

```javascript
<script>
    //只要声明了一个数组，那么这个数组就拥有所有的API
    var arr = [88,90,100,66,25];

    //1.将两个数组连成一个数组
    console.log ( arr.concat ( [ 90, 100, 200 ] ) );

    //2.将数组中所有的元素拼接成一个字符串
    console.log ( arr.join () );//'88,90,100,66,25'

    //3.删除并返回数组的最后一个元素
    var last =  arr.pop();//  arr.length--
    console.log ( arr );
    console.log ( last );

    //4.往数组后面添加一个元素
    arr.push ( 65 ) ;//arr[arr.lenth] = 65
    console.log ( arr );

    //5.翻转数组
    console.log ( arr.reverse () );

    //6.删除并返回数组的第一个元素
    arr.shift();
    console.log ( arr );//[66, 100, 90, 88]

    //7.返回数组中执行位置的元素
    //arr.slice(start,end);     start <= 范围 < end
    var newArr =  arr.slice(1,2);
    console.log ( newArr );

    //8.数组排序:参数是一个回调函数
  var newArr =   arr.sort(function(a,b){
        //return a - b;//从小到大
      return b - a;//从大到小
    })
    console.log ( newArr );
</script>
```

### 6. String对象
- 字符串恒定性:
	- 字符串不可以被修改的
	- 般调用字符串api的时候使用新的变量来接收
- 字符串恒定性原理:
	- 1.在内存中有一块专门的内存空间区域负责存储字符串(字符串常量区)
	- 2.(缓存机制)当我们声明一个字符串的时候，编译器会先去这个区域寻找有没有该字符串，如果有就直接取出来，没有就新开辟空间
	- 3.这块内存空间中的字符串一旦被创建，是不可以被修改的

字符串常用API

1. 获取字符串长度:str.length
2. 获取字符串某一个下标字符:str.charAt()
3. 拼接字符串:str.concat
4. 判断字符串中是否包含某些字符串:str.indexOf()
5. 截取字符串:str.substr()
6. 修改字符串:str.replace()
7. 大小写转换:str.toLowerCase()小写 str.toUpperCase()大写
8. 分隔字符串：str.split()

### 基本包装类型
> 主要是为了让基本数据类型也可以调用对象的方法

- 基本数据的对象类型
	- string对象: `new String()`
	- number对象: `new Number()`
	- boolean对象: `new Boolean()`

对开发者影响不大，当我们在使用字符串的api的时候，编译器会自动帮我们转换

```javascript
var num = 10;//为了可以让number这个数据类型能够调用对象的方法，编译器会自动帮我们转成对象(基本包装类型)
// 编译器帮我们做了一件事：  var num = new Number()
num.toString();
```
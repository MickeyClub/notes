#html5day04(localStorage)
[TOC]

### 地理定位
如何获取地理位置?
1. GPS模块 用GPS进行定位,精度是最高的,连接卫星来定位

2. wifi定位 利用的是大数据,你要连接WiFi,本质上连的是路由器,都有一个MAC地址,这就是路由器的唯一标识
	
3. 网络基站定位: 原理相当于WiFi定位 这个定位的精度是最差的,会有误差


使用方法
`navigator.geolocation.getCurrentPosition()`

```htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- 

        获取到你的设备当前所在的经纬度
            
        如何获取地理位置？
                设备有GPS模块： 用GPS进行定位，精度是最高的，连接卫星来定位
                        精度是最高的

                wifi定位：利用的是大数据，你要连wifi，本质上连的是路由器，都有一个MAC地址，这个就是路由器的唯一标识，可能有的时候你连这个WIFI刚好打开了百度地图，那么百度地图会自动获取到你GPS定位的信息，和当前连接的路由器的MAC地址，把这个路由器信息和你定位到的经纬度上传到服务器，假设以后有电脑来连wifi，百度地图知不知道它在哪？知道的

                    我不连wifi，只要开了wifi精度就更高了
                        因为你打开wifi后，手机上就会搜索各种附近的wifi信号，它根据你对各种wifi信号的强弱，利用三角定位来确定你大概位置


                网络基站定位：原理相当于是wifi定位
                        这个定位的精度是最差的，会有误差

    -->
</head>
<body>
    
</body>
</html>

<script>
    //第一：写一个navigator
    //当获取到你的经纬度后会调用的函数，经纬度信息会当做形参传递过来
    navigator.geolocation.getCurrentPosition(function(loc){

        // console.log(loc);
        
        //获取纬度
        console.log(loc.coords.latitude);
        // 获取经度
        console.log(loc.coords.longitude);
        
        
    });
</script>
```

`loc.coords.latitude` 获得纬度
`loc.coords.longtitude` 获得经度




### localStorage
> 可以用来存储数据到浏览器,和cookie很像,将数据存储到浏览器段,但cookie有失效时间,而localStorage没有失效时间

特点:
1. LocalStorage只有自己写代码删或者手动删才会删掉
2. **localstorage把所有数据都当字符串来存,所以取出来也是字符串**
3. 如果是存复杂类型,那么就会调用toString方法来存储
4. 如果要存复杂类型.那么先把复杂的JS数据J转成JSON字符串,然后在存取出来的时候把JSON字符串转回JS字符串

参数:
1. setItem 保存一个值
2. getItem 获取一个值
3. removeItem 删除一个值
4. clear 清空所有
5. 数据存在浏览器,不手动删除就一直存在

```htmlbars
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <input type="button" id="set" value="存储">
    <input type="button" id="set2" value="存储2">
    <input type="button" id="get" value="读取">
    <input type="button" id="remove" value="删除">

    <input type="button" id="arr" value="存数组">
    <input type="button" id="obj" value="存对象">
    <input type="button" id="getArr" value="取数组">
    <input type="button" id="getObj" value="取对象">
    <input type="button" id="clear" value="清空">

</body>

</html>

<script>
    // 存储
    document.getElementById('set').onclick = function () {
        //存储
        //参数1：key
        //参数2：value
       localStorage.setItem('name','Mickey')
    }

    // 覆盖存储
    document.getElementById('set2').onclick = function(){
        //会覆盖同一个键会被覆盖
        localStorage.setItem('name','Motto')
    }

    //获取
    document.getElementById('get').onclick = function () {
        //把读取到的值当做返回值返回
       var res = localStorage.getItem('name');
       console.log(res);

    }

    //移除
    document.getElementById('remove').onclick = function () {
        localStorage.removeItem('name')
    }

    // 存数组
    document.getElementById('arr').onclick = function () {
        //数组
        var arr = [10, 20, 30, 40, 50];
        //取到的是字符串,原因是localStorage在存复杂数据类型是会帮arr用toString()方法
        // localStorage.setItem('arr',arr) //10,20,30,40,50
        //要存复杂数据解决方法:  将复杂的js数据转成json数据,最后取出来在将JSON数据转成js数据
        var str = JSON.stringify(arr);
        localStorage.setItem('arr',str);

    }

    //存对象
    document.getElementById('obj').onclick = function () {
        var obj = { name: "jack", age: 16 };
        // console.log(obj.toString());
        var str = JSON.stringify(obj);
        localStorage.setItem('obj', str);
    }

    //取数组
    document.getElementById('getArr').onclick = function () {
        var res = localStorage.getItem('arr')
        //将JSON对象转成js对象  如果不转成js对象,获取到的是json字符串
        var arr = JSON.parse(res);
        console.log(arr);
    }

    //取对象
    document.getElementById('getObj').onclick = function () {
        var res = localStorage.getItem('obj');
        var obj = JSON.parse(res);
        console.log(obj);
    }

    //清除所有数据
    document.getElementById('clear').onclick = function(){
        localStorage.clear();
    }
</script>
```

百度换肤案例
```htmlbars
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>百度换肤</title>
    <style>
        html, body, div {
            margin: 0;
            padding: 0;
        }

        body {
            width: 100%;
            background: url(4_images/1.jpg) no-repeat center/cover;
        }

        .back {
            width: 100%;
            height: 180px;
            background: rgba(255, 255, 255, 0.3);
            padding-top: 50px;
            text-align: center;
        }

        .back img {
            width: 200px;
            height: 125px;
        }
    </style>

</head>
<body>

<div class="back">
    <a href="4_images/1.jpg">
        <img src="4_images/1.jpg" alt="">
    </a>
    <a href="4_images/2.jpg">
        <img src="4_images/2.jpg" alt="">
    </a>
    <a href="4_images/3.jpg">
        <img src="4_images/3.jpg" alt="">
    </a>
    <a href="4_images/4.jpg">
        <img src="4_images/4.jpg" alt="">
    </a>
    <a href="4_images/5.jpg">
        <img src="4_images/5.jpg" alt="">
    </a>
</div>


</body>
</html>

<script>
    //取出本地的路径,并给body
    var url = localStorage.getItem('skin');

    //如果没有本地路径,这段代码会报错,所以需要加个判断
    if(url)
    document.body.style.background = "url("+ url +") no-repeat center/cover";

    //获取所有a标签
    var aList = document.getElementsByTagName('a');

  //给每个a添加点击事件
    for (var i = 0; i < aList.length; i++) {
        aList[i].onclick = function () {
            //获取到当前点击a的href
            console.log(this.href);
            //用href更改背景图
            document.body.style.background = "url("+ this.href +") no-repeat center/cover"
            //利用localStorage将路径存到本地
            localStorage.setItem('skin',this.href);
            //   阻止跳转
            return false
        }
    }

</script>
```


总结
简单来说就是记录用户本地数据

### sessionStorage
用法和localStorage一样,也是保存用户数据的,但它关闭浏览器就会删除用户数据

使用场合
多用于临时保存一些小东西,例如多个表单传值案例

### 拖拽
> 在html元素中,默认不循序有别的东西拖拽到自己的范围,所以需要阻止默认行为

- 属性
`dragable=true`:允许元素拖拽
- 事件
`ondragstart`: 拖拽开始触发
`ondrag`: 拖拽一直触发
`ondragend`: 拖拽结束触发
**`ondragover`**:  当有元素划入时触发
**`ondrop`** 当有元素在我区域内划入并且松手才触发

注意:`ondrop`这个事件默认不会生效,**必须先在ondragover里阻止事件默认行为才会生效.因为在html元素中,默认不允许其他元素拖拽到自己区域,所以需要阻止默认事件**
	
代码:
```htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .box{
            width: 100px;
            height: 100px;
            background-color: #f00;
        }

        .container{

            width: 600px;
            height: 600px;
            border: 1px solid #000;
            position: absolute;
            right:50px;
            top:50px;
        }
    </style>
</head>
<body>

    <div class="box" draggable="true"></div>

    <div class="container"></div>

</body>
</html>

<script>
    /*
        ondragover：当有元素划入就触发

        ondrop：当有元素在我区域内划入并且松手才触发
                    但是这个事件默认不会生效，必须先在ondragover里阻止事件默认行为后才会生效
    */
    var box = document.getElementsByClassName('box')[0];
    var container = document.getElementsByClassName('container')[0];

    //因为在html元素中,默认不允许其他元素拖拽到自己的范围,所以需要阻止默认事件
    container.ondragover = function (e) {
        e.preventDefault();
    }

    //拖拽松手后触发
    container.ondrop = function () {
       container.appendChild(box);
    }

</script>
```



### 文件读取器
FileReader()对象


### video
- 属性
1. currentTime 获取设置设置视频的当前播放时间
2. duration 获取视频的总时长

- 方法
1. play() 播放视频
2. pause() 暂停视频

- 事件
- ontimeupdate 简单来说就是视频过程中一直触发


###文件读取器
> 传入一个文件,能把文件读取成base64形式的二进制

使用步骤
1. `FileReader()`实例化文件读取器对象
2. `readAsDataURL`读取文件
3. `onload`调用事件

```javascript
        //文件读取器,所有的文件都是二进制，就是把你选择的文件读取成二进制
        var read = new FileReader();
        // 读取文件
        read.readAsDataURL(e.dataTransfer.files[0]);

        // 读取后的结果怎么拿？
        //读取完毕调用的事件
        read.onload = function(){

            // result属性里保存了读取到的文件的二进制信息
            // base64加密后的二进制信息
            console.log(read.result);
            document.body.style.background = "url("+ read.result + ")";
        }
```




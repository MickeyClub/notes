

# Vue.filter过滤器

[TOC]



## 全局过滤器基本使用:

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./lib/vue.js"></script>
</head>

<body>

    <div id="app">
        {{ msg | msgFormat("善良") }}
    </div>


    <script>
        // 定义一个 Vue全局过滤器 名字叫msgFormat        function(参数1: 过滤的对象, 参数2: 替换的内容)
        Vue.filter('msgFormat', function (msg, auguments) {
            // 定义正则    
            return msg.replace(/单纯/g, auguments)
        })

        let app = new Vue({
            el: "#app",
            data: {
                msg: '曾经,我也是一个单纯的少年,单纯的我,傻傻的文,谁是世界上最单纯的男人'
            }
        })
    </script>
</body>

</html>
```



## 日期格式化(轮子)

```javascript
// 自定义过滤器 进行日期格式改为 yyyy-mm-dd
Vue.filter('dateFormat', function (dateStr, pattern) {
    // 根据传递过来的参数 放回特定的时间戳
    let dt = new Date(dateStr);
    // 年月日
    let y = dt.getFullYear();
    let m = dt.getMonth() + 1;
    let d = dt.getDate().toString().padStart(2,0);
    // 自定义时间戳 转小写   如果没参数 默认 yyyy-mm-dd-hh-mm-ss 
    if (pattern && pattern.toLowerCase == "yyyy-mm-dd") {
        return `${y}-${m}-${d}`
    } else {
        // hh = hh < 10 ? '0' + hh : hh;
        // 时分秒      padStart(maxLength, string))
        let hh = dt.getHours().toString().padStart(2,0);
        let mm = dt.getMinutes().toString().padStart(2,0);
        let ss = dt.getSeconds().toString().padStart(2,0);
        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
    }
})
```



## 使用Moment插件实现自定义日期格式

官网: http://momentjs.cn/

```javascript
// 导入moment.js时间插件
import moment from 'moment';
// 全局过滤器
Vue.filter('formatTime',(data,format='YYYY-MM-DD')=>{
    // 不传参数默认是(YYYY-MM-DD)
  return moment(data).format(format)
})

// 管道符  (自定义传参)
<span>{{item.add_time | formatTime('YYYY-MM-DD hh:mm:ss')}}</span>
```


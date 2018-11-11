# 移动web开发06(移动端Touch&手动tap封装&rem)
[TOC]

### 1. 移动端Touch触摸相关事件
1. touchstart: 当在屏幕上按下手指时触发
2. touchmove: 当在屏幕上移动手指时触发
3. touchend: 当屏幕上抬起手指时触发
4. touchcancel: 当一些更高级别的事件发生的时候（如电话接入或者弹出信息）会取消当前的touch操作，即触发touchcancel。一般会在touchcancel时暂停游戏、存档等操作

### 2. touch事件与cllick同时触发
> 如果使用了触摸事件,可以调用 event.preventDefault()来阻止触发默认事件

在很多情况下，触摸事件和鼠标事件会同时被触发（目的是让没有对触摸设备优化的代码仍然可以在触摸设备上正常工作）

因为双击缩放检测的存在，在移动设备屏幕上点击操作的事件执行顺序：
touchstart(瞬间触发) => touchend => click(200~300yms延迟)
### 3. 手动封装移动端tap事件
> 由于点击事件经常使用，如果用click会有延迟问题，一般我们会用touch事件模拟移动端的点击事件, 下面是简单的封装

```htmlbars
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: red;
        }
    </style>
</head>

<body>
    <div id="div"> </div>

    <script>
        // 获取div元素
        var div = document.getElementById('div')
        // 普通点击事件
        div.addEventListener('click', function () {
            console.log("普通click事件");
        })
        // 不延迟的点击事件
        tap(div, function () {
            console.log("不延迟的click事件");
        })

        // 第一个参数是dom元素,第二个参数是回调函数
        function tap(dom, callback) {

            var flag = true
            // 触摸开始
            dom.addEventListener('touchstart', function () {
                // console.log('touchstart');
            })
            // 触摸过程
            dom.addEventListener('touchmove', function () {
                console.log('touchmove');
                flag = false
            })
            // 触摸结束
            dom.addEventListener('touchend', function () {
                if (flag) {
                    // 短路运算  前面条件不成立 后面不执行
                    callback && callback();
                }
            })
        }
    </script>
</body>

</html>
```

### 4. 移动端点击穿透问题
如果某个返回按钮的位置，恰好在要返回的这个页面的带有href属性的a标签的范围内，在点击返回按钮后，页面快速切换到有a标签的页面，300ms后触发了click事件，从而触发了a标签的意外跳转，这个就是典型的点击穿透问题。罪魁祸首其实就是a标签跳转默认是click事件触发，而移动端的touch事件触发之后，依然会在300ms后触发click事件。

解决办法：
1.就是阻止触发touch事件完成后的click事件。
2.不要混用touch和click事件。显然不可能都绑定click事件，因为要解决300ms延迟问题(除了fastclick)，那么只能都绑定touch事件，这样click事件永远不会被触发。


### rem布局

##### rem是什么
1. rem是什么：rem root element 根元素(html元素) 是一个单位 这个单位的大小参照根元素的字体大小
2. em是什么：  element 当前元素 是一个单位 这个单位的大小参照当前元素的字体大小

##### rem的好处
1. rem大小是固定参照根元素的font-size  使用的时候rem值相对统一
2. 只需要修改根元素的font-size 可以实现全局修改 可以实现修改根元素字体大小实现全屏的缩放
3. 实现自适应缩放布局

##### 使用rem实现屏幕的适配

1. 针对当前写代码兼容的屏幕 作为标准屏幕
2. 给标准屏幕设置一个标准的根元素的字体大小 （千万要注意至少12以上） 一般以100px作为标准比较缩放100倍的rem  100px = 1rem 10px = 0.1rem
3. 针对1rem = 100px 把页面所有使用了px的单位都改成rem 全部缩小一半
4. 注意要把body的字体大小设置为默认16px
5. 使用JS来获取当前屏幕的宽度  / 标准屏幕 * 标准屏幕大小   == 当前屏幕要设置的根元素的字体大小 给当前html设置这个大小即可
6. 如果需要当屏幕宽度变化后自适应变化把JS计算和改变的代码放到resize里面

##### 实现代码
```htmlbars
<style>
    html {
        font-size: 100px;
    }

    body {
        font-size: 16px;
    }
</style>
<script>
    window.addEventListener('resize', setHtmlFontSize);
    setHtmlFontSize();

    function setHtmlFontSize() {
        // 根据屏幕宽度来改变跟元素的字体大小的值
        // 公式:　　当前屏幕的宽度 / 标准宽度(375) * 100　　　比如当前750/375*100 = 200px/html
        var windowWidth = document.documentElement.offsetWidth;
        // 5. 限制最大屏幕和最小屏幕
        if (windowWidth > 750) {
            windowWidth = 750;
        } else if (windowWidth < 320) {
            windowWidth = 320
        }
        // 标准屏幕的宽度
        var StandarWidth = 375;
        // 标准屏幕的html字体大小
        var StandarHtmlFontSize = 100;
        // 3. 当前屏幕需要设置的根元素字体大小
        var htmlFontSize = windowWidth / StandarWidth * StandarHtmlFontSize;
        // 4. 将当前计算的html字体大小设置到页面的html元素上
        // console.log(htmlFontSize);
        document.querySelector('html').style.fontSize = htmlFontSize + 'px';
    }
</script>
```
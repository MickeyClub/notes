# Mobil移动端(乐淘)

[TOC]



### Mui使用

#### Mui区域滚动(轮播图)

文档: http://dev.dcloud.net.cn/mui/ui/#slide

mui-slider插件

html

```
<!-- 区域滚动的父容器 -->
  <div class="mui-scroll-wrapper">
      <!-- 区域滚动的子容器 -->
      <div class="mui-scroll">
         <!-- 真实的内容放到区域滚动的子容器里面 -->
         <section id="slide">
         </section>
         <nav id="nav">
         </nav>
      </div>
  </div>
```

js

```
$(function () {
    var letao = new Letao();
    // 轮播图初始化
    letao.initSlide();
    // 页面区域滚动
    letao.initScroll()
})

var Letao = function () {
}

Letao.prototype = {
    // 初始化轮播图方法
    initSlide: function () {
        //获得slider插件对象
        var gallery = mui('.mui-slider');
        gallery.slider({
            interval: 5000 //自动轮播周期，若为0则不自动播放，默认为0；
        });
    },
    // 初始化区域滚动
    initScroll: function () {
        mui('.mui-scroll-wrapper').scroll({
            deceleration: 0.0005, //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006,
            scrollY: true, //是否竖向滚动
            scrollX: false, //是否横向滚动
            startX: 0, //初始化时滚动至x
            startY: 0, //初始化时滚动至y
            indicators: true, //是否显示滚动条
            deceleration:0.0006, //阻尼系数,系数越小滑动越灵敏
            bounce: true //是否启用回弹

            // 以上都是默认值 如果都一样可以不设置参数
        });
    }
}
```

#### 下拉刷新和上拉加载插件的使用

1. 写一个页面结构

```
<!-- 区域滚动的父容器 -->
<div class="mui-scroll-wrapper" id="refreshContainer">
  <!-- 区域滚动的子容器 -->
  <div class="mui-scroll">
    真实要刷新的内容
     <!-- 搜索表单 -->
            <div class="search-form">
            </div>
      <!-- 商品列表 -->
           <div class="product-list">
           </div>
  </div>
</div>
```

1. 写JS初始化下拉刷新

```
//初始化插件
 mui.init({
   //初始化上下拉刷新的
    pullRefresh: {
      // 初始化下拉和上拉的容器
      container: ".mui-scroll-wrapper",
      //初始化下拉刷新
      down:{
        //下拉刷新的回调函数 写请求最新数据 刷新页面 结束下拉刷新
        callback:function(){
           $.ajax({
              url:'url',
              success:function(){
                //请求完毕并且页面渲染完毕
                //调用结束下拉刷新  千万注意不能使用官网文档函数 使用百度或者官方demo的代码
                mui('#refreshContainer').pullRefresh().endPulldownToRefresh();
              }
           })
        }
      },
      //初始化上拉加载更多
      up{
        //上拉加载的回调函数 写请求下一页的数据 追加页面 结束上拉加载
        callback:function(){
           $.ajax({
              url:'url',
              data:{page:下一页的数据}
              success:function(){
                //请求完毕并且页面渲染完毕
                //调用结束上拉加载更多  千万注意不能使用官网文档函数 使用百度或者官方demo的代码
                mui('#refreshContainer').pullRefresh().endPullupToRefresh();
              }
           })
        }
      }
    }
 });
```

注意

1. 调用结束下拉刷新的方法结束下拉刷新
   mui('#refreshContainer').pullRefresh().endPulldownToRefresh();
   （如果不结束就会一直转圈圈）
2. 重置上拉加载更多效果（上拉加载更多到底了之后就无法拉了 重置一下）
   mui('#refreshContainer').pullRefresh().refresh(true);
3. 结束上拉加载更多（如果不结束就会一直转圈圈）
   mui('#refreshContainer').pullRefresh().endPullupToRefresh(false);
4. 如果返回数据数组没有长度表示没有数据了 结束上拉加载更多 并且提示没有更多数据
   mui('#refreshContainer').pullRefresh().endPullupToRefresh(true);

#### Mui-input(登录组件)

登录表单组件： http://dev.dcloud.net.cn/mui/ui/#input

1. 给登录按钮添加点击事件
2. 获取当前输入的用户和密码
3. 判断当前是否输入了用户名和密码 如果没有输入就提示用户输入
4. 调用登录的API实现登录的功能

#### MUI侧滑列表插件的使用

1. 侧滑列表也是列表

   ```
   <li class="mui-table-view-cell mui-transitioning">
     <div class="mui-slider-right mui-disabled">
       <a class="mui-btn mui-btn-grey mui-icon mui-icon-person" style="transform: translate(0px, 0px);"></a>
       <a class="mui-btn mui-btn-yellow mui-icon mui-icon-phone" style="transform: translate(-90px, 0px);"></a>
       <a class="mui-btn mui-btn-red mui-icon mui-icon-email" style="transform: translate(-180px, 0px);"></a>
     </div>
     <div class="mui-slider-handle" style="transform: translate(0px, 0px);">
       <div class="mui-table-cell">
         左滑显示多功能菜单
       </div>
     </div>
   </li>
   ```

   

2. 但是侧滑列表里面需要有2个容器
   左边容器的类名必须为class="mui-slider-handle"
   右边的按钮的容器类名必须为class="mui-slider-right"

3. 拷贝的代码里面的行内样式都可以去掉

### artTemplate模板引擎

文档: https://aui.github.io/art-template/zh-cn/index.html

1. artTemplate目前流行有3.0和4.0版本

2. 3.0 使用 **each 数组 as value**  => {{each list as value }} 

3. 4.0 使用 **each 数组 value 省略了as** =>  {{each list value }} 

4. 3.0的包: template.js  

5. 4.0的包: template-web.js

   ```
   // 3.0
   <script src="lib/artTemplate/template.js"></script>
   // 4.0
   <script src="lib/artTemplate/template-web.js"></script>
   ```

6.  模板引擎语法支持2种格式 通常是对象

   1. 数组 
   2. 对象

基本使用

```
   // 引入模板
   <script src="lib/artTemplate/template-web.js"></script>
   // html 模板
   <script id="historyTmp" type="text/html">
        {{each list value }}
            <li>
				{{value.id}}
            </li>
        {{/each}}
    </script>
    // js
    var html = template('historyTmp',{'list': historyList})
    // 渲染页面
    $('div').html(html);
```





### 移动端rem适配

1. 指定一个标准屏幕  375 iphone6 7 8标准屏幕 
2. 指定标准屏幕根元素字体大小100px  1rem=100px  
   把之前写px代码 /100即可 100px  1rem 10px 0.1rem
3. 把body还原成默认16px  或者写成 0.16rem; 
4. 把rem的js代码引进来（根据不同屏幕 改变根元素的字体大小） 375  根元素100px  750 根元素 = 200 
   当前屏幕宽度 / 标准屏幕宽度 * 标准屏幕根元素的字体大小
     750 / 375 * 100  == 200
5. 把之前写CSS的里面使用px的单位都转成rem  http://alurk.com/
   把CSS代码复制到工具里面设置 设置设计稿大小 和 基础字体值
   后面的媒体查询的复选框去掉
   把所有CSS转好替换之前写px单位的CSS

```
// 监听屏幕大小事件
window.addEventListener('resize',setHtmlFontSize);

// 初始化rem
setHtmlFontSize();

// 根据当前宽度改变rem
function setHtmlFontSize() {
    // 根据屏幕的宽度来改变根元素的字体大小的值
    // 当前屏幕 / 标准的375屏幕 求得你当前屏幕是标准屏幕的多少倍  * 标准屏幕根元素的字体大小
    // 当前屏幕的宽度 / 375 * 100
    // 假如当前750/375 = 2 * 100 == 200px  
    // 1. 当前屏幕的宽度
    var windowWidth = document.documentElement.offsetWidth;

    // 限制最大屏幕 和最小屏幕
    if(windowWidth > 640){
        windowWidth = 640;
    }else if(windowWidth < 320){
        windowWidth = 320;
    }

    //2. 标准屏幕的宽度
    var StandardWidth = 375;
    // 标准屏幕的字体大小
    var StandardHtmlFontSize = 100;
    // 根据公式(当前屏幕 / 标准屏幕 * 标准屏幕字体大小)
    var htmlFontSize = windowWidth / StandardWidth * StandardHtmlFontSize;

    // 计算结果改变html的根元素
    document.querySelector('html').style.fontSize = htmlFontSize + 'px';
}

```

### 让手机和电脑连接同一个网络

1. 手机开热点电脑连接
   电脑连接wifi

2. 查看手机热点wifi的ip地址

3. 打开网络共享中心 打开wifi设置 详细信息  172.20.10.2

4. 把localhost换成这个Ip地址 172.20.10.2
   http://localhost:3000/m/index.html 

   > http://172.20.10.2:3000/m/index.html
   > 把替换后的网站刷新一下 然后扫描或者发送给手机即可


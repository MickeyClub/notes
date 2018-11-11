## 总结

1. 分类左侧的点击吸顶
2. 移动端的滑动事件
3. 移动端点击事件延迟和解决
4. 移动端的zepto框架
5. rem的概念和实现屏幕适配
6. rem工具的使用实现jd项目
7. less的编译和gulp的了解

## 分类左侧的点击吸顶效果


### 1. 需求： 点击左侧分类的某个分类 要让当前的分类吸顶

### 2. 实现思路

  1. 给所有li添加事件 获取哪个被点击的li
  2. 计算点击了的li需要位移的距离 当前- li的索引*高度
  3. 给swiper-wrapper滑动的容器设置位移 位移到计算的距离

### 3. 实现步骤

  1. 给所有的li添加点击事件
  2. 给所有的li添加一个索引
  3. 给所有li删除active类名 
  4. 获取当前点击li的索引和高度
  5. 计算当前要位移的距离 千万注意这个值后面使用的时候要带px单位
  6. 计算最大位移的距离 父元素固定高度 -  子元素不固定的高度
  7. 判断如果当前位移的距离小于了最大位移的距离 就设置为最大的位移距离
  8. 把位移距离设置到滑动的swiper-wrapper身上
  9. 吸顶的时候添加过渡的效果


## 移动端的事件

1. touchstart  滑动开始事件
2. touchmove   滑动中的事件
3. touchend    滑动的结束事件
4. touchcancel 滑动的中断事件

## 使用touch事件实现滑动效果


1. 要给元素添加滑动事件 touchstart touchmove touchend事件
2. 要获取滑动开始的时候手指的坐标 和  滑动中的时候手指的坐标
3. 使用滑动中的坐标 -  开始的坐标   求得手指移动的距离
4. div位移设置位移当前手指位移的距离


##  移动端的jquery  （zepto）


1. 在移动端有一个类似一jquery的框架 叫zepto
2. zepto的用法和作用和jquery一模一样
3. jquery 获取元素 添加事件 样式操作 类名操作 节点操作 动画操作 发送请求
4. zepto 获取元素 添加事件 样式操作 类名操作 节点操作 动画操作 发送请求 滑动事件的操作
5. 区别
    1. jquery是针对PC端   体积比较大  兼容性好   jquery所有功能都在一个文件  jquery针对PC端
    2. zepto是针对移动端  体积小  兼容性差(只针对高级浏览器)  zepto 所有功能分开的 主模块里面只有5个功能其他功能需要单独引入 (模块化的) zepto针对移动端有一些针对移动端的特殊的一些方法 Touch触摸相关的
6. 使用方式和jquery一模一样

7. 使用不一样的地方就是要使用一些非主模块的东西要单独引入
     <!-- selector支持了jquery的一些扩展选择器first eq  -->
     <script src="jd/lib/zepto-js/selector.js"></script> 
     <!-- fx是支持动画animate方法的包 --> -->
     <script src="jd/lib/zepto-js/fx.js"></script>
     <!-- touch是支持移动端一些滑动事件 tap 左右滑动等事件  -->
     <script src="jd/lib/zepto-js/touch.js"></script> 
8. 也可以使用编译工具来把一些非主模块的包编译到主模块里面
  


## 常见的网页的布局方式

1. 固定宽高布局
2. 百分比布局
3. 伸缩布局flex布局
4. 响应式布局
5. 把伸缩+百分比结合在一起  伸缩百分比布局
6. rem布局（在移动端最好的一种布局方式 PC端不好用）

## rem布局

### rem是什么

1. rem是什么：rem root element 根元素(html元素) 是一个单位 这个单位的大小参照根元素的字体大小
2. em是什么：  element 当前元素 是一个单位 这个单位的大小参照当前元素的字体大小

### rem的好处

1. rem大小是固定参照根元素的font-size  使用的时候rem值相对统一
2. 只需要修改根元素的font-size 可以实现全局修改 可以实现修改根元素字体大小实现全屏的缩放
3. 实现自适应缩放布局

### 使用rem实现屏幕的适配

1. 针对当前写代码兼容的屏幕 作为标准屏幕
2. 给标准屏幕设置一个标准的根元素的字体大小 （千万要注意至少12以上） 一般以100px作为标准比较缩放100倍的rem  100px = 1rem 10px = 0.1rem
3. 针对1rem = 100px 把页面所有使用了px的单位都改成rem 全部缩小一半
4. 注意要把body的字体大小设置为默认16px
5. 使用JS来获取当前屏幕的宽度  / 标准屏幕 * 标准屏幕大小   == 当前屏幕要设置的根元素的字体大小 给当前html设置这个大小即可
6. 如果需要当屏幕宽度变化后自适应变化把JS计算和改变的代码放到resize里面


## 使用rem实现jd的适配

1. rem改变根元素的字体大小的js代码放到rem.js文件 （专门用来改变根元素字体大小的js代码）
2. 在京东的页面中引入rem.js文件
    <!-- 3. 引入rem.js 要在前面引入因为他会影响到页面渲染提前引入 -->
    <script src="js/rem.js"></script>
3. 给html设置默认100px字体大小和body设置默认16px
    /* 给默认所有页面都设置字体大小100px */
    html{
        font-size: 100px;
    }
    /* 给body还原成16px; */
    body{
        font-size: 16px;
    }
4. 要把所有使用px单位都变成rem单位 使用rem转换工具  http://alurk.com/


## less的编译

1. 安装nodejs  一定要装在默认的C:\Program Files\nodejs目录
2. 安装less的插件
    1. 打开cmd黑窗 
    2. 执行 npm i less -g
    3. 输入 lessc -v 回车 有版本号就表示安装成功
    4. 注意要联网
3. 找到你要编译的less文件 找到jd里面的less文件夹
4. 在less文件夹下 按住shift+右键 在此处打开窗口  在当前目录打开Node的黑窗
5. 输入 lessc index.less index.css
6. 输入 lessc category.less category.css


## 使用一些高级自动化工具编译代码

1. 把jd-rem-gulp里面的 gulpfile.js package.json 拷贝到你jd项目根目录
2. 在此处打开黑窗按住shift+右键 打开黑窗
3. 输入npm install  安装gulp需要的依赖包
4. 运行glup命令自动帮你把项目打包  把代码帮你压缩编译拷贝等等所有死操作


### 找bug技巧

1. 多看bug(见识广)
2. 会找bug
  1. 页面结构bug   标签是否有结束 标签是否会多加结束  标签单词错误 
  2. 页面样式bug  
      1. 样式没出来 
        1. 样式文件没引入
        2. 样式生效没有（审查元素找到要设置样式的元素） 如果看不到 选择器选错 类名或者id写错
      2. 样式有但没有效果
        1. 样式属性是否写 属性前有感叹号属性错了 或者值
        2. 样式属性是否被覆盖或者继承 样式属性有没有中划线
      3. 自己不知道怎么写样式  居中（内容居中text-align:center）块居中margin:0 auto;
      4. 引入bootstrap会出的样式问题
        1. 没有引入包
        2. 类名错误
        3. 发现覆盖不了bootstrap选择器优先级不够
  3. 页面功能bug
    1. 功能没生效
      1. 文件没引入
      2. 依赖的文件没引入（jquery  zepto  fx动画等等）
      3. 是否定义函数 有没有被调用
      4. 代码是否报错 （单词错 赋值错 变量名。。）
      5. 看代码是否执行 （断点调试看看代码是否执行（事件没被触发 事件名错误 元素没获取到））
      6. 一些获取值的方式错误（兼容性问题 单词错误）
      7. 逻辑错误  (代码执行顺序是否符合你写的顺序 )
      8. 变量名重复 全局已经有变量 局部又用var
  4. 环境系统bug

3. 善于总结
  1. 出了bug记录下来 bug现象 bug原因 解决方案


## 总结

1. 移动web开发的现状： 前端最热门的开发  市场大 需求大 工资高 代码少 兼容性少 容易学
2. 移动web的开发方式
    1. 响应式开发方式 一个页面适配多个终端
    2. 原生开发方式 单独PC和移动端都写一套代码
3. 响应式开发的原理： 媒体查询
   @media(width:值){
    //条件成立执行的代码
   }
  判断有3种
  width:值  等于这个值
  min-width:值 大于等于这个值  从小到大写 向下覆盖
  max-width:值 小于等于这个  从大到小写  向上覆盖

4. 响应式的开发框架
    bootstrap  最常用的
    MUI 
    AmazeUI 

5. bootstrap的使用
    1. 下载包
    2. 看懂文档 全局CSS样式（一些简单类名） 组件 （标签类名组合在一起的效果但是没有交互） 插件 （组件并带有交互）
    3. 找到需要的样式或者组件插件 复制结构
    4. 需要修改样式 审查元素找到样式对应的类名 覆盖 推荐外面使用id 方便覆盖

6. 容器   container 布局容器  居中  栅格系统 行和 列 row放到container里面 col放到行里面 
  列有4个档次 xs sm md lg
  一行最多分为12列  col-xs-1  col-xs-12

7. 导航条 轮播图 折叠菜单collapse  标签页 表单表格 媒体对象 弹出框 提示工具 模态框 按钮 响应式工具

8. 字体图标上传svg图标下载代码引入css使用类名   原理是使用CSS3 web字体 引入对应字体文件 使用图标对应的编码

9. 移动端基本概念  手机分辨率通常比真实宽高大2倍 图片都是按照分辨率来设计 页面CSS宽高是按照真实的宽高设置 使用图片和背景图要缩小一半来写 
10. 搭建jd首页布局 百分比布局 + flex布局
11. 顶部渐变效果 获取滚动条高度
12. 倒计时JS 获取时间计算 分别计算时分秒十位个位来显示倒计时
13. 轮播图使用swiper轮播图
14. 背景图背景图定位原点和背景图拆切
15. 分类左右2侧 flex布局
16. 分类左右2侧滑动使用swiper滑动
17.  swiper插件的使用
   1. 引入包 css
   2. 引入js
   3. 写页面结构
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          内容或者图片
        </div>
      </div>
    </div>
    4. 如果滚动还需要设置样式
      .swiper-container{
          height:100%;
      } 
      body,html父元素等都要设置100%高度
      .swiper-slide 高度自动
    5. 对轮播图插件去初始化
        // 3. 初始化swiper的滑动
        var swiper = new Swiper('.swiper-container', {
         // 控制轮播图滚动的方向 horizontal水平 vertical 垂直
            direction: 'vertical',
           //可以支持多个swiper-slide 可以有多个轮播图
            slidesPerView: 'auto',
            //支持弹簧
            freeMode: true,
            //控制轮播图动画切换的速度  轮播图动画的时间
            speed: 300,
            //添加一个小手
            grabCursor: true,
            // 添加循环 无缝轮播图 
            loop: true,
            //添加自动轮播图 delay自动轮播的间隔时间
            autoplay: {
                delay: 1000,
                //到最后一张停止自动轮播图 但是loop了后就停不下来了
                stopOnLastSlide: true,
                // 是否要当触摸的时候禁止自动轮播图  ture就禁止 false不禁止
                disableOnInteraction: false,
            },
            // 给图片直接添加间距
            // spaceBetween : 100,
            // 如果需要分页器  小圆点
            pagination: {
                el: '.swiper-pagination',
            },
            // 如果需要前进后退按钮  左右箭头
            navigation: {
                nextEl: '.swiper-button-next',
                prevEl: '.swiper-button-prev',
            },
            //支持鼠标滚轮  只有PC能用
            mousewheel: true,
        });

18. 移动端点击事件 有300ms延迟 可以使用touch事件模拟click 也可以使用第三方包fastclick解决
19. 常见移动端滑动事件 touchstart touchmove touchend e事件源对象 e.touches所有手指 
   e.touches[0].clientX 手指在页面上水平位置 e.touches[0].clientY 垂直位置
20. 移动端zepto库 类似jquery 有jquery就不要使用zepto 不要同时引入2个包 $ 会冲突
21. rem: root element根元素 参照根元素html元素的字体大小 1html的字体大小16px 1rem=16px
22. rem的好处 相对html字体大小html字体大小不会随意变化 统一所有使用rem的大小 实现宽度和高度的和内容都自适应
23. 使用rem实现网页自适应的原理是通过媒体查询 或者JS来不断改变html的字体大小html字体大小改变 那么使用rem单位大小都会被改变
24. rem工具  http://alurk.com/ 使用 把px转成rem按照750设计稿调试的时候也是按照750屏幕调试
  把页面做的跟设计稿一模一样 把px转成rem并且html字体大小也跟着变化 会在所有屏幕都进行等比例缩放 整个页面自适应 注意改了html字体大小 整个页面都会继承html字体 文字可能会很大 把body字体大小设置回16px



## 找网站和扒网站

1. bootstrap官网的案例 http://expo.bootcss.com/
2. 妹子UI的官网的案例 http://amazeui.org/showcase/
3. 无忧网络官网的案例 http://www.cnitc.net
4. 招聘网站上的一些项目
5. 扒网站 
    1. 打开 TeleportUltraPortable.exe 程序
    2. 点击左上角的file > new project wizard
    3. 点击第二个单选按钮 Duplicate  
    4. 点击下下一步 输入要扒的网址 up里面是扒几层
    5. 一路下一步 保存到一个目录
    6. 点击中间上方的蓝色开始箭头开始扒 
    7. 扒完了在目录旁边出一个文件夹 就是网站的文件夹

## 前端学习路线

1. https://zwxs.github.io/FrontEndMaterial/synthesis.html
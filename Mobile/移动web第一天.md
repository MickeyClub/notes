##  总结 
1. 移动web开发的概念和特点
2. 移动web的开发方式和对比
3. 响应式开发原理
4. 响应式开发框架bootstrap
5. bootstrap框架的使用（引包）
6. 视口viewport的概念和相关参数的意思
7. bootstrap的常用全局CSS样式使用
8. 熟练使用bootstrap的栅格系统实现布局
9. 实现微金所项目搭建和微金所的顶部和导航

## 移动web和电商全端课程

1. 移动web响应式项目
2. 移动web原生项目
3. 电商全端的完整移动端电商项目


## 移动web


1. 移动web响应式项目 3天
2. 移动web原生项目 3天


## 移动web开发的介绍

  1. 认识移动web开发： 
      1. 什么是移动web ： 在移动端手机端浏览器打开的网站 京东手机版 淘宝触屏版 手机腾讯网 手机微博 手机百度  
          特点页面针对手机端 布局也是针对手机屏幕的布局 布局类似App 触屏淘宝和App淘宝  移动web站点的域名 m.jd.com m.taobao.com 域名一般以m
      2. 什么PC端的web： 在PC端电脑端浏览器打开的网站 京东 淘宝 百度主站 百秀
          特点： 布局针对电脑的布局 东西特别多 特别大 站点www.jd.com www.taobao.com  一般都是www开头
  2. 为什么要学习移动web 和 发展前景
      1. 移动web发展历史 最早在2011-2012兴起 iphone4s发布 安卓广泛流行 有了智能触屏手机 就很多人使用智能手机上网 用到浏览器用到网站（但是那个时候针对手机网站特别少）  
      2. 移动web火热  2014 智能手机基本普及 HTML5的正式定稿 HTML5 给前端开发带了很多变化 最主要的就是能够使用开发web的方式来开发跨平台的移动App  前端行业也是从2014火起来直到现在一直很火
  3. 如何进行移动web开发和PC端开发的区别
      1. 移动开发的方式有2种方式
          1. 响应式方式 ： 一个页面同时适配多个设备 当屏幕宽度发生变化 页面布局会也跟着自动的响应变化
              好处： 只要写一个页面 开发维护快  
              缺点： 加载速度慢（因为PC和移动端代码都在一个页面）
              应用： 新建的网站 同时要开发PC和移动端
          2. 原生方式： 分别PC端开发一个页面 移动端也开发一个页面 写2个页面 判断屏幕宽度 如果是PC端 就去加载PC端页面 如果移动端就加载移动端的页面
              好处： 针对PC和移动端写的2个页面 代码分开了 加载速度快
              缺点： 开发慢 维护也慢
              应用： 老项目 PC已经上线再次开发移动端会分别开发  对性能要求高的网站也会单独开发2个


## 响应式开发

  1. 什么是响应式: 一个页面可以同时兼容PC和移动端 因为屏幕不一样 PC和移动端布局也不一样 响应式就是变化屏幕的宽度就去自动变化布局
  2. 响应式的原理：
      1. 获取屏幕宽度的变化 
      2. 在哪个屏幕 使用哪个布局
  3. 响应式需要兼容的屏幕
      1. 大电脑（笔记本 大台式机）：屏幕判断是>1200都是大电脑 大屏幕
      2. 小电脑（老的台式机）： 屏幕>992 并且 < 1200          中屏幕
      3. 平板电脑（ipad） : 屏幕 > 768 并且 < 992             小屏幕
      4. 手机 ： 屏幕 < 768                                   超小屏幕

  4. 响应式开发 就针对4个屏幕来进行响应式变化
    

## 响应式开发框架

  1. 为什么要用框架： 手写媒体查询完成整个响应式网站 要写很多判断代码量很大  会把常见的响应式的功能封装 就称之为一个框架
  2. 使用别人封装好的响应式框架即可：
      1. bootstrap 框架 前端最流行的框架 UI框架
      2. AmazeUI 妹子UI 中国人开发（模仿bootstrap的框架） UI框架
      3. MUI 米MUI 中国人开发的 （模仿bootstrap的框架）用来实现移动端的响应式布局的UI框架
      4. Layui 中国人开发（模仿bootstrap的框架） UI框架
  
## Bootstrap 


1. 如何学习框架

    1. 找对一个框架 有前途 适合你 入门简单的
    2. 看框架文档  https://v3.bootcss.com/ 
          1. 点击起步  ： 下包和引包
            下包通常有2种  第一种是源码 框架的源代码 里面也有编译好包 一般在dist目录 或者build
            第二种 开发需要用到的包  
          2. 引包：  要引入的bootstrap-3.3.7-dist里面的包
            通常会把第三方包放到lib文件夹里面   只保留bootstrap文件夹名称
            1. 引入bootstrap的CSS文件
            2. 引入 jquery  jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边)
            3. 引入bootstrap的js文件加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件



## 上午复习

1. 移动web开发的介绍和 开发方式的区别
2. 响应式的开发的原理 使用JS来实现响应式 
3. 响应式开发原理媒体查询 
4. 响应式开发的框架   bootstrap AmazeUI MUI Layui
5. 了解bootstrap框架  针对响应式开发的UI框架 也是移动优先的框架
6. 如何使用bootstrap框架
    1. 下载包  下载生成环境使用是bootstrap (在开发和上线的时候使用bootstrap) 另外还有一个是源码（使用源码里面的dist目录里面的内容）
    2. 引包：  1.引入bootstrap的CSS 2.引入jquery 3. 引包bootstrap的js
7. 引包的规则
    1. 如果有第三方的包 要先引入第三方的包 再引入自己的包 （为了方便覆盖框架的样式 或者方便使用框架里面的函数）
    2. 如果有依赖关系 bootstrap依赖jquery 先引入jquery再bootstrap(要先引入依赖的再引入不依赖)
    3. 如果CSS 放到head标签里面引入
    4. 如果JS 放到body结束标签的上面 
        1. 等dom元素加载完毕再引入js
        2. 保持html的结构 内容都是放到身体放到body
        3. 为了让页面有一个根节点
    5. 有一些特殊的JS对页面渲染有影响的JS就要放到head里面  兼容IE8 编译样式等等



## 学习bootstrap框架的使用


  1. 全局CSS样式  单个简单类名就是全局CSS样式
  2. 组件         组合起来的一堆控件（按钮组 输入框组 列表组件） 组件通常的静态
  3. javascript插件  组件的升级版 由很多标签组合在一起 并且有动态效果交互


## 1. 全局CSS样式

  1. 使用HTML5的文档
  2. 给页面添加视口
  3. 自带了清除默认样式的 Normalize.css
  4. 布局容器 （版心容器）  重点    /*固定宽度且居中容器*/ /*全屏容器*/
  5. 排班： 设置文本的样式 大小 字体 加粗 下划线 删除线 斜体 对齐 大小写
  6. 代码: 在页面中插入一些代码 表达式 代码段等
  7. 表格： table 表格的样式 表格边框 悬停 背景色等
  8. 表单： 让输入框 按钮更加大气 和各种组合表单
  9. 按钮： 有一个比较大气的按钮 改变按钮大小 颜色 边框等
  10. 辅助类： 和页面相关一些小样式 字体颜色 背景颜色 三角符号 xx符号 浮动 清除浮动 显示隐藏  居中等
  11. 响应式工具： 控制页面元素在什么屏幕显示什么屏幕隐藏


## 项目搭建

  1. 创建weijinsuo文件夹
  2. 打开1-教学资料 》 微金所完整版 》 复制img fonts lib文件夹
  3. 粘贴在weijinsuo文件夹里面
  4. 在weijinsuo文件夹里面新建index.html
  5. 在index.html创建html文档
  6. 在index.html里面引包
      <!-- 1. 引入bootstrap的css文件 -->
      <link rel="stylesheet" href="lib/bootstrap/css/bootstrap.css">
      <!-- 2. 引入jquery的js文件 -->
      <script src="lib/jquery/jquery.js"></script>
      <!-- 3. 引入bootstrap的js文件 -->
      <script src="lib/bootstrap/js/bootstrap.js"></script>
  7. 添加视口
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">

## 实现首页布局

  1. 实现首页结构划分
      头部
      导航条
      轮播图
      支付保障
      立即预约
      产品推荐
      新闻
      合作伙伴
    <!-- 头部区域 -->
    <header id="header">
    </header>
    <!-- 导航区域 -->
    <nav id="nav">
    </nav>
    <!-- 轮播图区域 -->
    <section id="slide">
    </section>
    <!-- 网站特色专题区域 -->
    <section id="featrue"></section>
    <!-- 立即预约区域 -->
    <section id="booking"></section>
    <!-- 产品推荐区域 -->
    <section id="products"></section>
    <!-- 全部新闻区域 -->
    <section id="news"></section>
    <!-- 合作伙伴区域 -->
    <footer id="partner"></footer>
  2. 写代码的约定
    网页的大容器结构一般会用id来约束 id是唯一
    id样式优先级高 如果要使用UI框架 使用id可以提高你样式的优先级 方便覆盖框架的样式
  3. 顶部的构建 使用版心布局容器 和 栅格系统
          <!-- 添加布局容器container -->
          <div class="container">
              <div class="row">
                  <div class="col-md-2 text-center">
                      <a href="#">手机微金所</a>
                  </div>
                  <div class="col-md-5 text-center">
                      <span>4006-89-4006（服务时间：9:00-21:00）</span>
                  </div>
                  <div class="col-md-2 text-center">
                      <a href="#">常见问题 </a>
                      <a href="#">财富登录</a>
                  </div>
                  <div class="col-md-3 text-center">
                      <!-- 红色按钮 小按钮-->
                      <button class="btn btn-danger btn-sm">免费注册</button>
                      <!-- 链接按钮 -->
                      <a href="#" class="btn btn-link">登录</a>
                  </div>
              </div>
          </div>
  4. 导航的构建 使用组件 》 导航条组件
  5. 轮播图构建 使用javascript插件 》  carousel 插件
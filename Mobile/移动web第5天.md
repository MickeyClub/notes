## 总结

1. 顶部搜索框渐变JS
2. 秒杀商品倒计时JS
3. 轮播图插件的使用
4. 分类页面的布局
5. 分类左侧的滑动效果



## 顶部搜索框背景透明度渐变

### 需求： 
    1. 顶部搜索框有一个背景色跟随滚动条滚动而变化背景色透明度
    2. 在滚动范围在轮播图高度范围内容进行渐变 超过了轮播图范围默认为1  最开始透明度是0
    3. 在滚动在轮播图高度范围内进行0-1透明度变化

### 实现思路
    1. 给滚动条添加滚动事件
    2. 在滚动条事件里面获取滚动的距离
    3. 获取轮播图的高度
    4. 判断滚动滚动距离是否小于轮播图高度
    5. 如果小于轮播图高度 计算当前要设置的透明度的值  距离/高度 * 1
    6. 如果滚动的距离大于轮播图高度 默认透明度为1
    7. 不管是计算的透明度还是默认1透明度 都要给搜索框的背景色设置



## 倒计时功能

### 需求：
    1. 需要有一个总的时间来倒计时
        总的时间 未来时间假设今天中午12点 - 当前的时间 当前11:17   转成秒数
    2. 定义定时器 每个1秒走一次 每秒 总时间(秒数)--
    3. 计算--完后的总时间的 时 分 秒
    4. 把时分秒的十位和个位显示到页面span里面

### 实现思路
    1. 定义一个总时间秒数 = 未来时间毫秒数 - 当前的时间毫秒数 / 1000 
    2. 定义一个定时器 每秒执行一次
        总时间--
    3. 分别计算总时间的时 分 秒
        时： 1 * 60 * 60  3600   3600/3600  Math.floor(总时间/3600)
        分:  1 * 60   总时间 / 60 % 60 
        秒数：        总时间 % 60
    4. 十位  Math.floor(时 / 10 )   个位  时 % 10



## 轮播图的功能

### 需求：
    1. 自动无缝轮播图
    2. 小圆点也跟着图片动
    3. 还要支持左右滑动

### 实现思路
    1. 使用轮播图插件swiper
    2. 实现自动无缝轮播图和左右滑动+小圆点

### 使用步骤

    1. 打开插件网站找需要的代码 https://www.swiper.com.cn/usage/index.html
    2. 根据官网代码学习如何使用
    3. 下载swiper https://pan.baidu.com/s/1sb2WnAU_E344nnDwBSwBLA 解压拷贝里面的dist目录到我们的lib文件夹里面 修改名称为swiper
    4. 创建一个页面 在页面中引入swiper插件的CSS和js
    5. 在页面中创建一个swiper需要标签结构
        <div class="swiper-container">
          <div class="swiper-wrapper">
              <div class="swiper-slide">Slide 1</div>
              <div class="swiper-slide">Slide 2</div>
              <div class="swiper-slide">Slide 3</div>
          </div>
          <!-- 如果需要分页器 -->
          <div class="swiper-pagination"></div>
          
          <!-- 如果需要导航按钮 -->
          <div class="swiper-button-prev"></div>
          <div class="swiper-button-next"></div>
          
          <!-- 如果需要滚动条 -->
          <div class="swiper-scrollbar"></div>
      </div>
    6. 需要给容器添加一个样式（宽高）
      .swiper-container {
          width: 600px;
          height: 300px;
      }
    7. 给轮播图初始化
        // 5. 调用JS的方法去初始化swiper插件
        // new Swiper 就是swiper插件的构造函数 这个构造函数需要传入2个参数第一个参数是轮播图的大容器的选择器
        //  第二个参数是轮播图配置项
        var mySwiper = new Swiper('.swiper-container', {
          //自动轮播图的参数
            autoplay: {
                //自动轮播图的间隔时间
                delay: 1000,
                //在手指滑动后是否要再次开启自动轮播图
                disableOnInteraction: false,
            },
            //无缝轮播图参数
            loop:true,
            //初始化小圆点 注意页面需要有这个容器
            pagination: {
              el: '.swiper-pagination',
           },
           //添加左右的箭头  注意页面需要有这个结构
           navigation: {
              nextEl: '.swiper-button-next',
              prevEl: '.swiper-button-prev',
           },
           //轮播图轮播的方向 默认水平horizontal 垂直vertical
           direction: 'horizontal',
           //添加滚动条 页面需要有这个结构
           // scrollbar: {
            //       el: '.swiper-scrollbar',
            //   }
            //添加轮播图效果 3D方块
            // effect: 'cube',
            // grabCursor: true,
            // cubeEffect: {
            //     shadow: true,
            //     slideShadows: true,
            //     shadowOffset: 20,
            //     shadowScale: 1,
            // },
            effect : 'fade',
            // effect : 'coverflow',
          // slidesPerView: 3,
          // centeredSlides: true,
          // effect : 'flip',
        });
      

### 轮播图初始化swiper的一些常见的API （配置项）

    1. 自动轮播图 调用轮播图初始化的时候传入一个autoplay属性
          autoplay: {
            //自动轮播图的间隔时间
            delay: 2500,
            //在手指滑动后是否要再次开启自动轮播图
            disableOnInteraction: false,
          },
    2. 无缝轮播图 调用初始化的时候传入一个 
           loop: true,
    3. 添加轮播图的小圆点
          pagination: {
            el: '.swiper-pagination',
          },
    4. 添加左右的箭头
         navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
          },

    5. 添加滚动的条
       scrollbar: {
          el: '.swiper-scrollbar',
       }

### 使用swiper 实现京东首页的轮播图

  1. 引入swiper
  2. 写页面结构
  3. 初始化


### 轮播图其他使用

    1. 效果 添加3d效果
    2. 添加点击链接
    3. 调用函数实现开始和暂停轮播图 https://www.swiper.com.cn/api/autoplay/startAutoplay.html 
    4. 鼠标移入暂停 离开就继续播放 https://www.imooc.com/wenda/detail/337775


## 实现分类页面全屏的布局

    1. 分类页面左侧分类 和右侧分类 左侧和右侧滑动使用插件来滑动 不是滚动条
    2. 整个页面是不能出现水平和垂直滚动条的
    3. 需要让html和body的宽高和屏幕的真实宽高一致
    4. 内容的高度也不能超过body宽高 main主体内容的高度和body的高度一样
    5. 当左右2边需要滑动的内容超过了屏幕范围要给父元素设置overflow:hidden;  给main添加overflow:hidden;
    6. 需要让内容不被头部和底部给盖住 给main添加上下padding


## 实现分类左右分类里面的内容的布局

    1. 左侧分类 容器高度是为总高度-头-底 的高度
    2. 左侧分类的内容ul的高度 由li撑开的 li的高度50*20     ul高度就是1000
    3. 给左侧分类的容器添加overflow:hidden;  


## 使用swiper内容滚动插件实现分类左侧右侧的滚动

    1. 引入swiper
    2. 写内容滚动的标签结构
        <!-- Swiper -->
        <div class="swiper-container">
            <div class="swiper-wrapper">
                  <ul class="swiper-slide">
                      <li class="active"><a href="#">热门推荐</a></li>
                      省略了很多li
                  </ul>
            </div>
            <!-- 添加滚动条 -->
            <div class="swiper-scrollbar"></div>
        </div>
    3. 写样式 给swiper轮播图的容器设置高度和屏幕一样高 
        html,
        body {
            position: relative;
            height: 100%;
        }
        .swiper-container {
            width: 100%;
            height: 100%;
        }
        <!-- 相当之前写ul让ul的高度是有内容撑开  注意一定要添加-->
        .swiper-slide{
            height:auto;
        }

    4. 初始化swiper
       var swiper = new Swiper('.swiper-container', {
            //垂直方向滑动
            direction: 'vertical',
            //支持多个子元素一起滑动
            slidesPerView: 'auto',
            // 一次性滑动多个子元素
            freeMode: true,
            //添加滚动条
            scrollbar: {
                el: '.swiper-scrollbar',
            },
            //支持鼠标滚轮
            mousewheel: true,
        });



# Wechat

[TOC]

#### pages

用于指定小程序由哪些页面组成，每一项都对应一个页面的 路径+文件名 信息。文件名不需要写文件后缀，框架会自动去寻找对于位置的 `.json`, `.js`, `.wxml`, `.wxss` 四个文件进行处理。 

#### 双向数据绑定

`Page.prototype.setData(Object data, Function callback)`

`setData` 函数用于将数据从逻辑层发送到视图层（异步），同时改变对应的 `this.data` 的值（同步）。 

html

```
<view bindtap='changeText'>{{info}}</view>
```

js

```
Page({
  /**
   * 页面的初始数据
   */
  data: {
    info: '我是一个快乐的div'
  },
  // 点击div改变文字
  changeText(){
    // this.info = '快乐你妹' // 这种写法无效,需要用setData方法
    this.setData({
      info: '快乐你妹啊'
    })
  },
})
```

#### require

作用: 载入其他的js但是官网**不支持相对路径**

#### 事件的使用方式

如`bindtap`，当用户点击该组件的时候会在该页面对应的Page中找到相应的事件处理函数。 

```
<view id="tapTest" data-hi="WeChat" bindtap="tapName"> Click me! </view>
```

```
Page({
  tapName: function(event) {
    console.log(event) 
  }
})
```



#### 跳转页面带参数 配合onLoad生命周期函数

https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html

list.wxml

```
<view wx:for="{{heroList}}">
  <navigator url='/pages/detail/detail?id={{item.id}}'>
    <image src='{{item.iconUrl}}'></image>
    <text>{{item.name}}</text>
  </navigator>
</view>
```

detail.js

```
Page({

  /**
   * 页面的初始数据
   */
  data: {
	id: ''
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    this.setData({
        this.id = options.id
    })
  },
```





#### 尺寸单位(rpx)

rpx单位是微信小程序中css的尺寸单位，rpx可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。 

| 设备     | rpx换算px (屏幕宽度/750) | px换算rpx (750/屏幕宽度) |
| -------- | ------------------------ | ------------------------ |
| iPhone5  | 1rpx = 0.42px            | 1px = 2.34rpx            |
| iPhone6  | 1rpx = 0.5px             | 1px = 2rpx               |
| iPhone6s | 1rpx = 0.552px           | 1px = 1.81rpx            |

**开发微信小程序时设计师可以用 iPhone6 作为视觉稿的标准。**

建议：设计稿使用设备宽度750px比较容易计算750px的话1rpx=1px，这样的话，设计图上量出来的尺寸是多少px就是多少rpx，至于在不同的设备上实际上要换算成多少个rem就交给小程序自己换算 



#### 关于小程序 scroll-view左右横向滑动没有效果(无法滑动)的问题

- scroll-view 中的需要滑动的元素不可以用 float 浮动；
- scroll-view 中的包裹需要滑动的元素的大盒子用 display:flex; 是没有作用的；

解决方案:

- scroll-view 中的需要滑动的元素要用 **dislay:inline-block;** 进行元素的横向编排；
- 包裹 scroll-view 的大盒子有明确的宽和加上样式-->  overflow:hidden;**white-space:nowrap;**

xwml

```
<text > scroll-view使用演示</text>
<!-- 横线滚动,内容要比父盒子宽 -->
  <scroll-view class="scroll-view_H" scroll-x style="width: 100%">
    <view id="green" class="scroll-view-item_H bc_green"></view>
    <view id="red"  class="scroll-view-item_H bc_red"></view>
    <view id="yellow" class="scroll-view-item_H bc_yellow"></view>
    <view id="blue" class="scroll-view-item_H bc_blue"></view>
  </scroll-view>
```

wxss

```
.scroll-view_H{
    height: 200rpx;
    white-space: nowrap;  // 这条属性很重要
}

/* 
    审查元素时 scroll-view的子元素就是我们写的那些
    但是使用>子代选不中
    微信小程序为了能够滚动 会加多一个滚动的View出来 但是审查元素看不到
*/

.scroll-view_H view{
    height: 100%;
    width: 50%;
    display: inline-block;  // 这条属性很重要
}

#green {
    background-color: green;
}

#red {
    background-color: red;
}

#yellow {
    background-color: yellow;
}

#blue {
    background-color: blue;
}
```



#### Nginx反向代理(代理服务器)

代理服务器: 指的是安装了某个软件的计算机

微信小程序没有跨域访问的概念

但是豆瓣接口限制了微信小程序的发送过来的请求,于是就用到反向代理解决访问受限问题

![1540450857620](E:\CodeSettle\notes\Wechat\assets\1540450857620.png)



#### 发起请求

文档: https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html

```
wx.request({
  url: 'test.php', //仅为示例，并非真实的接口地址
  data: {
    x: '',
    y: ''
  },
  header: {
    'content-type': 'application/json' // 默认值
  },
  success (res) {
    console.log(res.data)
  }
})
```



#### 五星评分

##### 评分处理(五星评分)

思路:

1. 往接口新增字段,用空数组存起来
2. 计算星星个数 向下取整  假设计算后是4颗星,默认5颗星
3. 给数组添加动态添加属性,有黄星=1,灰星=0  最终变成 [1,1,1,0]
4. 使用block wx:for 标签 相当于占位符,进行判断

```
// 处理星星评分数据(接口返回的)
result.data.subjects.forEach(v => {
  // 为V添加字段 纯数字无法循环 必须用数组存起来
  v.starArr = []
  // 计算星星个数
  let starNum = Math.floor(v.rating.stars / 10)
  // 遍历数组并未其赋值  黄星 = 1 / 灰星 = 0  
  // index 从 0开始 默认 5 个星星  0 1 2 3 4  假设星星为3个  
  for (let i = 0; i < 5; i++) {
    // 赋值给新的字段starArr             // 如果星星个数的4  
    v.starArr[i] = starNum > i ? 1 : 0; //最终会变成类似于 [1,1,1,1,0] 四颗星
  }
});
```

#### block wx:for

相当于占位符,渲染之后页面看不到

类似 `block wx:if`，也可以将 `wx:for` 用在`<block/>`标签上，以渲染一个包含多节点的结构块。例如：

#### 制定当前元素变量名

使用 `wx:for-item` 可以指定数组当前元素的变量名， 

使用 `wx:for-index` 可以指定数组当前下标的变量名：

```
<block wx:for="{{item.startArr}}" wx:for-item="itemName">
	<text wx:if="{{itemName==1}}" class="orange">★</text>
	<text wx:else class="gray">★</text>
</block>
```



#### 下拉刷新

文档: https://developers.weixin.qq.com/miniprogram/dev/api/ui/navigation-bar/wx.showNavigationBarLoading.html

```javascript
  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {
    console.log("下拉啦")
    // 页面显示导航条加载动画
    wx.showNavigationBarLoading()
    // 动态设置页面标题
    wx.setNavigationBarTitle({
      title: "加载中..."
    })
    // 延时 或者 请求数据完成
    setTimeout(() => {
      // 页面隐藏导航条加载动画
      wx.hideNavigationBarLoading()
      // 动态设置页面标题
      wx.setNavigationBarTitle({
        title: "加载完成"
      })
      // 结束下拉刷新
      wx.stopPullDownRefresh()
    }, 500);
  },
```

#### 页面上拉触底事件 

```
/**
 * 页面上拉触底事件的处理函数
 */
onReachBottom: function () {
  // 并判断,如果当前个数start大于总的total数,则返回
  if (this.data.startIndex > this.data.total) {
    wx.showToast({
      title: '已结到底啦~',
      icon: 'none',
      duration: 1000
    })
    return
  }

  // 提示用户加载中                                                          
  wx.showLoading({
    title: '加载中...'
  })

  // 旧的起始位置   请求新的页面 就必须要    当前的起始位置(startIndex) + 当前的个数(count) 去请求接口   最后请求后 将旧的数据和新的数据拼接起来
  let oldStart = this.data.startIndex;
  let newStart = oldStart + this.data.count;
  // 累加起始位置
  this.setData({
    startIndex: newStart
  })

  // 请求数据
  wx.request({
    url: 'https://douban.uieee.com/v2/movie/' + this.data.tgas,
    // 请求参数
    data: {
      start: this.data.startIndex,
      count: this.data.count
    },
    header: {
      'content-type': 'json'
    },
    method: 'GET',
    dataType: 'json',
    responseType: 'text',
    success: (result) => {
      // console.log(result);
      // 处理星星评分数据
      result.data.subjects.forEach(v => {
        // 为V添加字段 纯数字无法循环 必须用数组存起来
        v.starArr = []
        // 计算星星个数
        let starNum = Math.floor(v.rating.stars / 10)
        // 遍历数组并未其赋值  黄星 = 1 / 灰星 = 0  
        // index 从 0开始 默认 5 个星星  0 1 2 3 4  假设星星为3个  
        for (let i = 0; i < 5; i++) {
          // 赋值给新的字段starArr             // 如果星星个数的4  
          v.starArr[i] = starNum > i ? 1 : 0; //最终会变成类似于 [1,1,1,1,0] 四颗星
        }
      });

      // 新旧数据拼接
      let oldMovie = this.data.movieList;
      oldMovie = oldMovie.concat(result.data.subjects)
      // 即将上映
      this.setData({
        // 记录电影数据
        movieList: oldMovie,
        // 记录总条数
        total: result.data.total
      })

      // 隐藏加载动画
      wx.hideLoading()
    }
  })
},
```



#### 小程序发布需要配置和校验合法域名

1. 微信小程序开发者工具, 点击右上角详情 默认不校验合法域名、web-view（业务域名）、TLS 版本以及 HTTPS 证书 
2. 点击上传
3. 配置合法域名
4. 发版

#### 封装promise的request请求

```
// tools.js
// 请求数据  promise异步编程请求数据 解决地狱回调,使用链式编程
function myPro(options) {
    return new Promise(function (resolve, reject) {
        // 请求数据
        wx.request({
            url: options.url || '',
            data: options.data || {},
            header: options.header || {
                'content-type': 'json'
            },
            method: options.method || 'GET',
            dataType: options.dataType || 'json',
            responseType: options.responseType || 'text',
            success: (result) => {
                // 成功回调
                resolve(result)
            },
            fail: () => {
                // 失败回调
                reject("请求数据失败")
            }
        })
    })
}
// 暴露
module.exports = {
    // es6的快速赋值 等同于 myPro:myPro
    myPro
}
```

#### promise的all

promise异步请求,将回调里函数变成同步,这时候为了提高用户体验可以一次性请求

```
// 使用Promise 一次性调用 多个 promise对象 获取他们的结果
// 创建所有需要一部执行的 promise对象
let p1 = tool.myPro({
  url: 'https://douban.uieee.com/v2/movie/in_theaters',
});
let p2 = tool.myPro({
  url: 'https://douban.uieee.com/v2/movie/coming_soon'
})
let p3 = tool.myPro({
  url: 'https://douban.uieee.com/v2/movie/top250'
})
Promise.all([p1, p2, p3]).then(resultList => {
  // console.log(result);
  // 绑定数据
  // 数据1
  resultList[0].data.subjects.forEach(v => {
    // 使用抽取的星星算法
    v.starArr = tool.makeStar(v.rating.average);
  })
  this.setData({
    hotMovie: resultList[0].data.subjects
  })
  // 数据2 
  resultList[1].data.subjects.forEach(v => {
    // 使用抽取的星星算法
    v.starArr = tool.makeStar(v.rating.average);
  })
  this.setData({
    comingMovie: resultList[1].data.subjects
  })
  // 数据3
  resultList[2].data.subjects.forEach(v => {
    // 使用抽取的星星算法
    v.starArr = tool.makeStar(v.rating.average);
  })
  this.setData({
    goodMovie: resultList[2].data.subjects
  })
})
```



#### 模板使用

注意 引入时的标签是单标签  / 

使用

```
// 引入
<import src="../../template/item.wxml"/>
// 使用 注意 is要和模板的name对应上
<template is="item" data="{{item}}"/>
```

模板

```
<template name="item">
    <view class="item">
	我是模板
    </view>
</template>
```



#### 小程序自带的audio不支持修改样式

需要自己写音频

![1540624445975](E:\CodeSettle\notes\Wechat\assets\1540624445975.png)



####  代码的方式跳转 编程式导航 

```
wx.navigateTo({
	url: '/pages/search/search',
	success: (result)=>{

},
    fail: ()=>{},
    complete: ()=>{}
});
```

#### swiper轮播图

```
<swiper indicator-dots autoplay circular>
	<swiper-item>
		<img src="" alt="">
	</swiper-item>
</swiper>
```

#### getStorage

![1540887590606](E:\CodeSettle\notes\Wechat\assets\1540887590606.png)'

### mpvue

绑定事件(bindxxx)无法在mpvue中使用

需要去mpvue文档查看
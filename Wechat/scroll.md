

# Wechat



[TOC]

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

##### block wx:for

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

#### 

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




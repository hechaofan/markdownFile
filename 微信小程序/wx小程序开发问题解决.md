# wx小程序开发问题解决

## 去除滚动条

```javascript
::-webkit-scrollbar {
    width: 0;
    height: 0;
    color: transparent;
}
```



## wx:if 和hidden

```javascript
wx:if 配合block
hidden 配合view
```



## 动态的class

```javascript
<view class="goodName {{currentTabsIndex == index ?  'goodActive': ''}}">
    {{item}}
</view>
```



## 传递index

```javascript
<view bindtap="onTabsItemTap" data-index="{{index}}">{{item}}</view>
onTabsItemTap(event) {
      const {index} = event.currentTarget.dataset
      this.setData({
          currentTabsIndex:index
      })
  }
```



## 解决微信背景图

```javascript
<view class="orderMeal">
    <scroll-view class="orderMeal">
        <view class="orderMeal">
            <view class="orderMeal">
                <image src="/assets/img/orderMeal/orderMeal-bj.jpg" class="orderMeal"></image>
            </view>
        </view>
    </scroll-view>
</view>

page{
    height: 100%;
}
.orderMeal{
    width: 100%;
    height: 100%;
}
::-webkit-scrollbar {
    width: 0;
    height: 0;
    color: transparent;
}
```



## 获取屏幕宽高

   方法1：

```
imageLoad: function () {
   this.setData({
     imageWidth: wx.getSystemInfoSync().windowWidth
   })
 }
```

方法2

```
.imgClass{
 width: 100vw;
}
```

CSS3引入的”vw”和”vh”基于宽度/高度相对于窗口大小 

”vw”=”view width”“vh”=”view height” 

以上我们称为视窗单位允许我们更接近浏览器窗口来定义大小。

```
.demo {
  width: 100vw;
  font-size: 10vw; /* 宽度为窗口100%， 字体大小为窗口的10% */
}
.demo1 {
  width: 80vw;
  font-size: 8vw; /* 宽度为窗口80%， 字体大小为窗口的8% */
}
.demo2 {
  width: 50vw;
  font-size: 5vw; /* 宽度为窗口50%， 字体大小为窗口的5% */
}
.demo3 {
  width: 10vw;
  height: 50vh; /* 宽度为窗口10%， 容器高度为窗口的50% */}
```



多个页面需要屏幕高度可以定义在app.js中

```
//app.js
 onLaunch: function () {
    wx.getSystemInfo({
      success: function (res) {
        that.globalData.windowHeight = res.windowHeight
      }
    })
  }
  ,
  globalData: {
    windowHeight:null
  }
  
//index.js
const app = getApp()
onReady:function(){
    var tbodyHeight = app.globalData.windowHeight - 90; //90为头部固定高度 
    that.setData({
        tbodyHeight: tbodyHeight.toFixed(0)
    })
 }
```



## 设置view动态的高度

我们很多时候有这样一个需求：我们的页面分为上下两个部分或者更多，上半部分或用了**固定的rpx**设置了高度，我们想要剩下一部分的高度刚好占满剩余窗口的部分。 
代码如下：

```
//index.js
Page({
  data: {
    second_height:0
  },
  onLoad: function () {
    console.log('onLoad')
    var that = this
    // 获取系统信息
    wx.getSystemInfo({
      success: function (res) {
        console.log(res);
        // 可使用窗口宽度、高度
        console.log('height=' + res.windowHeight);
        console.log('width=' + res.windowWidth);
        // 计算主体部分高度,单位为px
        that.setData({
          // second部分高度 = 利用窗口可使用高度 - first部分高度（这里的高度单位为px，所有利用比例将300rpx转换为px）
          second_height: res.windowHeight - res.windowWidth / 750 * 300
        })
      }
    })
  }
})
<!--index.wxml-->
<view class="class_first">
  第一部分内容，高度是固定的rpx
</view>
<view class="class_second" style="height:{{second_height}}px">
  第二部分内容，高度为窗口剩余部分的高度
</view>1234567
/**index.wxss**/
.class_first{
  background-color: #666666;
  color: #fff;
  /* 高度固定300rpx */
  height: 300rpx; 
}

.class_second{
  background: #e5e5e5;
  color: #000;
}
```



## 获取view等组件的高度等信息（获取节点信息）

   js

```
//创建节点选择器
    var query = wx.createSelectorQuery();
    query.select('.list').boundingClientRect()
    query.exec((res) => {
      var listHeight = res[0].height; // 获取list高度
   })
```

res中会有这个节点的信息



**tips:**

query.exec((res) => {})中的回调函数是最后执行的，若要获取高度等信息进行操作的话，要在回调函数中进行。



## 获取用户信息

wx.getUserInfo(OBJECT)

```javascript
<!-- 如果只是展示用户头像昵称，可以使用 <open-data /> 组件 -->
<open-data type="userAvatarUrl"></open-data>
<open-data type="userNickName"></open-data>
<!-- 需要使用 button 来授权登录 -->
<button wx:if="{{canIUse}}" open-type="getUserInfo" bindgetuserinfo="bindGetUserInfo">授权登录</button>
<view wx:else>请升级微信版本</view>
```

```javascript
//js
Page({
  data: {
    canIUse: wx.canIUse('button.open-type.getUserInfo')
  },
  onLoad: function() {
    // 查看是否授权
    wx.getSetting({
      success: function(res){
        if (res.authSetting['scope.userInfo']) {
          // 已经授权，可以直接调用 getUserInfo 获取头像昵称
          wx.getUserInfo({
            success: function(res) {
              console.log(res.userInfo)
            }
          })
        }
      }
    })
  },
  bindGetUserInfo: function(e) {
    console.log(e.detail.userInfo)
  }
})
```





## 微信小程序中获取高度及设备的方法

https://www.cnblogs.com/xiaoyaoxingchen/p/8819880.html

由于js中可以采用操纵dom的方法来获取页面元素的高度，可是在微信小程序中不能操纵dom，经过查找之后发现仅仅只有以下几个方法可以获取到高度

```
wx.getSystemInfoSync().windowWidth   // 获取当前窗口的宽度
wx.getSystemInfoSync().windowHeight    // 获取当前窗口的高度
wx.getSystemInfoSync().model    // 获取当前采用的设备
wx.getSystemInfoSync().pixelRatio   
wx.getSystemInfoSync().language   // 获取当前所采用的的语言
wx.getSystemInfoSync().version    // 获取当前设备的版本
```

 

获取view等组件的高度等信息（获取节点信息）

```
//创建节点选择器
  var query = wx.createSelectorQuery();
    query.select('.list').boundingClientRect()；
    query.selectViewport().scrollOffset()；
    query.exec((res) => {
      var listHeight = res[0].height; // 获取list高度
  })；
```

res内容：

![img](https://img2018.cnblogs.com/blog/1281517/201812/1281517-20181207103416777-1197191420.png)

**tips:**

　　1、res[0]为当前节点的详细数据`；`

　　2、res[1]为`显示区域的竖直滚动位置；`

　　3、query.exec((res) => {})中的回调函数是最后执行的，若要获取高度等信息进行操作的话，要在回调函数中进行。





## 如何在微信小程序里引用npm包？

https://www.cnblogs.com/mmit/p/12590895.html
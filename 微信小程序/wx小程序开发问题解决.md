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


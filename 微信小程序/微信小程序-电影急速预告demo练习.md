# 微信小程序-电影急速预告demo练习

视频作者：github Maxlasting  项目源码 wx-douban-trailer  后端movie-trailer-api 找到dev分支

## api地址

https://db.miaov.com/doubanapi/v0/movie/list



## 服务器域名配置

![](C:\Users\15996\Desktop\ImageGithubToMarkdown\2018-07-24_162935.png)



## 跳转详情页传递id

```javascript
//index.wxml
<view bindtap="gotoDetail" data-id="{{item._id}}"></view>
//index.js
gotoDetail(e) {
      console.log(e.currentTarget.dataset.id);
      const {id}= e.currentTarget.dataset
      wx.navigateTo({
          url: '../movie-detail/movie-detail?id=' + id
      })
  }
  
//movie-detail.js
onLoad: function (options) {
      console.log(options.id);
  }
```


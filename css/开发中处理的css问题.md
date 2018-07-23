# 开发中处理的css问题



## 1.background(背景图)



###1.1背景图铺满背景

```
background-image url("../../img/youpchomepage/home-part1-1.png")
background-repeat no-repeat
background-size cover //铺满
background-position: center 0px //以中心定位
```



## 2.overflow和滚动条的问题

1.属性

当父元素,子元素的高宽确定时,

​            overflow: auto;     父<子,出滚动条

​            overflow: scroll;    不管 父是不是<子,都出滚动条

​            overflow: hidden;    子溢出的都会被截掉

2.overflow作用于谁上面

考虑 document html body;

​    1.html body中只有一个有overflow属性时,这个属性会传给document

​    2.html body都存在overflow属性时.body的overflow属性才能生效,

​    3.当html body都存在overflow属性时.body的overflow属性一直都会作用于body身上,

​                                  html的overflow属性一直都会作用于document身上

3.禁止系统滚动条

```
html,body{
    height: 100%;
    overflow: hidden;
}
```

4.隐藏滚动条

```
::-webkit-scrollbar {/隐藏滚动条/
	display: none;
}
```

```javascript
//stylus中
.visitSearchdiv
width 100%
height 100%
overflow auto
background-color #ffffff
&::-webkit-scrollbar
display: none
```



## 3.input(样式设置)

```javascript
border-radius:8px; //设置圆角
border: 1px solid #DBDBDB; //去除阴影
outline:none //消除聚焦时的外边框
background-image: url(../../assets/workdiary/riqi.png);/*设置小图标*/
background-size:15px 15px;/*小图标的大小*/
background-position: 10px 6px;/*小图标在input的位置*/
background-repeat: no-repeat;/*背景小图标不重复*/
padding: 8px 10px 8px 40px;/*设置input内边距*/
```

 设置placeholder提示字体的大小和颜色还有边距

```javascript
placeholder="筛选日志发布时间"
```

```javascript
text-indent 32px //更改input中文字到左边距离
background-color #eff0f4 //input 背景色
```



## 4.变手指

```
cursor: pointer;
```


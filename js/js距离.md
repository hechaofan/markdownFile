```javascript
//todo 最完整版本
//todo jquery <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>

//todo 鼠标
clientX //鼠标指针到浏览器页面的水平坐标(左)

clientY //鼠标指针到浏览器页面的垂直坐标(上)

offsetX //设置或获取鼠标指针位置相对于触发事件的对象的 x 坐标。

offsetY //设置或获取鼠标指针位置相对于触发事件的对象的 y 坐标。

pageX //当前鼠标到文档水平的距离 (左)

pageY //当前鼠标到文档顶部的距离 (上)

//todo 元素
offsetLeft  //元素距离父元素左边距离

offsetTop    //元素距离父元素右边距离

clientHeight //元素的高度（包括边框）

offsetHeight //元素的高度（不包括边框）

box1.getBoundingClientRect().top//元素到屏幕的距离

node.scrollTop - - node.scrollLeft         //元素内容上滚的高度      //所有主流浏览器都兼容

node.scrollWidth - - node.scrollHeight     //表示元素的内容区的高度和宽度,如果元素没有设置overflow:hidden;那么,此处的宽高包含了溢出以后的宽高



//todo 屏幕
document.documentElement.clientWidth //浏览器窗口的大小（最干净的视口）

window.innerWidth  //浏览器窗口的大小,包括滚动条

window.outerWidth  //浏览器窗口的大小,包括镶边

screen.width  //屏幕宽度

//todo  jquery
offset() //方法返回或设置匹配元素相对于文档的偏移(位置)。

offset().top //元素距离文档的top值

offset().left //元素距离文档的left值

var scroll = $(document).scrollTop(); //滚动高度
//todo 要在滚动事件中
$(window).scroll(function () {
    var scroll = $(document).scrollTop(); //滚动高度
    console.log(scroll)
  })
```
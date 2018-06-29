# DOM

* DOM的全称 Document Object Model，译为文档对象模型

   在 HTML DOM （文档对象模型）中，每个部分都是节点：

   - 文档本身是文档节点
   - 所有 HTML 元素是元素节点
   - 所有 HTML 属性是属性节点
   - HTML 元素内的文本是文本节点
   - 注释是注释节点

# 元素和节点的区别

* 通过**节点**访问或操作 HTML 页面内容:
   - 元素节点: 表示 HTML 页面中的标签。
   - 属性节点: 表示 HTML 页面中标签的属性。
   - 文本节点: 表示 HTML 页面中标签的文本内容。
* 通过**元素**访问或操作 HTML 页面内容:
   - 元素: 表示 HTML 页面中的标签。

也就是说，使用**节点**方式时，标签、属性和文本是平行关系；而使用**元素**方式时，只有标签，属性和文本成为了标签的一部分。

元素一定是节点，节点不一定是元素

##**节点树**

![1529645737089](C:\Users\何超凡\AppData\Local\Temp\1529645737089.png)

## **元素树**

![img](file:///E:/%E6%96%87%E4%BB%B6/%E8%A7%86%E9%A2%91/%E9%87%91%E8%80%81%E5%B8%88%E7%9A%84%E6%AF%8F%E4%B8%80%E5%A4%A9/day19/DAY19/NOTE/img/dom_element_tree.png) 

# Document(文档)对象

每个载入浏览器的 HTML 文档都会成为 Document 对象。

Document 对象使我们可以从脚本中对 HTML 页面中的所有元素进行访问。

## 属性

all
	获取当前HTML页面中的所有元素，返回数组
documentElement
	获取当前HTML页面的<html>元素
head
	获取当前HTML页面的<head>元素
body
	获取当前HTML页面的<body>元素

## 方法

### 获取节点

常用方法
	getElementById 			通过元素ID属性值获取节点
	getElementsByName 		通过元素NAME属性值获取节点，返回数组
	getElementsByTagName	通过元素名获取节点，返回数组
新增方法
	getElementsByClassName	通过元素的CLASS属性值获取节点，返回数组
使用选择器
	querySelector			通过CSS选择器获取页面元素
	querySelectorAll			通过CSS选择器获取页面符合该选择器的所有元素，返回数组

```javascript
var content = document.querySelector('.content')
//通过id查找
var div = document.getElementById('div')
```

### 创造节点

* 元素节点 - createElement(标签名)
* 文本节点 - createTextNode(文本内容)
* 属性节点(了解) - 并不是子节点
  * 创建属性节点 - createAttribute(属性名)
  * 为属性节点设置值 - attrNode.nodeValue = 值
  * 将属性节点添加到元素节点 - element.setAttributeNode(attrNode)
* 特殊属性(了解)
       * head属性 - 获取到页面中的<head>标签 document.head
       * body属性 - 获取到页面中的<body>标签 document.head
   * documentElement属性 - 获取到页面中的<html>标签 document.documentElement

```javascript
  //创建元素节点
  var li = document.createElement('li')
  //创建文本节点
  var text = document.createTextNode('text')
  //设置文本节点的值
  text.nodeValue= '这是一个li'
  //将文本节点添加到元素节点中
  li.appendChild(text)
```



# node(节点)对象

## 节点关系

 * 三种关系
     * 祖先与后代的关系
       * 祖先节点 - 父节点的父节点的父节点...
       * 后代节点 - 子节点的子节点的子节点...
     * 父与子的关系
       * 父节点
       * 子节点
     * 兄弟的关系(同一个父节点)

## 判断节点类型，节点名称，节点值

* 判断节点类型
   * nodeName - 节点名称
     * 文档节点 -> #document
     * 元素节点 -> 标签名
     * 属性节点 -> 属性名
     * 文本节点 -> #text
   * nodeType - 节点类型
     * 文档节点 -> 9
     * 元素节点 -> 1
     * 属性节点 -> 2
     * 文本节点 -> 3
   * nodeValue - 节点值
     * 文档节点 -> null
     * 元素节点 -> null
     * 属性节点 -> 属性值
     * 文本节点 -> 文本内容



## 创建节点

- 元素节点 - createElement(标签名)
- 文本节点 - createTextNode(文本内容)
- 属性节点(了解) - 并不是子节点
  - 创建属性节点 - createAttribute(属性名)
  - 为属性节点设置值 - attrNode.nodeValue = 值
  - 将属性节点添加到元素节点 - element.setAttributeNode(attrNode)
- 特殊属性(了解)
      * head属性 - 获取到页面中的<head>标签 document.head
      * body属性 - 获取到页面中的<body>标签 document.head
  - documentElement属性 - 获取到页面中的<html>标签 document.documentElement

```javascript
  //创建元素节点
  var li = document.createElement('li')
  //创建文本节点
  var text = document.createTextNode('text')
  //设置文本节点的值
  text.nodeValue= '这是一个li'
  //将文本节点添加到元素节点中
  li.appendChild(text)
```



## 插入，删，改节点

 * 插入节点
     * parent.appendChild(child)方法
       * 将 child 插入到 parent 节点的所有子节点列表的最后
     * parent.insertBefore(newChild,oldChild)
       * 将 newChild 插入到 parent 节点中子节点为 oldChild 的前面

[element.replaceChild()](http://www.w3school.com.cn/jsref/met_node_replacechild.asp) 

[element.removeChild()](http://www.w3school.com.cn/jsref/met_node_removechild.asp) 

## 遍历节点

 * 遍历节点
     * 父节点 - parentNode
       * parentNode与parentElement的区别
         * parentNode - Node对象的属性
           * 如果当前标签为<html>的话，parentNode是document对象
         * parentElement - Element对象的属性
           * 如果当前标签为<html>的话，parentElement是null值
     * 子节点
       * firstChild - 第一个子节点
       * lastChild - 最后一个子节点
       * childNodes - 所有子节点的列表(NodeList)
     * 兄弟节点
       * previousSibling - 前一个兄弟节点
       * nextSibling - 后一个兄弟节点
     * 空白节点
       * 原因 - HTML代码编写时换行符产生的
       * 实际 - 空白的文本节点
       * 场景
         * 当从父节点获取子节点时
         * 当目标节点获取兄弟节点时

# element(元素)对象

   * ##概念

     * 节点与元素的区别
       * 相同点 - (目的)获取或操作HTML页面中的内容
       * 不同点
         * 节点 - 元素节点、属性节点和文本节点 -> DOM节点树结构
         * 元素 - 元素、元素的属性和元素的文本 -> DOM元素树结构

   * ##遍历元素

     * 父元素 - parentElement
     * 子元素
       * firstElementChild - 第一个子元素
       * lastElementChild - 最后一个子元素
       * children - 所有子元素列表
     * 兄弟元素
       * previousElementSibling - 前一个兄弟元素
       * nextElementSibling - 后一个兄弟元素
     * 注意
       * 并没有类似于遍历节点中的空白节点问题

       * 并不是所有浏览器都支持元素的遍历属性

         

   * ##节点操作

     [element.replaceChild()](http://www.w3school.com.cn/jsref/met_node_replacechild.asp) 

     [element.removeChild()](http://www.w3school.com.cn/jsref/met_node_removechild.asp) 

   * ##属性操作

     * 获取属性 -[element.getAttribute()](http://www.w3school.com.cn/jsref/met_element_getattribute.asp) 

     * 设置属性 - [element.setAttribute()](http://www.w3school.com.cn/jsref/met_element_setattribute.asp) 

     * 删除属性 - [element.removeAttribute()](http://www.w3school.com.cn/jsref/met_element_removeattribute.asp) 

     * 判断属性 - [element.hasAttribute()](http://www.w3school.com.cn/jsref/met_element_hasattribute.asp)  判断是否有指定属性

       ​		 [element.hasAttributes()](http://www.w3school.com.cn/jsref/met_node_hasattributes.asp) 判断是否有属性

   * 文本操作
     * nodeValue属性 - 获取和设置文本内容
     * innerText属性和textContent属性 - 获取和设置文本内容
       * innerText属性 - 只在 IE 浏览器中支持
       * textContent属性 - 是在其他浏览器中支持
         * 注意 - IE 8以下版本不支持(结果为 undefined)

# HTML操作 - innerHTML属性

* 作用 - 获取和设置指定标签中的HTML代码
   * 注意 - 安全性不高
   * 不建议 - 不要使用 innerHTML 属性接收用户输入的内容

# CSS 操作

* 标签中的 style 属性 - 获取和设置内联样式
  * 直接操作 CSS 中的样式属性
  * 只能操作 style 属性自己的样式内容(其他的样式内容无法操作)
* 标签中的 class 属性 - 获取和设置内联样式
  标签.className 属性 -> 与保留字 class 同名
  * 通过操作 CSS 的类选择器
* 操作方式
  * 标签.style属性
    * 这种方式获取或设置属性 - 仅限于通用属性
  * 通过属性获取或设置 - 更通用
    * 标签.getAttribute()
    * 标签.setAttribute()

# 总结

##**获取dom**

常用方法
	getElementById 			通过元素ID属性值获取节点
	getElementsByName 		通过元素NAME属性值获取节点，返回数组
	getElementsByTagName	通过元素名获取节点，返回数组
新增方法
	getElementsByClassName	通过元素的CLASS属性值获取节点，返回数组
使用选择器
	querySelector			通过CSS选择器获取页面元素
	querySelectorAll			通过CSS选择器获取页面符合该选择器的所有元素，返回数组

```javascript
var content = document.querySelector('.content')
//通过id查找
var div = document.getElementById('div')
```

##创建元素节点，删除，替换，插入元素节点

document.createElement('li')

[element.removeChild()](http://www.w3school.com.cn/jsref/met_node_removechild.asp) 

[element.replaceChild()](http://www.w3school.com.cn/jsref/met_node_replacechild.asp) 

parent.appendChild(child)

parent.insertBefore(newChild,oldChild)

##元素节点属性操作

- 获取属性 -[element.getAttribute()](http://www.w3school.com.cn/jsref/met_element_getattribute.asp) 

- 设置属性 - [element.setAttribute()](http://www.w3school.com.cn/jsref/met_element_setattribute.asp) 

- 删除属性 - [element.removeAttribute()](http://www.w3school.com.cn/jsref/met_element_removeattribute.asp) 

- 判断属性 - [element.hasAttribute()](http://www.w3school.com.cn/jsref/met_element_hasattribute.asp)  判断是否有指定属性

  ​		 [element.hasAttributes()](http://www.w3school.com.cn/jsref/met_node_hasattributes.asp) 判断是否有属性

##文本操作

nodeValue属性 - 获取和设置文本内容

```javascript
//太复杂
//创建文本节点
var text =document.createTextNode('text') ;
//设置文本内容
text.nodeValue = '我是你爸爸'
//添加到元素节点中
li.appendChild(text)
```

- innerText属性和textContent属性 - 获取和设置文本内容

  ```javascript
    //获取li的文本内容
    var li = document.querySelector('li')
    console.log(li.innerText);
    //设置div的文本内容
    var div = document.querySelector('div')
    div.innerText='hahaha'
  ```

  

## 遍历元素

- 父元素 - parentElement
- 子元素
  - firstElementChild - 第一个子元素
  - lastElementChild - 最后一个子元素
  - children - 所有子元素列表
- 兄弟元素
  - previousElementSibling - 前一个兄弟元素
  - nextElementSibling - 后一个兄弟元素
- 注意
  - 并没有类似于遍历节点中的空白节点问题
  - 并不是所有浏览器都支持元素的遍历属性

##修改样式的方法

 注意css的层级

```javascript
<style>
    #wrap{
      width: 100px;
      height: 100px;
    }
    .border{
      border: 3px solid green;
    }
	.f-r{
      float: right;
    }
</style>

<body>
<div id="wrap"></div>
</body>

<script>
  var wrap = document.querySelector('#wrap')
//通过style
  wrap.style.backgroundColor = 'red'
//通过className
  wrap.className = 'border'
//通过element.setAttribute() 
  wrap.setAttribute('class','f-r')
</script>
```


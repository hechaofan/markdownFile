# JavaScript：面试频繁出现的几个易错点

作业：[守候i](https://juejin.im/user/596d953ff265da6c2915c513)  链接：[JavaScript：面试频繁出现的几个易错点](https://juejin.im/post/5ab4ab126fb9a028d4448365)



# 面向对象编程

> 关于面向对象和面向过程，个人觉得这两者不是绝对独立的，而是相互相成的关系。至于什么时候用面向对象，什么时候用面向过程，具体情况，具体分析。 

针对于面向对象编程的。知乎上有一个高赞回答：

面向对象： 狗.吃(屎)
面向过程： 吃.(狗,屎)

但是这个例子觉得不太优雅，我改一下了，举一个优雅些的小例子说明一下面向对象和面向过程的区别。

需求：定义 “超凡吃火锅”

面向对象的思想是：超凡.动作（吃火锅）

面向过程的思想是：动作（超凡，吃火锅）

代码实现：

```javascript
//面向对象
//定义人（姓名）
let People = function (name) {
    this.name = name
}
//动作
People.prototype = {
    eat: function (something) {
        console.log((`${this.name}吃${something}`));
    }
}
//超凡是个人，所以新建一个实例对象
let chaofan = new People('chaofan')
chaofan.eat('火锅')

//面向过程
let eat = function (who,something) {
    console.log((`${who}吃${something}`));
}
eat('chaofan','火锅')
```

结果都一样，都是输出‘chaofan吃火锅’。但是万一我现在吃饱了，准备写代码了。这下怎么实现呢？看代码 

```javascript
//面向对象
chaofan.coding = function () {
   console.log(this.name + '写代码')
}
chaofan.coding()
//面向过程
let coding = function (who) {
    console.log(who + '写代码')
}
coding('chaofan')
```

结果也一样：‘chaofan写代码’

但是不难发现面向对象更加的灵活，复用性和扩展性更加。因为面向对象就是针对对象（例子中的：‘chaofan’）来进行执行某些动作。这些动作可以自定义扩展。
而面向过程是定义很多的动作，来指定谁来执行这个动作。

好了，面向对象的简单说明就到这里了，至于面向对象的三大特性：继承，封装，多态这个自行上网查找资料。



# this



 

 

 

 
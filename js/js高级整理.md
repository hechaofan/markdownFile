# js高级整理

## 1.函数的3种定义方法

### 1.1 函数声明

```javascript
//ES5
function getSum(){}
function (){}//匿名函数
//ES6
()=>{}//如果{}内容只有一行{}和return关键字可省,
```

### 1.2 函数表达式(函数字面量)

```javascript
//ES5
var sum=function(){}
//ES6
let sum=()=>{}//如果{}内容只有一行{}和return关键字可省,
```

### 1.3 构造函数

```javascript
var sum=new GetSum(num1,num2)
```

### 1.4 三种方法的对比

1.函数声明有预解析,而且函数声明的优先级高于变量; 

2.使用Function构造函数定义函数的方式是一个函数表达式,这种方式会导致解析两次代码，影响性能。第一次解析常规的JavaScript代码，第二次解析传入构造函数的字符串 

## 2.ES5中this的指向

在ES5中函数内容的this指向和调用方法有关

### 2.1独立调用 

包括函数名()和匿名函数调用,this指向window 

```javascript
 function getSum() {
    console.log(this) //window
 }
 getSum()
 
 (function() {
    console.log(this) //window
 })()
 
 var getSum=function() {
    console.log(this) //window
 }
 getSum()
```



### 2.2 隐式调用

####2.2.1对象.方法名(),this指向对象 

```javascript
var objList = {
   name: 'methods',
   getSum: function() {
     console.log(this) //objList对象
   }
}
objList.getSum()
```

####2.2.2隐式丢失：发生引用传递时会有隐式丢失的情况

​            引用赋值（给一个变量）
            参数传递

####2.2.3.隐式丢失解决方案

​            使用硬绑定（一种显示绑定的形式）

### 2.3 构造器调用

new 构造函数名(),this指向构造函数

```javascript
function Person() {
  console.log(this); //指向构造函数Person
}
var personOne = new Person();
```

### 2.4 硬绑定

利用call和apply，bind来实现,this就是call和apply对应的第一个参数,如果不传值或者第一个值为null,undefined时this指向window

```javascript
function foo() {
   console.log(this);
}
foo.apply('我是apply改变的this值');//我是apply改变的this值
foo.call('我是call改变的this值');//我是call改变的this值
```

## 3.ES6中函数的调用（this的指向） 

```
 箭头函数的特点：
    1、简洁
    2、箭头函数没有自己的this，箭头函数的this不是调用的时候决定的，而是在定义的时候处在的对象就是它的this
   3、扩展理解： 箭头函数的this看外层的是否有函数，
        如果有，外层函数的this就是内部箭头函数的this，
        如果没有，则this是window。
```

 

```javascript
 //箭头函数
   let btn2 = document.getElementById('btn2');
   let obj = {
        name : 'kobe',
        age : 39,
        getName : function ()  {
            btn2.onclick = () => {
                console.log(this);//obj
            };
        }
    };
    obj.getName();


 function Person() {
     this.obj = {
         showThis : () => {
             console.log(this);

         }
     }
 }
    let fun5 = new Person();
    fun5.obj.showThis(); //Person
```

 

 ## 4.call,apply和bind

1. Function.prototype.bind(obj) :
  * 作用: 将函数内的this绑定为obj, 并将函数返回
2.  区别bind()与call()和apply()?
  * 都能指定函数中的this

  * bind()是将函数返回

  * call()/apply()是立即调用函数

  * call和apply传参方式不一样

    对象.call(新this对象,实参1,实参2,实参3.....) 

    对象.apply(新this对象,[实参1,实参2,实参3.....]) 

3.使用方法：

​    fun.call(obj, **12**)
    fun.apply(obj, **[12]**)
    fun.bind(obj, 12)**()**;

1.IE5之前不支持call和apply,bind是ES5出来的;
 2.call和apply可以调用函数,改变this,实现继承和借用别的对象的方法;

### 4.1 call和apply定义

调用方法,用一个对象替换掉另一个对象(this)
 对象.call(新this对象,实参1,实参2,实参3.....)
 对象.apply(新this对象,[实参1,实参2,实参3.....])

### 4.2 call和apply用法

1.间接调用函数,改变作用域的this值
 2.劫持其他对象的方法

```javascript
var foo = {
  name:"张三",
  logName:function(){
    console.log(this.name);
  }
}
var bar={
  name:"李四"
};
foo.logName.call(bar);//李四
实质是call改变了foo的this指向为bar,并调用该函数
```

3.两个函数实现继承

```javascript
function Animal(name){   
  this.name = name;   
  this.showName = function(){   
    console.log(this.name);   
  }   
}   
function Cat(name){  
  Animal.call(this, name);  
}    
var cat = new Cat("Black Cat");   
cat.showName(); //Black Cat
```

4.为类数组(arguments和nodeList)添加数组方法push,pop

```javascript
(function(){
  Array.prototype.push.call(arguments,'王五');
  console.log(arguments);//['张三','李四','王五']
})('张三','李四')
```

5.合并数组

```javascript
let arr1=[1,2,3]; 
let arr2=[4,5,6]; 
Array.prototype.push.apply(arr1,arr2); //将arr2合并到了arr1中
```

6.求数组最大值

```javascript
Math.max.apply(null,arr)
```

7.判断字符类型

```javascript
Object.prototype.toString.call({})
```

### 4.3 bind

bind是function的一个函数扩展方法，bind以后代码重新绑定了func内部的this指向,不会调用方法,不兼容IE8

```javascript
var name = '李四'
 var foo = {
   name: "张三",
   logName: function(age) {
   console.log(this.name, age);
   }
 }
 var fooNew = foo.logName;
 var fooNewBind = foo.logName.bind(foo);
 fooNew(10)//李四,10
 fooNewBind(11)//张三,11  因为bind改变了fooNewBind里面的this指向
```

## 5.JS常见的四种设计模式

### 5.1 工厂模式

简单的工厂模式可以理解为解决多个相似的问题;

```javascript
function CreatePerson(name,age,sex) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.sex = sex;
    obj.sayName = function(){
        return this.name;
    }
    return obj;
}
var p1 = new CreatePerson("longen",'28','男');
var p2 = new CreatePerson("tugenhua",'27','女');
console.log(p1.name); // longen
console.log(p1.age);  // 28
console.log(p1.sex);  // 男
console.log(p1.sayName()); // longen

console.log(p2.name);  // tugenhua
console.log(p2.age);   // 27
console.log(p2.sex);   // 女
console.log(p2.sayName()); // tugenhua  
```

### 5.2单例模式

只能被实例化(构造函数给实例添加属性与方法)一次

```javascript
// 单体模式
var Singleton = function(name){
    this.name = name;
};
Singleton.prototype.getName = function(){
    return this.name;
}
// 获取实例对象
var getInstance = (function() {
    var instance = null;
    return function(name) {
        if(!instance) {//相当于一个一次性阀门,只能实例化一次
            instance = new Singleton(name);
        }
        return instance;
    }
})();
// 测试单体模式的实例,所以a===b
var a = getInstance("aa");
var b = getInstance("bb");  
```

### 5.3 沙箱模式

将一些函数放到自执行函数里面,但要用闭包暴露接口,用变量接收暴露的接口,再调用里面的值,否则无法使用里面的值

```javascript
let sandboxModel=(function(){
    function sayName(){};
    function sayAge(){};
    return{
        sayName:sayName,
        sayAge:sayAge
    }
})()
```

### 5.4 发布者订阅模式

就例如如我们关注了某一个公众号,然后他对应的有新的消息就会给你推送,

```javascript
//发布者与订阅模式
    var shoeObj = {}; // 定义发布者
    shoeObj.list = []; // 缓存列表 存放订阅者回调函数

    // 增加订阅者
    shoeObj.listen = function(fn) {
        shoeObj.list.push(fn); // 订阅消息添加到缓存列表
    }

    // 发布消息
    shoeObj.trigger = function() {
            for (var i = 0, fn; fn = this.list[i++];) {
                fn.apply(this, arguments);//第一个参数只是改变fn的this,
            }
        }
     // 小红订阅如下消息
    shoeObj.listen(function(color, size) {
        console.log("颜色是：" + color);
        console.log("尺码是：" + size);
    });

    // 小花订阅如下消息
    shoeObj.listen(function(color, size) {
        console.log("再次打印颜色是：" + color);
        console.log("再次打印尺码是：" + size);
    });
    shoeObj.trigger("红色", 40);
    shoeObj.trigger("黑色", 42);  
```

代码实现逻辑是用数组存贮订阅者, 发布者回调函数里面通知的方式是遍历订阅者数组,并将发布者内容传入订阅者数组

更多设计模式请戳:[Javascript常用的设计模式详解](https://link.juejin.im?target=https%3A%2F%2Fwww.cnblogs.com%2Ftugenhua0707%2Fp%2F5198407.html)

## 6.对象与原型链

###6.0创建对象的方法

1. 字面量和new Object()

   ```javascript
   var o1 = {name: 'o1'}
   var o2 = new Object({name: 'o2'})
   ```

   

2. 构造函数

   ```javascript
   var M = function (name) { this.name = name; }
   var o3 = new M('o3')
   ```

   

3. Object.create

   ```javascript
   var p = {name: 'p'}
   var o4 = Object.create(p)
   ```

   

### 6.1 原型链定义

对象继承属性的一个链条

### 6.2构造函数,实例与原型对象的关系

![图片描述](https://user-gold-cdn.xitu.io/2018/6/8/163deceaab1052c8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)图片描述

```javascript
var Person = function (name) { this.name = name; }//person是构造函数
var o3personTwo = new Person('personTwo')//personTwo是实例
```

![图片描述](https://user-gold-cdn.xitu.io/2018/6/8/163decf026f2d9d7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)图片描述

原型对象都有一个默认的constructor属性指向构造函数

### 6.3 创建实例的方法

1.字面量

```javascript
let obj={'name':'张三'}
```

2.Object构造函数创建

```javascript
let Obj=new Object()
Obj.name='张三'
```

3.使用工厂模式创建对象

```javascript
function createPerson(name){
 var o = new Object();
 o.name = name;
 };
 return o; 
}
var person1 = createPerson('张三');
```

4.使用构造函数创建对象

```javascript
function Person(name){
 this.name = name;
}
var person1 = new Person('张三');
```

### 6.4 new运算符

1.创了一个新对象;
 2.this指向构造函数;
 3.构造函数有返回,会替换new出来的对象,如果没有就是new出来的对象
 4.手动封装一个new运算符

```javascript
var new2 = function (func) {
    var o = Object.create(func.prototype);//创建对象
    var k = func.call(o);//改变this指向，把结果付给k
    if (typeof k === 'object') {//判断k的类型是不是对象
        return k;//是，返回k
    } else {
        return o;//不是返回返回构造函数的执行结果
    }
}  
```

更多详情:[详谈JavaScript原型链](https://link.juejin.im?target=https%3A%2F%2Fwww.cnblogs.com%2Fchengzp%2Fp%2Fprototype.html)

### 6.5 对象的原型链

![图片描述](https://user-gold-cdn.xitu.io/2018/6/8/163ded0dad3145ec?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)图片描述

## 7.继承的方式

JS是一门弱类型动态语言,封装和继承是他的两大特性

### 7.1原型链继承

将父类的实例作为子类的原型
 1.代码实现
 定义父类:

```javascript
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
```

子类:

```javascript
function Cat(){ 
}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

//&emsp;Test Code
var cat = new Cat();
console.log(cat.name);//cat
console.log(cat.eat('fish'));//cat正在吃：fish  undefined
console.log(cat.sleep());//cat正在睡觉！ undefined
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
```

2.优缺点
 简单易于实现,但是要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行,无法实现多继承

### 7.2 构造继承

实质是利用call来改变Cat中的this指向
 1.代码实现
 子类:

```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
```

2.优缺点
 可以实现多继承,不能继承原型属性/方法

### 7.3 实例继承

为父类实例添加新特性，作为子类实例返回
 1.代码实现
 子类

```javascript
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}
```

2.优缺点
 不限制调用方式,但不能实现多继承

### 7.4 拷贝继承

将父类的属性和方法拷贝一份到子类中
 1.子类:

```javascript
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}
```

2.优缺点
 支持多继承,但是效率低占用内存

### 7.5 组合继承

通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
 1.子类:

```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
```

### 7.6 寄生组合继承

```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
```

### 7.7 ES6的extends继承

ES6 的继承机制是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this,

[阮一峰ECMAScript 6 入门之extends继承](http://es6.ruanyifeng.com/#docs/class-extends)

```javascript
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}   
```

更多详情请戳:[JS继承的实现方式](https://link.juejin.im?target=https%3A%2F%2Fwww.cnblogs.com%2Fhumin%2Fp%2F4556820.html%23undefined)

## 参考文献:

[Javascript设计模式详解](https://link.juejin.im?target=https%3A%2F%2Fwww.cnblogs.com%2Ftugenhua0707%2Fp%2F5198407.html)
 [JS继承的实现方式](https://link.juejin.im?target=https%3A%2F%2Fwww.cnblogs.com%2Fhumin%2Fp%2F4556820.html%23undefined)
 [详谈JavaScript原型链](https://link.juejin.im?target=https%3A%2F%2Fwww.cnblogs.com%2Fchengzp%2Fp%2Fprototype.html)

## 8函数高级

### 8.1闭包

#### 8.1.1闭包的理解

1. 如何产生闭包?

   当一个嵌套的内部(子)函数引用了嵌套的外部(父)函数的变量(函数)时, 就产生了闭包

2. 闭包到底是什么?
  使用chrome调试查看
  理解一: 闭包是嵌套的内部函数(绝大部分人)
  理解二: 包含被引用变量(函数)的对象(极少数人)
  注意: 闭包存在于嵌套的内部函数中

3. 产生闭包的条件?
  函数嵌套
  内部函数引用了外部函数的数据(变量/函数)

  ```javascript
  function fn1 () {
      var a = 3
      function fn2 () {
        console.log(a)
      }
    }
    fn1()
  ```

#### 8.1.2常见的闭包

1. 将函数作为另一个函数的返回值

   ```javascript
    function fn1() {
       var a = 2
   
       function fn2() {
         a++
         console.log(a)
       }
   
       return fn2
     }
     var f = fn1()
     f() // 3
     f() // 4
   ```

   

2. 将函数作为实参传递给另一个函数调用

   ```javascript
   function showMsgDelay(msg, time) {
       setTimeout(function () {
         console.log(msg)
       }, time)
     }
     showMsgDelay('hello', 1000)
   ```

 

####8.1.3闭包的作用 

1. 使用函数内部的变量在函数执行完后, 仍然存活在内存中(延长了局部变量的生命周期)

2. 让函数外部可以操作(读写)到函数内部的数据(变量/函数)

   ```javascript
   function fun1() {
       var a = 3;
   
       function fun2() {
         a++;            //引用外部函数的变量--->产生闭包
         console.log(a);
       }
   
       return fun2;
     }
     var f = fun1();  //由于f引用着内部的函数-->内部函数以及闭包都没有成为垃圾对象
   
     f();   //间接操作了函数内部的局部变量
     f();
   ```

   

#### 8.1.4闭包的生命周期

1. 产生: 在嵌套内部函数定义执行完时就产生了(不是在调用)

2. 死亡: 在嵌套的内部函数成为垃圾对象时

   ```javascript
    function fun1() {
       //此处闭包已经产生
       var a = 3;
   
       function fun2() {
         a++;
         console.log(a);
       }
   
       return fun2;
     }
     var f = fun1();
   
     f();
     f();
     f = null //此时闭包对象死亡
   ```

   

#### 8.1.5闭包的应用

  *模块化: 封装一些数据以及操作数据的函数, 向外暴露一些行为
  *循环遍历加监听
  *JS框架(jQuery)大量使用了闭包

**闭包的应用：定义JS模块**
  	1.具有特定功能的js文件
  	2.将所有的数据和功能都封装在一个函数内部(私有的)
  	3.只向外暴露一个包信n个方法的对象或函数
  	4.模块的使用者, 只需要通过模块暴露的对象调用方法来实现对应的功能

1_coolModule.js

```javascript
/**
 * 自定义模块1
 */
function coolModule() {
  //私有的数据
  var msg = 'atguigu'
  var names = ['I', 'Love', 'you']

  //私有的操作数据的函数
  function doSomething() {
    console.log(msg.toUpperCase())
  }
  function doOtherthing() {
    console.log(names.join(' '))
  }

  //向外暴露包含多个方法的对象
  return {
    doSomething: doSomething,
    doOtherthing: doOtherthing
  }
}
```

引入

```javascript
<script type="text/javascript" src="_coolModule.js"></script>
<script type="text/javascript">
  var module = coolModule()
  module.doSomething()
  module.doOtherthing()
```

2_coolModule2.js

```javascript
/**
 * 自定义模块2
 */
(function (window) {
  //私有的数据
  var msg = 'atguigu'
  var names = ['I', 'Love', 'you']
  //操作数据的函数
  function a() {
    console.log(msg.toUpperCase())
  }
  function b() {
    console.log(names.join(' '))
  }

  window.coolModule2 =  {
    doSomething: a,
    doOtherthing: b
  }
})(window)
```

引入

```javascript
<script type="text/javascript" src="_coolModule2.js"></script>
<script type="text/javascript">
  coolModule2.doSomething()
  coolModule2.doOtherthing()
```

#### 8.1.6闭包的缺点及解决

1. 缺点
  * 函数执行完后, 函数内的局部变量没有释放, 占用内存时间会变长
  * 容易造成内存泄露
2. 解决
  * 能不用闭包就不用
  * 及时释放

```javascript
function fn1() {
    var a = 2;

    function fn2() {
      a++;
      console.log(a);
    }

    return fn2;
  }
  var f = fn1();

  f(); // 3
  f(); // 4

  f = null // 释放
```

#### 8.1.7闭包的面试题

```javascript
//代码片段一
var name = "The Window";
var object = {
  name: "My Object",
  getNameFunc: function () {
    return function () {
      return this.name;
    };
  }
};
console.log(object.getNameFunc()());  //?

//代码片段二
var name2 = "The Window";
var object2 = {
  name2: "My Object",
  getNameFunc: function () {
    var that = this;
    return function () {
      return that.name2;
    };
  }
};
console.log(object2.getNameFunc()()); //?

//代码片段三
function fun(n, o) {
  console.log(o)
  return {
    fun: function (m) {
      return fun(m, n)
    }
  }
}
var a = fun(0)
a.fun(1)
a.fun(2)
a.fun(3) //undefined,?,?,?

var b = fun(0).fun(1).fun(2).fun(3) //undefined,?,?,?

var c = fun(0).fun(1)
c.fun(2)
c.fun(3) //undefined,?,?,?
```


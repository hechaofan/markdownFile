# es5

## 变量和函数的提升

- Js代码分为两个阶段：编译阶段和执行阶段

  **编译阶段**：找到所有的声明

  ​    *<u>变量的声明</u>*：         操作会被提升到执行环境的顶部，值是undefined(表示未初始化)

  ​    *<u>函数声明提升</u>*：     会在编译阶段把声明和函数体整体都提前到执行环境顶部

  ​    *<u>函数表达式提升</u>*： 其实就是变量声明的一种，声明操作会被提升到执行环境顶部，并赋值undefined。

    **执行阶段**：赋值操作被留在原地等到执行  

- **函数优先提升**：

  声明的顺序是这样的：

  1. 找到所有的函数声明，初始化函数体，如有同名的函数则会进行覆盖

  2. 查找变量声明，初始化为undefined，如果已经存在同名的变量，就什么也不做直接略过

   

##数据类型，赋值问题，值传递与址传递

### 数据类型

基本数据类型

number、string、boolean、null、undefined、symbol（es6新加的）

引用数据类型：object

### 栈和堆

栈：自动分配内存空间，系统自动释放，里面存放的是**基本类型的值和引用类型的地址**

堆：动态分配的内存，大小不定，也不会自动释放。里面**存放引用类型的值。**

### 赋值

在将一个值赋给变量时，解析器必须确定这个值是基本类型值还是引用类型值。

基本数据类型：在栈内保存基本类型的值

引用数据类型：在栈内保存引用类型的地址，在堆内保存引用类型的值

### 值传递与址传递

基本类型与引用类型最大的区别实际就是传值与传址的区别

**值传递：**

基本类型采用的是值传递。

基本数据赋值就是在内存开辟一段新的栈内存，然后将值赋值到内存中

```javascript
var a = 10;
var b = a;
a ++ ;
console.log(a); // 11
console.log(b); // 10
```

![基本数据类型的赋值](https://user-gold-cdn.xitu.io/2017/9/3/8d973a9718da1806d19db0c1541ff425) 

**址传递：**

引用类型则是地址传递，将存放在栈内存中的地址赋值给接收的变量。

引用数据类型的赋值是保存在栈内存中引用地址的赋值，赋值后的变量指向同一个对象，互相有影响

```javascript
var a = {}; // a保存了一个空对象的实例
var b = a;  // a和b都指向了这个空对象
a.name = 'jozo';
console.log(a.name); // 'jozo'
console.log(b.name); // 'jozo'
b.age = 22;
console.log(b.age);// 22
console.log(a.age);// 22
console.log(a == b);// true
```

![引用类型的赋值](https://user-gold-cdn.xitu.io/2017/9/3/01dad9dc00fb0efe81d9bcbe9d30a1bd) 



# es6







# DOM







#BOM





# 执行上下文

执行上下文可以理解为当前代码的执行环境








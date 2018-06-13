

#Promise对象

[TOC]



##1理解
1:Promise对象: 代表了未来某个将要发生的事件(通常是一个异步操作)
2:有了promise对象, 可以将异步操作以同步的流程表达出来, 避免了层层嵌套的回调函数(俗称'回调地狱')
3:ES6的Promise是一个构造函数, 用来生成promise实例
##2使用promise基本步骤(2步)
1：创建promise对象

> ```javascript
> let promise = new Promise((resolve, reject) => {
>         //初始化promise状态为 pending
>       //执行异步操作
>       if(异步操作成功) {
>         resolve(value);//修改promise的状态为fullfilled
>       } else {
>         reject(errMsg);//修改promise的状态为rejected
>       }
>     })
> ```

2：调用promise的then()

```javascript
promise.then(function(
  result => console.log(result),
  errorMsg => alert(errorMsg)
))
```
## 3promise对象的3个状态
  * pending: 初始化状态
  * fullfilled: 成功状态
  * rejected: 失败状态

## 4应用:
  * 使用promise实现超时处理

  * 使用promise封装处理ajax请求

      ```javascript
        let request = new XMLHttpRequest();
      
          request.onreadystatechange = function () {
      
          }
      
          request.responseType = 'json';
      
          request.open("GET", url);
      
          request.send();
      
      ```

      

## 5创建一个promise实例对象
```javascript
let promise = new Promise((resolve, reject) => {
    //初始化promise的状态为pending---->初始化状态
    console.log('1111');//同步执行
    //启动异步任务
    setTimeout(function () {
        console.log('3333');
        //resolve('atguigu.com');//修改promise的状态pending---->fullfilled（成功状态）
        reject('xxxx');//修改promise的状态pending----->rejected(失败状态)
    },1000)
});
promise.then((data) => {
    console.log('成功了。。。' + data);
}, (error) => {
    console.log('失败了' + error);
});
console.log('2222');
```

##6定义一个请求news的方法
```javascript
function getNews(url) {
    //创建一个promise对象
    let promise = new Promise((resolve, reject) => {
        //初始化promise状态为pending
        //启动异步任务
        let request = new XMLHttpRequest();
        request.onreadystatechange = function () {
            if(request.readyState === 4){
                if(request.status === 200){
                    let news = request.response;
                    resolve(news);
                }else{
                    reject('请求失败了。。。');
                }
            }
        };
        request.responseType = 'json';//设置返回的数据类型
        request.open("GET", url);//规定请求的方法，创建链接
        request.send();//发送
    })
    return promise;
}

getNews('http://localhost:3000/news?id=2')
        .then((news) => {
            console.log(news);
            document.write(JSON.stringify(news));
            console.log('http://localhost:3000' + news.commentsUrl);
            return getNews('http://localhost:3000' + news.commentsUrl);
        }, (error) => {
            alert(error);
        })
        .then((comments) => {
            console.log(comments);
            document.write('<br><br><br><br><br>' + JSON.stringify(comments));
        }, (error) => {
            alert(error);
        })

```
# class类

## 理解

```
1. 通过class定义类/实现类的继承
2. 在类中通过constructor定义构造方法
3. 通过new来创建类的实例
4. 通过extends来实现类的继承
5. 通过super调用父类的构造方法
6. 重写从父类中继承的一般方法
```

## 代码示例

```javascript
class Person {
        //调用类的构造方法
        constructor(name, age){
            this.name = name;
            this.age = age;

        }
        //定义一般的方法
        showName(){
            console.log(this.name, this.age);
        }
    }
    let person = new Person('kobe', 39);
    console.log(person, person.showName());

    //定义一个子类
    class StrPerson extends Person{
        constructor(name, age, salary){
            super(name, age);//调用父类的构造方法
            this.salary = salary;
        }
        showName(){//在子类自身定义方法
            console.log(this.name, this.age, this.salary);
        }
    }
    let str = new StrPerson('weide', 38, 1000000000);
    console.log(str);
    str.showName();
```
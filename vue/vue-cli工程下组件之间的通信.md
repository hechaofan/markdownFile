# vue-cli工程下组件之间的通信

vue组件之间的通信包括三种：

1.父组件向子组件通信

2.子组件向父组件通信

3.同级组件之间的通信



##1.父组件向子组件通信



基本原则: 
    不要在子组件中直接修改父组件的状态数据

使用props
    父组件中标签: `<my-component :name='tom' :age='myAge' :set-name='setName'></my-component>`

组件: MyComponent
    1.在组件内声明所有的props 

```javascript
  		//方式一: 只指定名称
        props: ['name', 'age', 'setName']
        //方式二: 指定名称和类型
        props: {
          name: String,
          age: Number,
          setNmae: Function
        }
        //方式三: 指定名称/类型/必要性/默认值
        props: {
          name: {type: String, required: true, default:xxx},
        }
```

2.所有props的属性都会成为component对象的属性, 模板页面可以直接引用



步骤：

父组件一共需要做4件事

1.import son from './son.js' 引入子组件 son

2.在components : {"son"} 里注册所有子组件名称

3.在父组件的template应用子组件, <son></son>

4.如果需要传递数据给子组件,就在template模板里写 <son :num="number"></son>

```javascript
// 1.引入子组件 

    import counter from './counter'     

	import son from './son'

// 2.在ccmponents里注册子组件    

	components : {

        counter,

        son

    },

// 3.在template里使用子组件   <son></son>

// 4.如果需要传递数据,也是在templete里写

  <counter :num="number"></counter>

```

  子组件只需要做1件事

1.用props接受数据,就可以直接使用数据

2.子组件接受到的数据,不能去修改。如果你的确需要修改,可以用计算属性,或者把数据赋值给子组件data里的一个变量

```javascript
// 1.用Props接受数据     
	props: [ 'num'],
// 2.如果需要修改得到的数据,可以这样写
		props: ['num'],  
		data () {        
			return {
				number : this.num
        }
    },
```



## 2.子组件向父组件通信

```javascript
子组件
<div @click="confirmAddress">确定</div>
confirmAddress() {
      this.$emit('myAddress',this.myAddress)
    }
	
父组件中
<mintuiPicker @myAddress="getAddress"></mintuiPicker>
getAddress (address) {
        this.address = address
    }
```

###过程：

父组件一共需要做2件事情
在template里定义事件
在methods里写函数,监听子组件的事件触发

```javascript
// 1. 在templete里应用子组件时,定义事件changeNumber
	<counter :num="number" @changeNumber="changeNumber"></counter>
// 2. 用changeNumber监听事件是否触发
	methods: {
	changeNumber(e){                
		console.log('子组件emit了',e);                
		this.number = e
	  }
	}

```

子组件一共需要1件事情

在数据变化后,用$emit触发即可

```javascript
// 1. 子组件在数据变化后,用$emit触发即可,第二个参数可以传递参数
	methods: {
		increment(){                    
			this.number++                    
			this.$emit('changeNumber', this.number)
				},
		}
```



## 3.同级组件之间的通信

③同级组件的通信：子组件ZiZuJianOne.vue和子组件ZiZuJianTwo.vue之间的通信
要点：建立一个新的Vue实例，以后它就承担起了组件之间通信的桥梁了，也就是中央事件总线。
方法：新建一个js文件,命名为event.js。
event.js的代码：

![](https://raw.githubusercontent.com/hechaofan/imgMarkdown/master/2018-07-10_122558.png)

子组件ZiZuJianOne.vue的代码： 

![](https://raw.githubusercontent.com/hechaofan/imgMarkdown/master/2018-07-10_122619.png)

子组件ZiZuJianTwo.vue的代码： 

![](https://raw.githubusercontent.com/hechaofan/imgMarkdown/master/2018-07-10_122631.png)

解释：

①在两个组件中均引入：import bus from '../assets/event.js'；

②使用bus.$emit('事件名称'，'传入参数')，作为发送消息的那一方；

③使用bus.$on('事件名称'，'回调函数')，作为接收消息的那一方。





#利用localStorage或者sessionStorage



这种通信比较简单,缺点是数据和状态比较混乱,不太容易维护。

通过window.localStorage.getItem(key) 获取数据
通过window.localStorage.setItem(key,value) 存储数据

注意用JSON.parse() / JSON.stringify() 做数据格式转换。



# 利用Vuex


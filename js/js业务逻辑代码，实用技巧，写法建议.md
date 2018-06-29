

# js业务逻辑代码

## 月份坐标轴

**第一种**

```javascript
var _data = []
  var currentYear = new Date().getFullYear()
  var now = new Date().getMonth()
  for (let i = 0; i < 12; i++) {
    if (now-i < 0 ) {
      _data.unshift(currentYear-1 + '年' +  (12+now-i+1 )+ '月')
    }else {
      _data.unshift(currentYear + '年' + (now-i+1) + '月')
    }
  }
```

**第二种**

```javascript
var _date=[],dateData=["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"];
  //准备一个月份反转的数组
  var dateDataRet=Object.assign([],dateData).reverse();
  //获取当前年份
  var yearText=new Date().getFullYear();
  //获取当前月份   3月-now=2,12月now=11...
  var now=new Date().getMonth();
  for(let i=0;i<6;i++){
    if(now-i<0){
      //如果now-i<0，从dateDataRet里面拿数据，下标=|now-i|-1。
      _date.push(yearText-1+'年'+dateDataRet[Math.abs(now-i)-1]);
    }
    else{
      //从dateData里面拿数据，下标=now-i
      _date.push(yearText+'年'+dateData[now-i]);
    }

  }
  console.log(_date.reverse());
```

## 数组对比

**需求**

1、如果arryA中有a，arryB中没有，那么在arryB中增加一个key值为a的boj，且其他属性值可均为'0';如下： {key:'a',num1:'0',num2:'0',num3:'0',tot':0'}
2、如果arryA中有a，arryB中也有key值为a的obj,那么arryB则不改变，并且该obj里的其他属性和属性值均不变;
3、如果在arryA中删除了a，那么arryB中key值为a的obj整个删掉。

```javascript
var arrayA = ['a','b','c','e'];
  var arrayB = [
    {
      key: 'd',
      num1: '111',
      num2: '222',
      num3: '333',
      tot:666
    },{
    key:'a',
    num1:'1',
    num2:'2',
    num3:'3',
    tot:'6'
  },{
    key:'b',
    num1:'11',
    num2:'22',
    num3:'33',
    tot:'66'
  },{
    key: 'c',
    num1: '111',
    num2: '222',
    num3: '333',
    tot:666
  },];
//准备临时数组
  function compareArr(arr1,arr2){
    var result=[],arr;
    //遍历
    for(var i=0;i<arr1.length;i++){
      //根据arr1[i]的值，查找arrayB，如果arr2中的有满足条件（arrayB中的对象，有key值等于arrayA[i]）的  			项，就会返回满足条件的项，否则返回underfind;
      arr=arr2.find(function(val){return val.key===arr1[i]});
      //如果arr不是undefind，就会添加arr，否则{key:arrayA[i],num1:'0',num2:'0',num3:'0',tot:'0'}。
      arr?result.push(arr):result.push({key:arrayA[i],num1:'0',num2:'0',num3:'0',tot:'0'});
    }
  }
  console.log(compareArr(arrayA, arrayB));
  function compareArr(arr1,arr2) {
    var result = [],arr
    for (let i = 0; i < arr1.length; i++) {
      arr = arr2.find(value => value.key === arr1[i])
      arr ? result.push(arr) : result.push({key: arr1[i],
        num1: '0',
        num2: '0',
        num3: '0',
        tot:0})

    }
    return result
  }
```

## 学院获奖

**需求**

统计学生申请优秀毕业生
  1自己申请过的
  2复合要求的（成绩优秀，拿过奖学金，获得过三好学生三个条件满足两个）

```javascript
  //todo 数组去重
  function removeRepeatArr(arr) {
     return [...new Set(arr)]
  }
  //todo 一个数据在数组中出现的次数
  function getEleCount(obj,ele) {
    let num = 0
    for (var i = 0,len=obj.length; i < len; i++) {
      if (ele === obj[i]){
        num ++;
      }
    }
    return num
  }
  let studentList = [
    {
      name: 'aa',
      isApply: false,
      id: 1
    },
    {
      name: 'bb',
      isApply: true,
      id: 2
    },
    {
      name: 'cc',
      isApply: true,
      id: 3
    }
  ];
  //过滤申请学生
  let _student = studentList.filter(value => value.isApply)
  //将复合条件的学生id，添加到accord中，查找出现id个数，来过滤复合条件的学生
  let isExcellent = [1, 2, 3, 4, 5], isScholarship = [4, 2, 5, 6, 2, 1, 2], isThreeGood = [2, 1, 4, 52, 36], accord = [];
//接受三个数组中的数据到一个数组中方法一
  accord = [...removeRepeatArr(isExcellent),...removeRepeatArr(isScholarship),...removeRepeatArr(isThreeGood)]
//接受三个数组中的数据到一个数组中方法二
/*accord.push.apply(accord, removeRepeatArr(isExcellent));
accord.push.apply(accord, removeRepeatArr(isScholarship));
accord.push.apply(accord, removeRepeatArr(isThreeGood));*/
  console.log(accord);
  let accordStudent = []
  for (let i = 0; i < _student.length; i++) {
    if(getEleCount(accord,_student[i].id)>=2){
      accordStudent.push(_student[i])
    }

  }
  console.log(accordStudent);
```

## 数组连续的最大长度

```javascript
  //假如有一个数组，下面这个数组最大的连续长度就是4——————8,9,10,11
  var arr=[1,2,4,5,6,8,9,10,11,12];
  console.log(countLen(arr));
  //代码实现
  function countLen(arr) {
    //定义一个目前数组连续的长度，和数组连续的最大长度
    var nowArr = 1,maxArr = 0
    //判断是否为数组，数组为空
    if(!Array.isArray(arr) || arr.length == 0){
        return 0
    }
    for (var i = 1; i < arr.length; i++) {
      //计算目前数组连续的长度
      if(arr[i]-arr[i-1] === 1){
          nowArr++
      }else {
        //目前数组连续的长度大于数组连续的最大长度就重新赋值给数组连续的最大长度
        if (nowArr>maxArr) {
          maxArr = nowArr
        }
        //使目前数组连续的长度重置
        nowArr = 1
      }
    }
   //循环完再判断一次当前连续长度是否大于最大连续长度（避免最大连续长度是数组最后面几个数组时产生的bug）
    if(nowArr>maxArr){
      maxArr = nowArr
    }
    return maxArr
  }
```

## 答案连对最大值

```javascript
  var arr= [true,true,false,true,true,true,true,]
  function maxRight(arr) {
    if (!Array.isArray(arr) || arr.length ===0) {
      return 0
    }
    var nowArr= 0,maxArr=0
    for (var i = 0; i < arr.length; i++) {
      if(arr[i]){
          nowArr++
      }else {
        if(nowArr>maxArr){
            maxArr = nowArr
        }
        nowArr = 0
      }
      
    }
    if (nowArr>maxArr) {
      maxArr = nowArr
    }
    return maxArr
  }
  console.log(maxRight(arr));
```

## 命名方式转换

```javascript
 // todo 比如驼峰命名方式转'-'命名方式。

  var str = "shouHou";
  //$1-第一个括号匹配的内容
  //这个实例，$1='H'
  str = str.replace(/([A-Z])/g,"-$1").toLowerCase();

  // todo 比如'-'命名方式转驼峰命名方式var str="shou-hou";
  //$0-匹配的结果   $1-第一个括号匹配的内容
  //这个实例$0='-h'    $1='h'
  str=str.replace(/-(\w)/g,function($0,$1){
    return $1.toUpperCase();
  });
```

## 格式化字符

**这个最常见的就是在金额方面的显示需求上，比如后台返回10000。前端要显示成10,000或者其他格式等！** 

```javascript
//str
  //size-每隔几个字符进行分割 默认3
  //delimiter-分割符 默认','
  function formatText(str,size,delimiter){
    var _str=str.toString();
    var _size=size||3,_delimiter=delimiter||',';
    //todo 如果_size是3,  "\d{1,3}(?=(\d{3})+$)"
    var regText='\\d{1,'+_size+'}(?=(\\d{'+_size+'})+$)';
     // todo /\d{1,3}(?=(\d{3})+$)/g     这个正则的意思：匹配连续的三个数字，但是这些三个数字不能是字符串的开头1-3个字符
    var reg=new RegExp(regText,'g');
     // todo (-?) 匹配前面的-号   (\d+)匹配中间的数字   ((\.\d+)?)匹配小数点后面的数字
     //todo $0-匹配结果，$1-第一个括号返回的内容----(-?)    $2,$3如此类推
    return _str.replace(/^(-?)(\d+)((\.\d+)?)$/, function ($0, $1, $2, $3) {
      return $1 + $2.replace(reg, '$&,') + $3;
    })
  }
```

## 对象合并，并且记录异常数据

多处地方记录了同一个信息 。现在要合并信息，并且记录可能有异常的信息。比如上面的name属性，在四个对象都有，而且四个个对象的值不一样，那么就不知道到底是哪个对象中的name属性是正确的。所以，就得把name这个属性记录起来，方便以后核对name这个属性。

![](C:\Users\何超凡\Desktop\图片\2018-06-13_165640.png)

```javascript
 let objA={
    name:"何超凡",
    sex:"男",
  },objB={
    name:"哈哈哈",
    job:"web前端"
  },
  objC={
    name:"妈卖批",
    add:"杭州",
    job:"w前端工程师"
  },
    objD={
      name:"哦哦哦",
      age:18,
      job:"w前端工程师"
    }
  let arr = [objA,objB,objC,objD]
  let objAll={};
  function assignObj(objArr) {
    let _obj={};
    for(let i=0;i<objArr.length;i++){
      _obj=Object.assign(_obj,objArr[i]);
    }
    return JSON.parse(JSON.stringify(_obj));
  }
  objAll=assignObj(arr);
  objAll.warnInfo=["对于最后一个对象:"];
  function checkObj(_objAll,objList) {
    //获取所有属性
    let _keys=Object.keys(_objAll);
    for(let i=0;i<objList.length;i++){
      for(let j=0;j<_keys.length;j++){
        //todo 记录一个数据的条件  对于这两个数据_objAll[_keys[j]]一定存在  objList[i][_keys[j]]不一定存在
        // _objAll[_keys[j]]和objList[i][_keys[j]]两个数据一定都要存在
        // 而且这两个值是不严格全等的，那么就是一个数据，需要记录！
        if(objList[i][_keys[j]]!==undefined&&_objAll[_keys[j]]!==objList[i][_keys[j]]){
          _objAll.warnInfo.push('第'+(i+1)+'个对象的'+_keys[j]+'属性值不一致');
        }
      }
    }
    return _objAll;
  }
  console.log(checkObj(objAll,arr));
```

## 将对象数据转化为数组数据

发送请求返回一个含有商品信息的对象

```
http://example.com?proId=100072236-8
var searchParam={proId:'100072236-8',proName:'甘油'}  
```

转化为数组渲染到页面中

```javascript
var searchTag=[
    {label:'产品编码',value:'100072236-8'},
    {label:'产品名称',value:'甘油'}
]
```

![img](https://user-gold-cdn.xitu.io/2017/12/20/1607163005707a87?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```javascript
//代码实现
//todo 发送请求返回一个含有商品信息的对象,转化为数组，渲染到页面
  //发送请求 http://example.com?proId=100072236-8
  var searchTag=[
    {label:'产品编码',value:'100072236-8'},
    {label:'产品名称',value:'甘油'}
  ]
  //返回的数据
  var searchParam={proId:'100072236-8',proName:'甘油'}
  var searchTag = []
  var searchText = {proId:'商品id',proName:'商品名称'}
  Object.keys(searchParam).forEach(item => {
    searchTag.push({
      label:searchText[item],
      key:item,
      value:searchParam[item]
    })
  })
  console.log(searchTag);
```

![1528944890669](C:\Users\何超凡\AppData\Local\Temp\1528944890669.png)

## 倒入excel内容

就是excel上这样的内容

![img](https://user-gold-cdn.xitu.io/2017/12/20/160716308aa34b59?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

转成下面的数据

![img](https://user-gold-cdn.xitu.io/2017/12/20/160716308ee554b8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2017/12/20/16071630906c2869?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

目录如下

![img](https://user-gold-cdn.xitu.io/2017/12/20/16071630907cb32c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

下面开始写代码，我们利用node.js来写

```javascript
let path = require('path');
//使用ejsexcel读取excel文件  npm install ejsexcel --save
let ejsExcel=require('ejsexcel');
let fs=require('fs');
//读取excel
let exBuf=fs.readFileSync(__dirname+'/resource/userList.xlsx');
let _data=[];
//获取成功后
ejsExcel.getExcelArr(exBuf).then(exlJson=>{
    //获取excel数据
    let workBook=exlJson;
    //获取excel第一张表 sheet1
    let workSheets=workBook[0];
    //导出js的路径
    let newfilepath=path.join(__dirname,"/resource/test.js");
    //遍历第一张表的的每一行数据
    workSheets.forEach((item,index)=>{
        //从第二行开始插入，避免连表头也插入_data里面
        if(index>0){
            //往_data插入单元格个值，item[0]相当于excel中的姓名，item[1]就是excel中的联系电话
            _data.push({
                name:item[0],
                phone:item[1]
            })
        }
    });
    //写入js文件
    fs.writeFileSync(newfilepath, 'let _data='+JSON.stringify(_data)+';export {_data}');
}).catch(error=>{
    //打印获取失败信息
    console.log("读取错误!");
    console.log(error);
});
```

然后命令行执行该js

```
$ node importFile.js
```

然后就发现多了一个test.js文件

![img](https://user-gold-cdn.xitu.io/2017/12/20/16071630c7ba133d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

excel的数据就这样导入成js的一个数组了，只要引入这个数组，就可以正常的使用了！

## 随机循环

当时接到的业务是实际显示客户的信息，感觉有点像音乐播放器的随机循环。

要求：
1.一个提示列表里面，提示的信息每隔1s随机展示。
2.同一轮循环里面，一个提示信息只能展示一次。
3.列表的提示信息全部展示完了，进行下一轮展示。

```javascript
var tipList=['提示1','提示2','提示3','提示4','提示5','提示6','提示7','提示8','提示9'];
  var tipListShow=[];
  tipListShow = Object.assign([],tipList)
  var timer = null
  function play() {
    //Math.floor(Math.random() * tipListShow.length)
    console.log(tipListShow.splice(Math.floor(Math.random() * tipListShow.length), 1)[0]);
    if (tipListShow.length === 0) {
      console.log('加载完一轮')
      tipListShow = Object.assign([],tipList)
    }
    timer = setTimeout(function () {
      play()
    },1000)
  }
  play()
```

![1528959126267](C:\Users\何超凡\AppData\Local\Temp\1528959126267.png)

## 数值区间

如下图，就是几个数值区间，而且会有一个最小值和最大值

![img](https://user-gold-cdn.xitu.io/2017/12/20/1607162f4a2281ac?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```javascript
var _min=5,_max=50;
function checkArr(arr,min,max){
    //排序
    arr.sort(function(n1,n2){return n1.min-n2.min})
    //遍历
    for(var i=0;i<arr.length;i++){
        //区间的最小值不能大于等于区间最大值
        if(arr[i].min>=arr[i].max){
            console.log('区间的最小值不能大于等于区间最大值');
            return;
        }
        //区间的最小值不能小于默认最小值
        if(arr[i].min<min){
            console.log('区间的最小值不能小于默认最小值');
            return;
        }
                    
        //区间的最大值不能大于默认最大值
        if(arr[i].max>max){
            console.log('区间的最大值不能大于默认最大值');
            return;
        }
        //元素对比，从第二个元素开始
        if(i>0){
            //minInclude，maxInclude，为false就是不包含，为true就是包含
            //{min:10,max:20,minInclude:false,maxInclude:false}
            //等同于(10,20)
            //{min:20,max:30,minInclude:true,maxInclude:false}
            //等同于[20,30);
            
            //如果前一个的最大值和当前的最小值都是包含情况，那么当前区间的最小值一定要比前一个区间的最大值大1
            if(arr[i].minInclude&&arr[i-1].maxInclude&&arr[i].min-arr[i-1].max!==1){
                console.log('取值范围错误-当前区间的最小值和前一个区间的最大值都是包含情况，当前区间的最小值一定要比前一个区间的最大值大1');
                   return;
                
            }
            //如果前一个的最大值和当前的最小值。一个是包含，一个是不包含，那么当前区间的的最小值一定要等于上一个区间的最大值
            else if(arr[i].minInclude!==arr[i-1].maxInclude&&arr[i].min!==arr[i-1].max){
                console.log('取值范围错误-当前区间的最小值和前一个区间的最大值其中一个是包含，一个是不包含情况，当前区间的最小值一定要等于前一个区间的最大值');
                return;
            }
            //如果前一个的最大值和当前的最小值都是不包含，肯定不满足
            else if((!arr[i].minInclude)&&(!arr[i-1].maxInclude)){
                console.log('取值范围错误-前一个的最大值和当前的最小值都是不包含情况，不满足收尾相连');
                return;
            }
        }
    }
}
```

测试用例

```
var arr1=[{min:10,max:20,minInclude:false,maxInclude:true},{min:21,max:30,minInclude:true,maxInclude:true}],
arr2=[{min:10,max:20,minInclude:false,maxInclude:true},{min:20,max:30,minInclude:true,maxInclude:false}],
arr3=[{min:10,max:20,minInclude:false,maxInclude:true},{min:20,max:30,minInclude:false,maxInclude:false}],
arr4=[{min:10,max:20,minInclude:false,maxInclude:false},{min:20,max:30,minInclude:true,maxInclude:false}],
arr5=[{min:10,max:20,minInclude:false,maxInclude:false},{min:21,max:30,minInclude:true,maxInclude:false}],
arr6=[{min:10,max:20,minInclude:false,maxInclude:false},{min:15,max:30,minInclude:false,maxInclude:false}],
arr7=[{min:10,max:20,minInclude:false,maxInclude:false},{min:20,max:30,minInclude:false,maxInclude:false}],
arr8=[{min:1,max:20,minInclude:false,maxInclude:false},{min:20,max:30,minInclude:false,maxInclude:false}],
arr9=[{min:20,max:20,minInclude:false,maxInclude:false},{min:20,max:30,minInclude:false,maxInclude:false}], 
arr10=[{min:20,max:30,minInclude:false,maxInclude:false},{min:20,max:70,minInclude:false,maxInclude:false}];  
```

运行结果

![img](https://user-gold-cdn.xitu.io/2017/12/20/1607162f3bf74694?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

# js实用技巧

## 更短的数组去重写法

```javascript
  var arr = [1,2,3,2,1,4,5,5,'5']
  var arr1= [...new Set(arr)]
  console.log(arr1);
```

## 对象深浅拷贝

关于对象的深浅拷贝，我个人见解就是有一下几点：

1.深拷贝和浅拷贝只针对像Object, Array这样的引用类型数据。

2.浅拷贝是对对象引用地址进行拷贝，并没有开辟新的栈，也就是拷贝后的结果是两个对象指向同一个引用地址，修改其中一个对象的属性，则另一个对象的属性也会改变。

3.深拷贝则是开启一个新的栈，两个对象对应两个不同的引用地址，修改一个对象的属性，不会改变另一个对象的属性。

### 浅拷贝

```
var myInfo={name:'守候',sex:'男'};
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444b153bd43?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
var newInfo=myInfo;
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444e6487915?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
newInfo.sex='女';
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444e2d6a43b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
console.log(myInfo)   //{name: "守候", sex: "女"}
```

### 假-深拷贝

假-深拷贝这个是自己随性命名的，大家看看就好，别当真！

```
var myInfo={name:'守候',sex:'男'};
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444b153bd43?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
var newInfo=Object.assign({},myInfo)
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444ea8bca2c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
newInfo.sex='女';
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444a56dc84e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
console.log(myInfo)   //{name: "守候", sex: "男"}
console.log(newInfo)   //{name: "守候", sex: "女"}
```

### 真-深拷贝

真-深拷贝这个是自己随性命名的，大家看看就好，别当真！

看着深浅拷贝，区别写法很简单，但是那个上面的深拷贝写法是有问题的。看下面案例

```javascript
var arr=[{a:1,b:2},{a:3,b:4}]
var newArr=Object.assign([],arr)
//截断数组
newArr.length=1
console.log(newArr)//[{a:1,b:2}]
console.log(arr)//[{a:1,b:2},{a:3,b:4}]
//操作newArr，这里看着对arr没影响，实际上已经挖了一个坑，下面就跳进去
newArr[0].a=123
//修改newArr[0]这个对象，也是影响了arr[0]这个对象
console.log(arr[0])//{a: 123, b: 2}
```

为什么会这样呢，因为Object.assign并不是深拷贝，是披着深拷贝外衣的浅拷贝。最多也是Object.assign会课拷贝第一层的值，对于第一层的值都是深拷贝，而到第二层的时候就是 复制引用。类似的情况还有，slice方法和concat方法等。
要解决这个问题，就得自己封装方法！如下

```javascript
//利用递归来实现深拷贝，如果对象属性的值是引用类型（Array,Object），那么对该属性进行深拷贝，直到遍历到属性的值是基本类型为止。  
function deepClone(obj){    
  if(!obj&& typeof obj!== 'object'){      
    return;    
  }    
  var newObj= obj.constructor === Array ? [] : {};    
  for(var key in obj){       
    if(obj[key]){          
      if(obj[key] && typeof obj[key] === 'object'){  
        newObj[key] = obj[key].constructor === Array ? [] : {}; 
        //递归
        newObj[key] = deepClone(obj[key]);          
      }else{            
        newObj[key] = obj[key];         
      }       
    }    
  }    
  return newObj; 
}
var arr=[{a:1,b:2},{a:3,b:4}]
var newArr=deepClone(arr)
console.log(arr[0])//{a:1,b:2}
newArr[0].a=123
console.log(arr[0])//{a:1,b:2}
```

还有一个方法就是简单粗暴法，我现在在用的一个方法！原理很简单，就是先把对象转成字符串，再把字符串转成对象！也能实现同样效果

```javascript
var newArr2=JSON.parse(JSON.stringify(arr));
console.log(arr[0])//{a:1,b:2}
newArr2[0].a=123
console.log(arr[0])//{a:1,b:2}
```

上面所说的浅拷贝，真假深拷贝（自己随性命名的），这几种情况，在开发上都有可能要用到，至于要使用哪一种方式，视情况而定！

## 使用事件委托

一个简单的需求，比如想给ul下面的li加上点击事件，点击哪个li，就显示那个li的innerHTML。这个貌似很简单！代码如下！

```
   <body>
        <ul id="ul-test">
            <li>0</li>
            <li>1</li>
            <li>2</li>
        </ul>
    </body>
    <script type="text/javascript">
        var oUl=document.getElementById("ul-test");
        var oLi=oUl.getElementsByTagName("li");
        for(var i=0,len=oLi.length;i<len;i++){
            oLi[i].addEventListener("click",function(){
                alert(this.innerHTML)
            })
        }
    </script>
```

很简单，这样就实现了，实际上这里有坑，也待优化！
1.for循环，循环的是li，10个li就循环10次，绑定10次事件，100个就循环了100次，绑定100次事件！
2.如果li不是本来就在页面上的，是未来元素，是页面加载了，再通过js动态加载进来了，上面的写法是无效的，点击li是没有反应的！
所以就者需要用事件委托（即使不考虑上面的第二种情况，也是建议使用事件委托）！代码如下

```
<body>
        <ul id="ul-test">
            <li>0</li>
            <li>1</li>
            <li>2</li>
        </ul>
    </body>
    <script type="text/javascript">
        var oUl=document.getElementById("ul-test");
        oUl.addEventListener("click",function(ev){
            var ev=ev||window.event;
            var target=ev.target||ev.srcElement;
            //如果点击的最底层是li元素
            if(target.tagName.toLowerCase()==='li'){
                alert(target.innerHTML)
            }
        })
    </script>
</html>
```

这样写，即使是动态添加进来的li点击也有反应，还有一个就是ul只有一个，事件绑定在ul上，无论li多少个，都是添加一次事件！但是也是可能会有问题，如果li下面还有子元素，那么点击的时候，target可能不是li，而是鼠标点击那个位置的最底层元素！如下图，如果鼠标点击白色区域，那个target就是body元素,鼠标点击绿色区域target就是div元素，鼠标点击蓝色区域target就是ul，点击橙色就是li。

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf444ed94451a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 数组合并

方法一：concat

```javascript
var arr1 = [1,2,3]
var arr2 = [7,5,6]
var Arr = arr1.concat(arr2)
```

方法二: for循环

```javascript
var Arr1 = []
for (var i = 0; i < arr1.length; i++) {
  Arr1.push(arr1[i])

}
for (var i = 0; i < arr2.length; i++) {
  Arr1.push(arr2[i])

}
console.log(Arr1);
```

方法三：reduce

```javascript
var Arr2 = arr2.reduce((coll,item) => {
  coll.push(item)
  return coll
},arr1)
```

方法四: push.apply

```javascript
var arr1=[1,2,3,4,5],arr2=[6,7,8,9,10];
arr1.push.apply(arr1,arr2);
console.log(arr1)//[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

逼格看着高，代码少，也不会产生新的数组，也不难理解，就是调用`arr1.push`这个函数实例的`apply`方法，同时把`arr2`当作参数传入，这样`arr1.push`这个方法就会遍历`arr2`数组的所有元素，达到合并的效果。相当于`arr1.push.apply(arr1,[6,7,8,9,10]);`，最后相当于`arr1.push(6,7,8,9,10)`。遗憾的就是，这个方法对数组长度有限制，网上说法是**不同浏览器，不同的长度限制，一般不超过10万**

之前是建议用push.apply，但是现在保留意见，就是大家觉得哪个方式用哪个方式！这个没有一定的对错！



方法五：...运算符

```javascript
var arr3=[1,2]
var arr4=[3,5]
var arr5=[...arr3,...arr4]
console.log(arr5);
```

## toFixed保留整数

在开发上，经常会遇到最多保留多少位小数或者类似的问题，针对这个，使用toFixed可以很简单的解决问题，但是如果数据是要和后台交互的，而且后台存储的数据一般是保存数字类型，而使用toFixed后生成的是一个字符串，这下，就需要把toFixed生成的是一个字符串转成数字类型，转发很多。今天我说一个最简单--+。代码如下

```javascript
var a= 1.2356.toFixed(2)
console.log(typeof a); //string
console.log(a); //1.24
a =+ a
console.log(typeof a); // number
console.log(a); //1.24
```

PS：a=a|0和~~a也可以实现，但是生成的是一个整数，如下

```javascript
var a=123.36896335.toFixed(2)
console.log(a)//'123.37'
a=a|0  
console.log(a)//123 
//---------------------------------分割线
var a=123.36896335.toFixed(2)
console.log(a)//'123.37'
a=~~a  
console.log(a)//123        
```

## 使用innerHTML添加元素

```javascript
<body>
	<ul></ul>
</body>
<script>
  //todo 在ul中添加li
  var ul = document.querySelector('ul')
  //createElement方式
  console.time()
  for (var i = 0; i < 10; i++) {
    var li = document.createElement('li')
    li.innerHTML=i
    ul.appendChild(li)
  }
  console.timeEnd() //default: 0.259033203125ms
  //使用innerHTML添加元素
  console.time()
  var _html= ''
  for (var i = 0; i < 10; i++) {
    _html += `<li>${i}</li>`

  }
  ul.innerHTML= _html
  console.timeEnd() //default: 0.069091796875ms
</script>
```

第8点也说了，DOM操作能少就少！第一种要操作10次DOM，第二种只需要操作1次DOM。还有一个就是，这个只是很简单的li,如果是下面的列表呢？用第一种方式，得createElement多少次，innerHTML多少次，appendChild多少次？代码多，各个节点的逻辑和嵌套关系也乱！用第二种方式就是一个拼接字符串的操作，比第一种方式好多了

 ## 将参数转成数组

函数里的arguments，虽然拥有length属性，但是arguments不是一个数组，是一个类数组，没有push,slice等方法。有些时候，需要把arguments转成数组，转的方法也不止一个，推荐的是是下面的写法！

```javascript
//两种写法
var _arguments=Array.prototype.slice.apply(arguments)
var _arguments=[].slice.apply(arguments)
```

## 函数节流

这里拿一个栗子说，比如mousemove,onscroll,onresize这些事件触发的时候，可能已经触发了60次事件，这样很消耗性能，而且实际上，我们并不需要这么频繁的触发，只要大约100毫秒触发一次就好！那么这样就需要函数节流了！

普通写法

```
var count = 0;
function beginCount() {
    count++;
    console.log(count);
}
document.onmousemove = function () {
   beginCount();
};
```

效果

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf4453fbde66a?imageslim)

节流写法

```
var count = 0;
function beginCount() {
    count++;
    console.log(count);
}
function delayFn(method, thisArg) {
    clearTimeout(method.props);
    method.props = setTimeout(function () {
        method.call(thisArg)
    },100)
}
document.onmousemove = function () {
    delayFn(beginCount)
};
```

效果

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf445673b76fc?imageslim)

这种方式，其实是有问题的，在不断触发停下来等待100ms才开始执行，中间操作得太快直接无视。于是在网上找到下面这种方案！

第二种节流写法

```
function delayFn2 (fn, delay, mustDelay){
     var timer = null;
     var t_start;
     return function(){
         var context = this, args = arguments, t_cur = +new Date();
         //先清理上一次的调用触发（上一次调用触发事件不执行）
         clearTimeout(timer);
         //如果不存触发时间，那么当前的时间就是触发时间
         if(!t_start){
             t_start = t_cur;
         }
         //如果当前时间-触发时间大于最大的间隔时间（mustDelay），触发一次函数运行函数
         if(t_cur - t_start >= mustDelay){
             fn.apply(context, args);
             t_start = t_cur;
         }
         //否则延迟执行
         else {
             timer = setTimeout(function(){
                 fn.apply(context, args);
             }, delay);
         }
     };
}
var count=0;
function fn1(){
    count++;
    console.log(count)
} 
//100ms内连续触发的调用，后一个调用会把前一个调用的等待处理掉，但每隔200ms至少执行一次
document.onmousemove=delayFn2(fn1,100,200)
```

![img](https://user-gold-cdn.xitu.io/2017/11/15/15fbf44576a04947?imageslim)

> 我现在函数节流用得很少，这两个写法是比较基础的，希望大家能共享下自己的比较好的方法！
>
> 







# js写法建议

## 缓存变量

**1.for循环缓存length**

```
var arr=[1,2,3,4,5,6]
for(var i=0,i<arr.length;i++){
    ...
}
//------------------------分割线
var arr=[1,2,3,4,5,6]
for(var i=0,len=arr.length;i<len;i++){
    ...
}
```

第一段就是每一次循环的时候，都要查询一次arr.length。第二段代码就是缓存了arr.length，每次对比len就好，理论上是第二段代码的写法比较好，性能比较高！但是随着浏览器的发展，这个细节的性能上的影响貌似远远小于预期，现在还是建议缓存！我写了下面的测试用例(谷歌浏览器测试)！

```
var arr100=[], arr10000=[];
for(var i=0;i<100;i++){
    arr100.push(i)
}
for(var i=0;i<10000;i++){
    arr10000.push(i)
}
//缓存情况
function testCache(arr){
    console.time();
    for(var i=0,len=arr.length;i<len;i++){
        
    }
    console.timeEnd()
}
//不缓存情况
function testNoCache(arr){
    console.time();
    for(var i=0;i<arr.length;i++){
        
    }
    console.timeEnd()
}
  testCache(arr100)//     default: 0.0029296875ms
  testCache(arr10000)//   default: 0.098876953125ms
  testNoCache(arr100)//   default: 0.025146484375ms
  testNoCache(arr10000)// default: 0.19775390625ms
//这只是一个最简单的数组，如果遍历的是一个nodeList（元素列表），效果可能会更明显。
```

**2元素事件**

这里我用jquery来讲解，比较容易理解，原生js也是这个道理！如下代码

```
$('.div1').click(function(){
   ...
})
//--------------------------分割线   
var $div1=$('.div1');
$div1.click(function(){
   ...
})
```

上面的代码，改变的也是缓存了$('.div1')，但是这里就建议是第二种写法了，因为第一种点击一次就要查询一次.div1，Dom的操作还是能减少就减少！

 

#**参考文章**

[JavaScript 实用技巧和写法建议](https://juejin.im/post/5a0c199851882531926e4297#heading-2)

[JavaScript 打怪升级 —— 把业务逻辑当练习题做](https://juejin.im/post/5a39b2dcf265da431d3cd036)

[如何优雅的编写 JavaScript 代码](https://zhuanlan.zhihu.com/p/28910636)
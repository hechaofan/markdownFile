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

## 筛选标签

如下图，在下面渲染这个标签

![img](https://user-gold-cdn.xitu.io/2017/12/20/1607163005707a87?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

大家可能第一可能觉得压根没难度
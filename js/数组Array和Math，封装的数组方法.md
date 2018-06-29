# 数组Array

## 1.创建数组

###1.1.new Array()

```javascript
var arr1 = new Array(); //创建一个空数组
var arr2 = new Array(20); // 创建一个包含20项的数组
var arr3 = new Array("lily","lucy","Tom"); // 创建一个包含3个字符串的数组
```

### 1.2.数组字面量表示法

```javascript
var arr4 = []; //创建一个空数组
var arr5 = [20]; // 创建一个包含1项的数组
var arr6 = ["lily","lucy","Tom"]; // 创建一个包含3个字符串的数组
```

## 2.判断数组 Array.isArray() 

## 3.数组方法

###  join()

1.join(separator): 将数组的元素组起一个字符串，以separator为分隔符，省略的话则用默认用逗号为分隔符，该方法只接收一个参数：即分隔符。 

```javascript
var arr = [1,2,3];
console.log(arr.join()); // 1,2,3
console.log(arr.join("-")); // 1-2-3
console.log(arr); // [1, 2, 3]（原数组不变）
```

2.join()中添加参数为数字或者字符窜

```javascript
  var Arr= [1,2,3]
  var Arr2=Arr.join(1)
  console.log(Arr2);           //11213 
  console.log(typeof Arr2);   //string
```

3.通过join()方法可以实现重复字符串，只需传入字符串以及重复的次数，就能返回重复后的字符串 

```javascript
//重复字符窜
  var str = 123
  function repeatString(str,n) {
    return new Array(n+1).join(str)
  }
  console.log(repeatString(str, 3));  //123123123
```

### **sort()** 

sort()：按升序排列数组项——即最小的值位于最前面，最大的值排在最后面。 

在排序时，sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值， sort()方法比较的也是字符串，因此会出现以下的这种情况： 

```javascript
var arr1 = ["a", "d", "c", "b"];
console.log(arr1.sort()); // ["a", "b", "c", "d"]
arr2 = [13, 24, 51, 3];
console.log(arr2.sort()); // [13, 24, 3, 51]
console.log(arr2); // [13, 24, 3, 51](元数组被改变)
```

为了解决上述问题，sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面。比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回 0，如果第一个参数应该位于第二个之后则返回一个正数。以下就是一个简单的比较函数： 

```javascript
function compare(value1, value2) {
if (value1 < value2) {
return -1;
} else if (value1 > value2) {
return 1;
} else {
return 0;
}
}
arr2 = [13, 24, 51, 3];
console.log(arr2.sort(compare)); // [3, 13, 24, 51]
```



### **reverse()** 

reverse()：反转数组项的顺序。 

```javascript
var arr = [13, 24, 51, 3];
console.log(arr.reverse()); //[3, 51, 24, 13]
console.log(arr); //[3, 51, 24, 13](原数组改变)
```



### **push()和pop()** ,**shift() 和 unshift()** 

都会改变原数组

push(): 可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。 

pop()：数组末尾移除最后一项，减少数组的 length 值，然后返回移除的项。 

```javascript
var arr = ["Lily","lucy","Tom"];
var count = arr.push("Jack","Sean");
console.log(count); // 5
console.log(arr); // ["Lily", "lucy", "Tom", "Jack", "Sean"]
var item = arr.pop();
console.log(item); // Sean
console.log(arr); // ["Lily", "lucy", "Tom", "Jack"]
```

shift()：删除原数组第一项，并返回删除元素的值；如果数组为空则返回undefined 。

unshift:将参数添加到原数组开头，并返回数组的长度 。 

```javascript
var arr = ["Lily","lucy","Tom"];
var count = arr.unshift("Jack","Sean");
console.log(count); // 5
console.log(arr); //["Jack", "Sean", "Lily", "lucy", "Tom"]
var item = arr.shift();
console.log(item); // Jack
console.log(arr); // ["Sean", "Lily", "lucy", "Tom"]
```

### **splice()** ,**slice()** ,split() 

splice 拼接  slice 切割   split分离 

**splice()**：很强大的数组方法，它有很多种用法，可以实现删除、插入和替换。**改变原数组**

**删除**：可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。例如， splice(0,2)会删除数组中的前两项。

**插入**：可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、 0（要删除的项数）和要插入的项。例如，splice(2,0,4,6)会从当前数组的位置 2 开始插入4和6。
**替换**：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,4,6)会删除当前数组位置 2 的项，然后再从位置 2 开始插入4和6。

splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项，如果没有删除任何项，则返回一个空数组。

```javascript
var arr = [1,3,5,7,9,11];
var arrRemoved = arr.splice(0,2);
console.log(arr); //[5, 7, 9, 11]
console.log(arrRemoved); //[1, 3]
var arrRemoved2 = arr.splice(2,0,4,6);
console.log(arr); // [5, 7, 4, 6, 9, 11]
console.log(arrRemoved2); // []
var arrRemoved3 = arr.splice(1,1,2,4);
console.log(arr); // [5, 2, 4, 4, 6, 9, 11]
console.log(arrRemoved3); //[7]
```



**slice()**：返回从原数组中指定开始下标到结束下标之间的项组成的新数组。slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下， slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。

```javascript
var arr = [1,3,5,7,9,11];
var arrCopy = arr.slice(1);
var arrCopy2 = arr.slice(1,4);
var arrCopy3 = arr.slice(1,-2);
var arrCopy4 = arr.slice(-4,-1);
console.log(arr); //[1, 3, 5, 7, 9, 11](原数组没变)
console.log(arrCopy); //[3, 5, 7, 9, 11]
console.log(arrCopy2); //[3, 5, 7]
console.log(arrCopy3); //[3, 5, 7]
console.log(arrCopy4); //[5, 7, 9]
```

arrCopy只设置了一个参数，也就是起始下标为1，所以返回的数组为下标1（包括下标1）开始到数组最后。 
arrCopy2设置了两个参数，返回起始下标（包括1）开始到终止下标（不包括4）的子数组。 
arrCopy3设置了两个参数，终止下标为负数，当出现负数时，将负数加上数组长度的值（6）来替换该位置的数，因此就是从1开始到4（不包括）的子数组。 
arrCopy4中两个参数都是负数，所以都加上数组长度6转换成正数，因此相当于slice(2,5)。



**split()**：方法使用指定的分隔符字符串将一个string对象分割成字符串数组，以将字符串分隔为子字符串，以确定每个拆分的位置。

语法：str.split([separator[, limit]])

参数： separator：可以是一个字符串或正则表达式

​	     limit：一个整数，限定返回的分割片段数量 

Tip: 如果空字符串("")被用作分隔符，则字符串会在每个字符之间分割。 

```javascript
//separator为字符窜
var myString = "Hello World. How are you doing?";
var splits = myString.split(" ");
console.log(splits);    //["Hello", "World.", "How", "are", "you", "doing?"]

// 限定数量
var splits1 = myString.split(" ", 3);
console.log(splits1);   //["Hello", "World.", "How"]

// separator为正则
var names = "Harry Trump ;Fred Barney; Helen Rigby ; Bill Abel ;Chris Hand ";
var re = /\s*;\s*/;
var nameList = names.split(re);
console.log(nameList);
//["Harry Trump", "Fred Barney", "Helen Rigby", "Bill Abel", "Chris Hand "]
```

### find()和findIndex()

find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。
findIndex() 方法，它返回数组中找到的元素的索引，而不是其值。
如果你需要找到一个元素的位置或者一个元素是否存在于数组中，使用Array.prototype.indexOf() 或 Array.prototype.includes()。

```javascript
var array1 = [5, 12, 8, 130, 44];

var found = array1.find(function(element) {
  return element > 10;
});

console.log(found);
// expected output: 12
```

findIndex()方法返回数组中满足提供的测试函数的第一个元素的**索引**。否则返回-1。 

```javascript
var array1 = [5, 12, 8, 130, 44];

function findFirstLargeNumber(element) {
  return element > 13;
}

console.log(array1.findIndex(findFirstLargeNumber));
// expected output: 3
```



### **indexOf()和 lastIndexOf()** 

indexOf()：接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的开头（位置 0）开始向后查找。 
lastIndexOf：接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的末尾开始向前查找。

这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1。在比较第一个参数与数组中的每一项时，会使用全等操作符。

```javascript
var arr = [1,3,5,7,7,5,3,1];
console.log(arr.indexOf(5)); //2
console.log(arr.lastIndexOf(5)); //5
console.log(arr.indexOf(5,2)); //2
console.log(arr.lastIndexOf(5,4)); //2
console.log(arr.indexOf("5")); //-1
```

###includes()

includes()方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。 

```
arr.includes(searchElement, fromIndex)
```

searchElement
需要查找的元素值。
fromIndex 可选
从该索引处开始查找 searchElement。如果为负值，则按升序从 array.length + fromIndex 的索引开始搜索。默认为 0。

**示例：**

```javascript
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true
```



### **forEach()** 

forEach()：对数组进行遍历循环，对数组中的每一项运行给定函数。这个方法没有返回值。参数都是function类型，默认有传参，参数分别为：遍历的数组内容；第对应的数组索引，数组本身。 

```javascript
var arr = [1, 2, 3, 4, 5];
arr.forEach(function(x, index, a){
console.log(x + '|' + index + '|' + (a === arr));
});
// 输出为：
// 1|0|true
// 2|1|true
// 3|2|true
// 4|3|true
// 5|4|true
```



### map()

map()：指“映射”，对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。 

**语法**：

```
let new_array = arr.map(function callback(currentValue, index, array) { 
    // Return element for new_array 
}[, thisArg])
```

**参数**：

currentValue  callback 的第一个参数，数组中正在处理的当前元素。
index 		 callback 的第二个参数，数组中正在处理的当前元素的索引。
array 		 callback 的第三个参数，map 方法被调用的数组。
thisArg 	         可选的。执行 callback 函数时 使用的this 值。

```javascript
  var arr = [1,2,3,4,5]
  var arr1= arr.map(item => {
    return item*item
  })
  console.log(arr1);

// 使用 map 重新格式化数组中的对象
  var kvArray = [{key: 1, value: 10}, {key: 2, value: 20}, {key: 3, value: 30}];
  var newKvArray = kvArray.map(item => {
    var newobj = {}
    // 设置新的键值对
    newobj[item.key] = item.value
    return newobj
  })
  console.log(newKvArray);
```



### **filter()** 

filter()：“过滤”功能，数组中的每一项运行给定函数，返回满足过滤条件组成的数组。在回调中返回true表示保留该元素（通过测试），false则不保留。

```javascript
//删选复合要求的数字
  function BigTenNum(ele) {
    return ele >= 10
  }
  var numArr = [2,8,12,15,17]
  //直接传入函数
  var NumArr = numArr.filter(BigTenNum)
  console.log(NumArr); // [12, 15, 17]
	
  //直接写入回调函数中
  numArr.filter(item => {
    return item > 10
  });  // [12, 15, 17]
```

 **过滤json数据 得到id是不为0的数字的对象组成的数组** 

```javascript
//过滤json数据 得到id是不为0的数字的对象
  var jsonArr = [
    { id: 15 },
    { id: -1 },
    { id: 0 },
    { id: 3 },
    { id: 12.2 },
    { },
    { id: null },
    { id: NaN },
    { id: 'undefined' }
  ];
  function isNum(obj) {
    return  obj !== undefined && typeof obj === "number" && !isNaN(obj)
  }
  var trueIndex = 0
  var falseIndex = 0
  function idArr(item) {
    if(isNum(item.id) && item.id !==0){
      trueIndex++
        return true
    }
    falseIndex++
    return false
  }
  console.log(jsonArr.filter(idArr),trueIndex,falseIndex); 
  // [{ id: 15 }, { id: -1 }, { id: 3 }, { id: 12.2 }]  4   5
```

**Searching in array**

```javascript
  //Searching in array
  var fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];
  var searchStr = (query) => {
    return fruits.filter((el) => {
      return el.toLowerCase().indexOf(query.toLowerCase()) > -1
    })
  }
  searchStr('o') // ["mango", "orange"]
```



### **reduce()和 reduceRight()** 

这两个方法都会实现迭代数组的所有项，然后构建一个最终返回的值。reduce()方法从数组的第一项开始，逐个遍历到最后。而 reduceRight()则从数组的最后一项开始，向前遍历到第一项。 

**reduce()** 方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。 

**语法**：

```
arr.reduce(callback, initialValue)
```

**参数**：

执行数组中每个值的函数，包含四个参数：
accumulator 累加器累加回调的返回值; 
			它是上一次调用回调时返回的累积值，或initialValue（如下所示）。

currentValue 	 数组中正在处理的元素。
currentIndex可选 数组中正在处理的当前元素的索引。 
				 如果提供了initialValue，则索引号为0，否则为索引为1。
array可选 		 调用reduce的数组
initialValue可选	用作第一个调用 callback的第一个参数的值。 
					如果没有提供初始值，则将使用数组中的第一个元素。
					在没有初始值的空数组上调用 reduce 将报错。

例子：

**数组值求和**

```javascript
  var reduceArr = [1,2,3,4,5]
  //不加初始值
  reduceArr.reduce((prev, curr) => {
    return prev + curr
  }) //15
  //加初始值
  reduceArr.reduce((prev, curr) => {
    return prev + curr
  }，3) //18
```

**将二维数组转化为一维数组**

```javascript
  var flattened = [[0, 1], [2, 3], [4, 5]]
  var newFlattened = flattened.reduce((prev,curr) => {
    return prev.concat(curr)
  })
  console.log(newFlattened); //[0, 1, 2, 3, 4, 5]
```

**统计名字出现次数**

```javascript
  var nameArr=['xiaoming','hong','xiaoming']
  var namesObj=nameArr.reduce((names,name) => {
    if(name in names) {
      names[name]++
    }else {
      names[name] = 1
    }
    return names
  },{})
  console.log(namesObj);
```

**绑定包含在对象中数组的数组**

```javascript
var friends = [{
    name: 'Anna',
    books: ['Bible', 'Harry Potter'],
    age: 21
  }, {
    name: 'Bob',
    books: ['War and peace', 'Romeo and Juliet'],
    age: 26
  }, {
    name: 'Alice',
    books: ['The Lord of the Rings', 'The Shining'],
    age: 18
  }];
  friends.reduce((prev, curr) => {
    return prev.concat(curr.books)
    //使用三点运算符
    // [...prev, ...curr.books]
  }, [])
  //["Bible", "Harry Potter", "War and peace", "Romeo and Juliet", "The Lord of the Rings", "The Shining"]
```



### **every()** 

every()：判断数组中每一项都是否满足条件，只有所有项都满足条件，才会返回true。 

```javascript
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.every(function(x) {
return x < 10;
}); 
console.log(arr2); //true
var arr3 = arr.every(function(x) {
return x < 3;
}); 
console.log(arr3); // false
```



### **some()** 

some()：判断数组中是否存在满足条件的项，只要有一项满足条件，就会返回true。 

```javascript
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.some(function(x) {
return x < 3;
}); 
console.log(arr2); //true
var arr3 = arr.some(function(x) {
return x < 1;
}); 
console.log(arr3); // false
```



## 4.Array.from()

Array.from()方法从一个类似数组或可迭代对象中创建一个新的数组实例。

**示例** 

**Array from a String**

```
Array.from('foo'); 
// ["f", "o", "o"]
```

**Array from a Set**

```
let s = new Set(['foo', window]); 
Array.from(s); 
// ["foo", window]
```

**Array from a Map**

```javascript
let m = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(m); 
// [[1, 2], [2, 4], [4, 8]]
```

**Array from an Array-like object (arguments)**

```javascript
function f() {
  return Array.from(arguments);
}

f(1, 2, 3);

// [1, 2, 3]
```

**Using arrow functions and Array.from**

```javascript
// Using an arrow function as the map function to
// manipulate the elements
Array.from([1, 2, 3], x => x + x);      
// [2, 4, 6]


// Generate a sequence of numbers
// Since the array is initialized with `undefined` on each position,
// the value of `v` below will be `undefined`
Array.from({length: 5}, (v, i) => i);
// [0, 1, 2, 3, 4]
```

**数组去重合并**

```javascript
//数组去重合并
  function combine() {
    var arr= Array.from(arguments).reduce((prev,curr) => {
      return prev.concat(curr)
    })
    return   [...new Set(arr)]
  }
  console.log(combine([1], [2,2,3],[2,3,4,5])); //[1, 2, 3, 4, 5]
```



# Math

##Math.abs(x)

Math.abs(x) 函数返回指定数字 “x“ 的绝对值。 

## Math.random()

返回一个浮点型伪随机数字在0（包括0）和1（不包括）之间 

## Math.max()

**返回值**：返回给定的一组数字中的最大值。如果给定的参数中至少有一个参数无法被转换成数字，则会返回 NAN

```javascript
//代码实例  

  //todo 第一种 Math.max()
  Math.max(1, 2, 3) //3

  //todo 第二种  三点运算符
  var arr = [1,2,3]
  Math.max(...arr) //3

  //todo 第三种 创建getMaxOfArray()方法
  function getMaxOfArray(arr) {
    return Math.max.apply(null,arr)
  }
  getMaxOfArray(arr) //3
```

##Math.min() 

Math.min() 返回零个或更多个数值的最小值。

##Math.pow(x,y)

Math.pow(x,y) 函数返回基数（x）的指数（y）次幂

##Math.round()

Math.round() 函数返回一个数字四舍五入后最接近的整数。 

## Math.ceil() ===向上取整

```
Math.ceil(7.004);  // 8
```



## Math.floor() ===向下取整

```
Math.floor( 45.95);  // 45 
```







# 封装的数组方法（简单实现，没优化）

##chunk(arr,size)

将数组（array）拆分成多个 size长度的区块，并将这些区块组成一个新数组。 如果array无法被分割成全部等长的区块，那么最后剩余的元素将组成一个区块。 

```javascript
//todo 将数组按照size的个数切开，比如三个三个的切
var arr= [1,2,3,4,5,6]
function chunk(arr,size) {
  var arr1 = Object.assign([],arr)
  var num = Math.ceil(arr1.length/size)
  var result= new Array(num)
  for (var i = 0; i < num; i++) {
    result[i] = arr1.splice(0,size)

  }
  return result
}
console.log(chunk(arr, 4));  // [1, 2, 3, 4],[5, 6]
console.log(arr)
```



# 参考文章

[JavaScript数组方法大全(推荐)](https://www.jb51.net/article/87930.htm)

[mdn—Math方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

[mdn—Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
#变量的解构赋值
理解：从对象或数组中提取数据, 并赋值给变量(多个)
## 1数组的解构赋值
	let [a,b,...arr1]= [1,2,3,4,5]
	a//1
	b//2
	arr1//[3,4,5]
###默认值
ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。

	let [x = 1] = [undefined];
	x // 1
	let [x = 1] = [null];
	x // null null不严格等于undefined。
	let [x = 1, y = 'b'] = ['a', undefined]; //x='a',y='b'
**如果默认值是一个表达式**，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。

	function f() {
 	 console.log('aaa');
	}

	let [x = f()] = [1];
上面代码中，因为x能取到值，所以函数f根本不会执行。上面的代码其实等价于下面的代码。

	let x;
	if ([1][0] === undefined) {
  		x = f();
		} else {
  		x = [1][0];
		}
## 2对象的解构赋值
对象的解构与数组有一个重要的不同。**数组**的元素是按次序排列的，变量的取值由它的**位置**决定；而 **对象**的属性没有次序，变量必须与**属性名**同名，才能取到正确的值。

	let { bar, foo } = { foo: "aaa", bar: "bbb" };
	foo // "aaa"
	bar // "bbb"

	let { baz } = { foo: "aaa", bar: "bbb" };
	baz // undefined
如果变量名与属性名不一致，必须写成下面这样。

	let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
	baz // "aaa"

	let obj = { first: 'hello', last: 'world' };
	let { first: f, last: l } = obj;
	f // 'hello'
	l // 'world'
这实际上说明，对象的解构赋值是下面形式的简写（参见《对象的扩展》一章）。

	let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
也就是说，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

	let { foo: baz } = { foo: "aaa", bar: "bbb" };
	baz // "aaa"
	foo // error: foo is not defined
上面代码中，foo是匹配的模式，baz才是变量。真正被赋值的是变量baz，而不是模式foo。
####对象的解构也可以指定默认值。<br />默认值生效的条件是，对象的属性值严格等于undefined。

	var {x = 3} = {};
	x // 3

	var {x, y = 5} = {x: 1};
	x // 1
	y // 5

	var {x: y = 3} = {};
	y // 3

	var {x: y = 3} = {x: 5};
	y // 5

	var { message: msg = 'Something went wrong' } = {};
	msg // "Something went wrong"
## 3函数参数的解构赋值
	function move ({x=0,y=0} = {}) {
    	return [x,y]
  	}
  	move({x: 1,y: 2}) 
  	console.log(move({x: 1,y: 2})) //[1, 2]
  	console.log(move({x: 1})) //[1, 0]
  	function move1 ({x,y} = {x:0,y:0}) {
    	return [x,y]
  	}
  	console.log(move1({x: 1})) // [1, undefined]
函数move的参数是一个对象，通过对这个对象进行解构，得到变量x和y的值。如果解构失败，x和y等于默认值。<br />
函数move1是为参数指定默认值，而不是为变量x和y指定默认值，所以会得到与前一种写法不同的结果。

## 4用途
变量的解构赋值用途很多。

（1）交换变量的值

	let x = 1;
	let y = 2;

	[x, y] = [y, x];
上面代码交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。

（2）从函数返回多个值

函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

	// 返回一个数组

	function example() {
  	return [1, 2, 3];
	}
	let [a, b, c] = example();

	// 返回一个对象

	function example() {
  	return {
    	foo: 1,
    	bar: 2
  	};
	}
	let { foo, bar } = example();
（3）函数参数的定义

解构赋值可以方便地将一组参数与变量名对应起来。

	// 参数是一组有次序的值
	function f([x, y, z]) { ... }
	f([1, 2, 3]);

	// 参数是一组无次序的值
	function f({x, y, z}) { ... }
	f({z: 3, y: 2, x: 1});
（4）提取 JSON 数据

解构赋值对提取 JSON 对象中的数据，尤其有用。

	let jsonData = {
  	id: 42,
  	status: "OK",
  	data: [867, 5309]
	};

	let { id, status, data: number } = jsonData;

	console.log(id, status, number);
	// 42, "OK", [867, 5309]
上面代码可以快速提取 JSON 数据的值。

（5）函数参数的默认值

	jQuery.ajax = function (url, {
	  async = true,
	  beforeSend = function () {},
	  cache = true,
	  complete = function () {},
	  crossDomain = false,
	  global = true,
	  // ... more config
	} = {}) {
	  // ... do stuff
	};
指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。

（6）遍历 Map 结构

任何部署了 Iterator 接口的对象，都可以用for...of循环遍历。Map 结构原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便。

	const map = new Map();
	map.set('first', 'hello');
	map.set('second', 'world');
	
	for (let [key, value] of map) {
	  console.log(key + " is " + value);
	}
	// first is hello
	// second is world
如果只想获取键名，或者只想获取键值，可以写成下面这样。

	// 获取键名
	for (let [key] of map) {
	  // ...
	}
	
	// 获取键值
	for (let [,value] of map) {
	  // ...
	}
（7）输入模块的指定方法

加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

	const { SourceMapConsumer, SourceNode } = require("source-map");
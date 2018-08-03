#1Set
##基本用法
 Set是ES6 新的数据结构。类似于数组，成员的值唯一。<br />
Set本身是一个构造函数，用来生成 Set 数据结构。

	const s = new Set();
	
	[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
	
	for (let i of s) {
	  console.log(i);
	}
	// 2 3 5 4
去除数组重复成员的方法。

	// 去除数组的重复成员
	[...new Set(array)]
##Set内部值的判断
**1**Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（===），主要的区别是NaN等于自身，而精确相等运算符认为NaN不等于自身。

	let set = new Set();
	let a = NaN;
	let b = NaN;
	set.add(a);
	set.add(b);
	set // Set {NaN}
**2**两个对象总是不相等的。
	
	let set = new Set();
	
	set.add({});
	set.size // 1
	
	set.add({});
	set.size // 2
##Set 实例的属性和方法
###set实例的属性
Set.prototype.constructor：构造函数，默认就是Set函数。<br />
Set.prototype.size：返回Set实例的成员总数。
###set实例的方法
Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。<br />
**操作方法。**

add(value)：添加某个值，返回 Set 结构本身。<br />
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。<br />
has(value)：返回一个布尔值，表示该值是否为Set的成员。<br />
clear()：清除所有成员，没有返回值。

####Array.from方法可以将 Set 结构转为数组。

	const items = new Set([1, 2, 3, 4, 5]);
	const array = Array.from(items);
#####这就提供了去除数组重复成员的另一种方法。

	function dedupe(array) {
	  return Array.from(new Set(array));
	}
	
	dedupe([1, 1, 2, 3]) // [1, 2, 3]
**遍历方法。**<br />
Set 结构的实例有四个遍历方法，可以用于遍历成员。<br />

keys()：返回键名的遍历器<br />
values()：返回键值的遍历器<br />
entries()：返回键值对的遍历器<br />
forEach()：使用回调函数遍历每个成员<br />
（1）keys()，values()，entries()

keys方法、values方法、entries方法返回的都是遍历器对象。
由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法的行为完全一致。

	let set = new Set(['red', 'green', 'blue']);
	
	for (let item of set.keys()) {
	  console.log(item);
	}
	// red
	// green
	// blue
	
	for (let item of set.values()) {
	  console.log(item);
	}
	// red
	// green
	// blue
	
	for (let item of set.entries()) {
	  console.log(item);
	}
	// ["red", "red"]
	// ["green", "green"]
	// ["blue", "blue"]
Set 结构的实例默认可遍历，直接用for...of循环遍历 Set。

	let set = new Set(['red', 'green', 'blue']);
	
	for (let x of set) {
	  console.log(x);
	}
	// red
	// green
	// blue
##遍历的应用
扩展运算符（...）内部使用for...of循环，所以也可以用于 Set 结构。

	let set = new Set(['red', 'green', 'blue']);
	let arr = [...set];
	// ['red', 'green', 'blue']
扩展运算符和 Set 结构相结合，就可以去除数组的重复成员。

	let arr = [3, 5, 2, 2, 5, 5];
	let unique = [...new Set(arr)];
	// [3, 5, 2]
而且，数组的map和filter方法也可以间接用于 Set 了。

	let set = new Set([1, 2, 3]);
	set = new Set([...set].map(x => x * 2));
	// 返回Set结构：{2, 4, 6}
	
	let set = new Set([1, 2, 3, 4, 5]);
	set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
因此使用 Set 可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）。

	let a = new Set([1, 2, 3]);
	let b = new Set([4, 3, 2]);
	
	// 并集
	let union = new Set([...a, ...b]);
	// Set {1, 2, 3, 4}
	
	// 交集
	let intersect = new Set([...a].filter(x => b.has(x)));
	// set {2, 3}
	
	// 差集
	let difference = new Set([...a].filter(x => !b.has(x)));
	// Set {1}
如果想在遍历操作中，同步改变原来的 Set 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用Array.from方法。

	// 方法一
	let set = new Set([1, 2, 3]);
	set = new Set([...set].map(val => val * 2));
	// set的值是2, 4, 6
	
	// 方法二
	let set = new Set([1, 2, 3]);
	set = new Set(Array.from(set, val => val * 2));
	// set的值是2, 4, 6
上面代码提供了两种方法，直接在遍历操作中改变原来的 Set 结构。

#2Map
##含义和基本用法
JavaScript 的对象（Object），Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应<br />
###Map构造函数的参数
**数组，Set和Map都可以用来生成新的 Map。**<br />
不仅仅是数组，任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构（详见《Iterator》一章）都可以当作Map构造函数的参数
	
	const set = new Set([
	  ['foo', 1],
	  ['bar', 2]
	]);
	const m1 = new Map(set);
	m1.get('foo') // 1
	
	const m2 = new Map([['baz', 3]]);
	const m3 = new Map(m2);
	m3.get('baz') // 3

###实例的属性和操作方法
####属性
（1）size 属性
（2）set(key, value)
（3）get(key)
（4）has(key)
（5）delete(key)
（6）clear()
####遍历方法
keys()：返回键名的遍历器。<br />
values()：返回键值的遍历器。<br />
entries()：返回所有成员的遍历器。<br />
forEach()：遍历 Map 的所有成员。<br />
#####Map 结构转为数组结构，比较快速的方法是使用扩展运算符（...）。

	const map = new Map([
	  [1, 'one'],
	  [2, 'two'],
	  [3, 'three'],
	]);
	
	[...map.keys()]
	// [1, 2, 3]
	
	[...map.values()]
	// ['one', 'two', 'three']
	
	[...map.entries()]
	// [[1,'one'], [2, 'two'], [3, 'three']]
	
	[...map]
	// [[1,'one'], [2, 'two'], [3, 'three']]
结合数组的map方法、filter方法，可以实现 Map 的遍历和过滤（Map 本身没有map和filter方法）。

	const map0 = new Map()
	  .set(1, 'a')
	  .set(2, 'b')
	  .set(3, 'c');
	
	const map1 = new Map(
	  [...map0].filter(([k, v]) => k < 3)
	);
	// 产生 Map 结构 {1 => 'a', 2 => 'b'}
	
	const map2 = new Map(
	  [...map0].map(([k, v]) => [k * 2, '_' + v])
	    );
	// 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
此外，Map 还有一个forEach方法，与数组的forEach方法类似，也可以实现遍历。

	map.forEach(function(value, key, map) {
	  console.log("Key: %s, Value: %s", key, value);
	});
forEach方法还可以接受第二个参数，用来绑定this。
	
	const reporter = {
	  report: function(key, value) {
	    console.log("Key: %s, Value: %s", key, value);
	  }
	};
	
	map.forEach(function(value, key, map) {
	  this.report(key, value);
	}, reporter);
上面代码中，forEach方法的回调函数的this，就指向reporter。

##与其他数据结构的互相转换
###（1）Map 转为数组

前面已经提过，Map 转为数组最方便的方法，就是使用扩展运算符（...）。

	const myMap = new Map()
	  .set(true, 7)
	  .set({foo: 3}, ['abc']);
	[...myMap]
	// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
###（2）数组 转为 Map

将数组传入 Map 构造函数，就可以转为 Map。

	new Map([
	  [true, 7],
	  [{foo: 3}, ['abc']]
	])
	// Map {
	//   true => 7,
	//   Object {foo: 3} => ['abc']
	// }
###（3）Map 转为对象

如果所有 Map 的键都是字符串，它可以无损地转为对象。

	function strMapToObj(strMap) {
	  let obj = Object.create(null);
	  for (let [k,v] of strMap) {
	    obj[k] = v;
	  }
	  return obj;
	}
	
	const myMap = new Map()
	  .set('yes', true)
	  .set('no', false);
	strMapToObj(myMap)
	// { yes: true, no: false }
如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

###（4）对象转为 Map

	function objToStrMap(obj) {
	  let strMap = new Map();
	  for (let k of Object.keys(obj)) {
	    strMap.set(k, obj[k]);
	  }
	  return strMap;
	}
	
	objToStrMap({yes: true, no: false})
	// Map {"yes" => true, "no" => false}
###（5）Map 转为 JSON

Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

	function strMapToJson(strMap) {
	  return JSON.stringify(strMapToObj(strMap));
	}
	
	let myMap = new Map().set('yes', true).set('no', false);
	strMapToJson(myMap)
	// '{"yes":true,"no":false}'
另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。

	function mapToArrayJson(map) {
	  return JSON.stringify([...map]);
	}
	
	let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
	mapToArrayJson(myMap)
	// '[[true,7],[{"foo":3},["abc"]]]'
###（6）JSON 转为 Map

JSON 转为 Map，正常情况下，所有键名都是字符串。
	
	function jsonToStrMap(jsonStr) {
	  return objToStrMap(JSON.parse(jsonStr));
	}
	
	jsonToStrMap('{"yes": true, "no": false}')
	// Map {'yes' => true, 'no' => false}
但是，有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作。

	function jsonToMap(jsonStr) {
	  return new Map(JSON.parse(jsonStr));
	}
	
	jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
	// Map {true => 7, Object {foo: 3} => ['abc']}
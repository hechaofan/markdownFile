# ECMAScript 6
# let和const命令
## 1let命令
###基本用法
1let所声明的变量，只在let命令所在的代码块内有效。<br />
2for循环的计数器，就很合适使用let命令。<br />
3不存在变量提升，var的变量可以提升<br />
4暂时性死区<br />
&emsp;&emsp;如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。<br />
&emsp;&emsp;<font color=red>在代码块内，使用let命令声明变量之前，该变量都是不可用的。</font>这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。<br />
&emsp;&emsp;<font color=red>“暂时性死区”也意味着typeof不再是一个百分之百安全的操作。</font><br />
5不允许重复声明<br />
&emsp;&emsp;let不允许在**相同作用域内**，重复声明同一个变量。


## 2块级作用域
###ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景
1内层变量可能会覆盖外层变量<br />
2用来计数的循环变量泄露为全局变量<br />
####let实际上为 JavaScript 新增了块级作用域。解决这些问题。<br />ES6 允许块级作用域的任意嵌套<br />外层作用域无法读取内层作用域的变量。<br />内层作用域可以定义外层作用域的同名变量。

## 3const 命令
###本质
const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动<br />
	
	const foo = {};

	// 为 foo 添加一个属性，可以成功
	foo.prop = 123;
	foo.prop // 123

	// 将 foo 指向另一个对象，就会报错
	foo = {}; // TypeError: "foo" is read-only
    
一个将对象彻底冻结的函数
	
	var constantize = (obj) => {
  		Object.freeze(obj);
  		Object.keys(obj).forEach( (key, i) => {
    		if ( typeof obj[key] === 'object' ) {
      		constantize( obj[key] );
    		}
  		});
		};

##ES6 一共有 6 种声明变量的方法
ES5 只有两种声明变量的方法：var命令和function命令<br />
ES6 let和const命令，import命令和class命令

##顶层对象的属性 
###ES6 为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；<br />另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
	var a = 1;
	// 如果在 Node 的 REPL 环境，可以写成 global.a
	// 或者采用通用方法，写成 this.a
	window.a // 1

	let b = 1;
	window.b // undefined
#ES6 新特性

# 字符串
 - 字符串的遍历器接口

        for(let codePoint of 'foo'){
           console.log(codePoint)
         }
		//"f"
		//"0"
        //"0"
 
## includes(), startsWith(), endsWith()
- 传统上，JavaScript只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了三种新方法。

   - includes()：返回布尔值，表示是否找到了参数字符串。
   - startsWith()：返回布尔值，表示参数字符串是否在源字符串的头部。
   - endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部。
			var s = 'Hello world!';
			
			s.startsWith('Hello') // true
			s.endsWith('!') // true
			s.includes('o') // true
   这三个方法都支持第二个参数，表示开始搜索的位置。

			var s = 'Hello world!';
			
			s.startsWith('world', 6) // true
			s.endsWith('Hello', 5) // true
			s.includes('Hello', 6) // false
上面代码表示，使用第二个参数n时，endsWith的行为与其他两个方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束。

## repeat()
 - repeat方法返回一个新字符串，表示将原字符串重复n次。

		'x'.repeat(3) // "xxx"
		'hello'.repeat(2) // "hellohello"
		'na'.repeat(0) // ""
参数如果是小数，会被取整。

 		'na'.repeat(2.9) // "nana"
如果repeat的参数是负数或者Infinity，会报错。

		'na'.repeat(Infinity)
		// RangeError
		'na'.repeat(-1)
		// RangeError
但是，如果参数是0到-1之间的小数，则等同于0，这是因为会先进行取整运算。0到-1之间的小数，取整以后等于-0，repeat视同为0。

		'na'.repeat(-0.9) // ""
参数NaN等同于0。

		'na'.repeat(NaN) // ""
如果repeat的参数是字符串，则会先转换成数字。

		'na'.repeat('na') // ""
		'na'.repeat('3') // "nanana"

##模板字符串
 - 传统的JavaScript语言，输出模板通常是这样写的。

		$('#result').append(
		  'There are <b>' + basket.count + '</b> ' +
		  'items in your basket, ' +
		  '<em>' + basket.onSale +
		  '</em> are on sale!'
		);
上面这种写法相当繁琐不方便，ES6引入了模板字符串解决这个问题。

		$('#result').append(`
		  There are <b>${basket.count}</b> items
		   in your basket, <em>${basket.onSale}</em>
		  are on sale!
		`);
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

		// 普通字符串
		`In JavaScript '\n' is a line-feed.`
		
		// 多行字符串
		`In JavaScript this is
		 not legal.`
		
		console.log(`string text line 1
		string text line 2`);
		
		// 字符串中嵌入变量
		var name = "Bob", time = "today";
		`Hello ${name}, how are you ${time}?`


# 数值 Number.isFinite(), Number.isNaN()
 - ES6在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法。
  - Number.isFinite()用来检查一个数值是否为有限的（finite）。

			Number.isFinite(15); // true
			Number.isFinite(0.8); // true
			Number.isFinite(NaN); // false
			Number.isFinite(Infinity); // false
			Number.isFinite(-Infinity); // false
			Number.isFinite('foo'); // false
			Number.isFinite('15'); // false
			Number.isFinite(true); // false
ES5可以通过下面的代码，部署Number.isFinite方法。

			(function (global) {
			  var global_isFinite = global.isFinite;
			
			  Object.defineProperty(Number, 'isFinite', {
			    value: function isFinite(value) {
			      return typeof value === 'number' && global_isFinite(value);
			    },
			    configurable: true,
			    enumerable: false,
			    writable: true
			  });
			})(this);
Number.isNaN()用来检查一个值是否为NaN。

			Number.isNaN(NaN) // true
			Number.isNaN(15) // false
			Number.isNaN('15') // false
			Number.isNaN(true) // false
			Number.isNaN(9/NaN) // true
			Number.isNaN('true'/0) // true
			Number.isNaN('true'/'true') // true
ES5通过下面的代码，部署Number.isNaN()。

			(function (global) {
			  var global_isNaN = global.isNaN;
			
			  Object.defineProperty(Number, 'isNaN', {
			    value: function isNaN(value) {
			      return typeof value === 'number' && global_isNaN(value);
			    },
			    configurable: true,
			    enumerable: false,
			    writable: true
			  });
			})(this);
它们与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，非数值一律返回false。

			isFinite(25) // true
			isFinite("25") // true
			Number.isFinite(25) // true
			Number.isFinite("25") // false
			
			isNaN(NaN) // true
			isNaN("NaN") // true
			Number.isNaN(NaN) // true
			Number.isNaN("NaN") // false


  
# 数组的扩展
#Array.from
Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。

  - 下面是一个类似数组的对象，Array.from将它转为真正的数组。

			let arrayLike = {
			    '0': 'a',
			    '1': 'b',
			    '2': 'c',
			    length: 3
			};
			
			// ES5的写法
			var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
			
			// ES6的写法
			let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

 - Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

		Array.from(arrayLike, x => x * x);
		// 等同于
		Array.from(arrayLike).map(x => x * x);
		
		Array.from([1, 2, 3], (x) => x * x)
		// [1, 4, 9]

#Array.of()
 - Array.of方法用于将一组值，转换为数组。

		Array.of(3, 11, 8) // [3,11,8]
		Array.of(3) // [3]
		Array.of(3).length // 1
这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。

		Array() // []
		Array(3) // [, , ,]
		Array(3, 11, 8) // [3, 11, 8]
上面代码中，Array方法没有参数、一个参数、三个参数时，返回结果都不一样。只有当参数个数不少于2个时，Array()才会返回由参数组成的新数组。参数个数只有一个时，实际上是指定数组的长度。

 - Array.of基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一。

		Array.of() // []
		Array.of(undefined) // [undefined]
		Array.of(1) // [1]
		Array.of(1, 2) // [1, 2]
		Array.of总是返回参数值组成的数组。如果没有参数，就返回一个空数组。
		
		Array.of方法可以用下面的代码模拟实现。
		
		function ArrayOf(){
		  return [].slice.call(arguments);
		}

# 数组实例的find()和findIndex()
 - 数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。

		[1, 4, -5, 10].find((n) => n < 0)
		// -5
上面代码找出数组中第一个小于0的成员。

		[1, 5, 10, 15].find(function(value, index, arr) {
		  return value > 9;
		}) // 10
上面代码中，find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

 - 数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

		[1, 5, 10, 15].findIndex(function(value, index, arr) {
		  return value > 9;
		}) // 2
这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。

 - 另外，这两个方法都可以发现NaN，弥补了数组的IndexOf方法的不足。

		[NaN].indexOf(NaN)
		// -1
		
		[NaN].findIndex(y => Object.is(NaN, y))
		// 0
上面代码中，indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到。


# 数组实例的fill()

 - fill方法使用给定值，填充一个数组。
		
		['a', 'b', 'c'].fill(7)
		// [7, 7, 7]
		
		new Array(3).fill(7)
		// [7, 7, 7]
上面代码表明，fill方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。

 - fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
		
		['a', 'b', 'c'].fill(7, 1, 2)
		// ['a', 7, 'c']
上面代码表示，fill方法从1号位开始，向原数组填充7，到2号位之前结束。


#数组实例的entries()，keys()和values()
 - ES6提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。

		for (let index of ['a', 'b'].keys()) {
		  console.log(index);
		}
		// 0
		// 1
		
		for (let elem of ['a', 'b'].values()) {
		  console.log(elem);
		}
		// 'a'
		// 'b'
		
		for (let [index, elem] of ['a', 'b'].entries()) {
		  console.log(index, elem);
		}
		// 0 "a"
		// 1 "b"
如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历。

		let letter = ['a', 'b', 'c'];
		let entries = letter.entries();
		console.log(entries.next().value); // [0, 'a']
		console.log(entries.next().value); // [1, 'b']
		console.log(entries.next().value); // [2, 'c']


# 数组实例的includes()
 - Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。该方法属于ES7，但Babel转码器已经支持。
 
		[1, 2, 3].includes(2);     // true
		[1, 2, 3].includes(4);     // false
		[1, 2, NaN].includes(NaN); // true
该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。

		[1, 2, 3].includes(3, 3);  // false
		[1, 2, 3].includes(3, -1); // true
没有该方法之前，我们通常使用数组的indexOf方法，检查是否包含某个值。

		if (arr.indexOf(el) !== -1) {
		  // ...
		}
indexOf方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是，它内部使用严格相当运算符（===）进行判断，这会导致对NaN的误判。

		[NaN].indexOf(NaN)
		// -1
includes使用的是不一样的判断算法，就没有这个问题。

		[NaN].includes(NaN)
		// true
下面代码用来检查当前环境是否支持该方法，如果不支持，部署一个简易的替代版本。

		const contains = (() =>
		  Array.prototype.includes
		    ? (arr, value) => arr.includes(value)
		    : (arr, value) => arr.some(el => el === value)
		)();
		contains(["foo", "bar"], "baz"); // => false

# 函数的扩展
 - 函数参数的默认值
  - 基本用法在ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法。

			function log(x, y) {
			  y = y || 'World';
			  console.log(x, y);
			}
			
			log('Hello') // Hello World
			log('Hello', 'China') // Hello China
			log('Hello', '') // Hello World

        上面代码检查函数log的参数y有没有赋值，如果没有，则指定默认值为World。这种写法的缺点在于，如果参数y赋值了，但是对应的布尔值为false，则该赋值不起作用。就像上面代码的最后一行，参数y等于空字符，结果被改为默认值。

        为了避免这个问题，通常需要先判断一下参数y是否被赋值，如果没有，再等于默认值。
		
			if (typeof y === 'undefined') {
			  y = 'World';
			}

  - ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。

			function log(x, y = 'World') {
			  console.log(x, y);
			}
			
			log('Hello') // Hello World
			log('Hello', 'China') // Hello China
			log('Hello', '') // Hello

  - 可以看到，ES6的写法比ES5简洁许多，而且非常自然。下面是另一个例子。
  
			function Point(x = 0, y = 0) {
			  this.x = x;
			  this.y = y;
			}
			
			var p = new Point();
			p // { x: 0, y: 0 }
  - 除了简洁，ES6的写法还有两个好处：首先，阅读代码的人，可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档；其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。
      - 参数变量是默认声明的，所以不能用let或const再次声明。
				
				function foo(x = 5) {
				  let x = 1; // error
				  const x = 2; // error
				}
上面代码中，参数变量x是默认声明的，在函数体中，不能用let或const再次声明，否则会报错。

  - 与解构赋值默认值结合使用
  
			function foo({x, y = 5}) {
			  console.log(x, y);
			}
			
			foo({}) // undefined, 5
			foo({x: 1}) // 1, 5
			foo({x: 1, y: 2}) // 1, 2
			foo() // TypeError: Cannot read property 'x' of undefined
上面代码使用了对象的解构赋值默认值，而没有使用函数参数的默认值。只有当函数foo的参数是一个对象时，变量x和y才会通过解构赋值而生成。如果函数foo调用时参数不是对象，变量x和y就不会生成，从而报错。如果参数对象没有y属性，y的默认值5才会生效。
	- 下面是另一个对象的解构赋值默认值的例子。
	
			function fetch(url, { body = '', method = 'GET', headers = {} }) {
			  console.log(method);
			}
			
			fetch('http://example.com', {})
			// "GET"	
			
			fetch('http://example.com')
			// 报错
上面代码中，如果函数fetch的第二个参数是一个对象，就可以为它的三个属性设置默认值。上面的写法不能省略第二个参数，如果结合函数参数的默认值，就可以省略第二个参数。这时，就出现了双重默认值。

			function fetch(url, { method = 'GET' } = {}) {
			  console.log(method);
			}
				
			fetch('http://example.com')
			// "GET"
上面代码中，函数fetch没有第二个参数时，函数参数的默认值就会生效，然后才是解构赋值的默认值生效，变量method才会取到默认值GET。

 - 参数默认值的位置
  
   - 通常情况下，定义了默认值的参数，应该是函数的尾参数。因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。
			
			// 例一
			function f(x = 1, y) {
			  return [x, y];
			}
			
			f() // [1, undefined]
			f(2) // [2, undefined])
			f(, 1) // 报错
			f(undefined, 1) // [1, 1]
			
			// 例二
			function f(x, y = 5, z) {
			  return [x, y, z];
			}
			
			f() // [undefined, 5, undefined]
			f(1) // [1, 5, undefined]
			f(1, ,2) // 报错
			f(1, undefined, 2) // [1, 5, 2]
上面代码中，有默认值的参数都不是尾参数。这时，无法只省略该参数，而不省略它后面的参数，除非显式输入undefined。

    - 如果传入undefined，将触发该参数等于默认值，null则没有这个效果。

			function foo(x = 5, y = 6) {
			  console.log(x, y);
			}
			
			foo(undefined, null)
			// 5 null
上面代码中，x参数对应undefined，结果触发了默认值，y参数等于null，就没有触发默认值。

# 对象的扩展
- ES6允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。
 
		var foo = 'bar';
		var baz = {foo};
		baz // {foo: "bar"}
		
		// 等同于
		var baz = {foo: foo};

		
- 上面代码表明，ES6允许在对象之中，只写属性名，不写属性值。这时，属性值等于属性名所代表的变量。下面是另一个例子
		function f(x, y) {
		  return {x, y};
		}
		
		// 等同于
		
		function f(x, y) {
		  return {x: x, y: y};
		}
		
		f(1, 2) // Object {x: 1, y: 2}

- 除了属性简写，方法也可以简写。

		var o = {
		  method() {
		    return "Hello!";
		  }
		};
		
		// 等同于
		
		var o = {
		  method: function() {
		    return "Hello!";
		  }
		};
  - 下面是一个实际的例子。
			   var birth = '2000/01/01';
			
			var Person = {
			
			  name: '张三',
			
			  //等同于birth: birth
			  birth,
			
			  // 等同于hello: function ()...
			  hello() { console.log('我的名字是', this.name); }
			
			};

    - 这种写法用于函数的返回值，将会非常方便。
    
			function getPoint() {
			  var x = 1;
			  var y = 10;
			  return {x, y};
			}
			
			getPoint()
			// {x:1, y:10}


 - CommonJS模块输出变量，就非常合适使用简洁写法。
 
			var ms = {};
			
			function getItem (key) {
			  return key in ms ? ms[key] : null;
			}
			
			function setItem (key, value) {
			  ms[key] = value;
			}
			
			function clear () {
			  ms = {};
			}
			
			module.exports = { getItem, setItem, clear };
			// 等同于
			module.exports = {
			  getItem: getItem,
			  setItem: setItem,
			  clear: clear
			};


# Object.is()
 - ES5比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。JavaScript缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。

- ES6提出“Same-value equality”（同值相等）算法，用来解决这个问题。Object.is就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

		 Object.is('foo', 'foo')
		// true
		Object.is({}, {})
	    // false
不同之处只有两个：一是+0不等于-0，二是NaN等于自身。

		+0 === -0 //true
		NaN === NaN // false
		
		Object.is(+0, -0) // false
		Object.is(NaN, NaN) // true
ES5可以通过下面的代码，部署Object.is。

		Object.defineProperty(Object, 'is', {
		  value: function(x, y) {
		    if (x === y) {
		      // 针对+0 不等于 -0的情况
		      return x !== 0 || 1 / x === 1 / y;
		    }
		    // 针对NaN的情况
		    return x !== x && y !== y;
		  },
		  configurable: true,
		  enumerable: false,
		  writable: true
		});

# Object.assign()
 - Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
 
		var target = { a: 1 };
		
		var source1 = { b: 2 };
		var source2 = { c: 3 };
		
		Object.assign(target, source1, source2);
		target // {a:1, b:2, c:3}
注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
		var target = { a: 1, b: 1 };
		
		var source1 = { b: 2, c: 2 };
		var source2 = { c: 3 };
		
		Object.assign(target, source1, source2);
		target // {a:1, b:2, c:3}

       如果只有一个参数，Object.assign会直接返回该参数。

		var obj = {a: 1};
		Object.assign(obj) === obj // true

	如果该参数不是对象，则会先转成对象，然后返回。

		typeof Object.assign(2) // "object"

	由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。

		Object.assign(undefined) // 报错
		Object.assign(null) // 报错

	如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果undefined和null不在首参数，就不会报错。

		let obj = {a: 1};
		Object.assign(obj, undefined) === obj // true
		Object.assign(obj, null) === obj // true

# Symbol
- ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript语言的第七种数据类型，前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

- Symbol值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的Symbol类型。凡是属性名属于Symbol类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。
 
		let s = Symbol();
		
		typeof s
		// "symbol"

    面代码中，变量s就是一个独一无二的值。typeof运算符的结果，表明变量s是Symbol数据类型，而不是字符串之类的其他类型。

	注意，Symbol函数前不能使用new命令，否则会报错。这是因为生成的Symbol是一个原始类型的值，不是对象。也就是说，由于Symbol值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。
	
	Symbol函数可以接受一个字符串作为参数，表示对Symbol实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
		var s1 = Symbol('foo');
		var s2 = Symbol('bar');
		
		s1 // Symbol(foo)
		s2 // Symbol(bar)
		
		s1.toString() // "Symbol(foo)"
		s2.toString() // "Symbol(bar)"

	上面代码中，s1和s2是两个Symbol值。如果不加参数，它们在控制台的输出都是Symbol()，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。
	
	注意，Symbol函数的参数只是表示对当前Symbol值的描述，因此相同参数的Symbol函数的返回值是不相等的。

		// 没有参数的情况
		var s1 = Symbol();
		var s2 = Symbol();
		
		s1 === s2 // false
		
		// 有参数的情况
		var s1 = Symbol("foo");
		var s2 = Symbol("foo");
		
		s1 === s2 // false

	上面代码中，s1和s2都是Symbol函数的返回值，而且参数相同，但是它们是不相等的。

	Symbol值不能与其他类型的值进行运算，会报错。

    Symbol值可以显式转为字符串。
    Symbol值也可以转为布尔值，但是不能转为数值
	

# Promise对象	 
 -  ES6规定，Promise对象是一个构造函数，用来生成Promise实例。
 -  下面代码创造了一个Promise实例。
 
		var promise = new Promise(function(resolve, reject) {
		  // ... some code
		
		  if (/* 异步操作成功 */){
		    resolve(value);
		  } else {
		    reject(error);
		  }
		});
	Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。

	resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
	
	Promise实例生成以后，可以用then方法分别指定Resolved状态和Reject状态的回调函数。

		promise.then(function(value) {
		  // success
		}, function(error) {
		  // failure
		});

	then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为Resolved时调用，第二个回调函数是Promise对象的状态变为Reject时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。
	
	下面是一个Promise对象的简单例子。

		let promise = new Promise(function(resolve, reject) {
		  console.log('Promise');
		  resolve();
		});
		
		promise.then(function() {
		  console.log('Resolved.');
		});
		
		console.log('Hi!');
		
		// Promise
		// Hi!
		// Resolved
   
    上面代码中，Promise新建后立即执行，所以首先输出的是“Promise”。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以“Resolved”最后输出。

	下面是异步加载图片的例子。
		
		function loadImageAsync(url) {
		  return new Promise(function(resolve, reject) {
		    var image = new Image();
		
		    image.onload = function() {
		      resolve(image);
		    };
		
		    image.onerror = function() {
		      reject(new Error('Could not load image at ' + url));
		    };
		
		    image.src = url;
		  });
		}
    上面代码中，使用Promise包装了一个图片加载的异步操作。如果加载成功，就调用resolve方法，否则就调用reject方法。


# Class
 - Class基本语法

		function Point(x, y) {
		  this.x = x;
		  this.y = y;
		}
		
		Point.prototype.toString = function () {
		  return '(' + this.x + ', ' + this.y + ')';
		};
		
		var p = new Point(1, 2);
     
    可以改写成

    	//定义类
		class Point {
		  constructor(x, y) {
		    this.x = x;
		    this.y = y;
		  }
		
		  toString() {
		    return '(' + this.x + ', ' + this.y + ')';
		  }
		}
	上面代码定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。也就是说，ES5的构造函数Point，对应ES6的Point类的构造方法。
	
	Point类除了构造方法，还定义了一个toString方法。注意，定义“类”的方法的时候，前面不需要加上function这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错。
	
	ES6的类，完全可以看作构造函数的另一种写法。
   上面代码表明，类的数据类型就是函数，类本身就指向构造函数。

    使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致。

		class Bar {
		  doStuff() {
		    console.log('stuff');
		  }
		}
		
		var b = new Bar();
		b.doStuff() // "stuff"

- 构造函数的prototype属性，在ES6的“类”上面继续存在。事实上，类的所有方法都定义在类的prototype属性上面。

		class Point {
		  constructor(){
		    // ...
		  }
		
		  toString(){
		    // ...
		  }
		
		  toValue(){
		    // ...
		  }
		}
		
		// 等同于
		
		Point.prototype = {
		  toString(){},
		  toValue(){}
		};



# 编程风格

## 数组
- 使用扩展运算符（...）拷贝数组。
 
		 // bad
		const len = items.length;
		const itemsCopy = [];
		let i;
		
		for (i = 0; i < len; i++) {
		  itemsCopy[i] = items[i];
		}
		
		// good
		const itemsCopy = [...items];
- 使用Array.from方法，将类似数组的对象转为数组。

		const foo = document.querySelectorAll('.foo');
		const nodes = Array.from(foo);

## 函数
 - 立即执行函数可以写成箭头函数的形式。

		(() => {
		  console.log('Welcome to the Internet.');
		})();

## Class
 - 总是用Class，取代需要prototype的操作。因为Class的写法更简洁，更易于理解。
 
		 // bad
		function Queue(contents = []) {
		  this._queue = [...contents];
		}
		Queue.prototype.pop = function() {
		  const value = this._queue[0];
		  this._queue.splice(0, 1);
		  return value;
		}
		
		// good
		class Queue {
		  constructor(contents = []) {
		    this._queue = [...contents];
		  }
		  pop() {
		    const value = this._queue[0];
		    this._queue.splice(0, 1);
		    return value;
		  }
		}


## ECMAScript 6

##### 1: 转码器 babel、traceur

##### 2: let 变量声明

​	1）：块级作用域 => {},   外层代码块不受内层代码块的影响。var声明的变量的作用域是整个封闭函数。

​	2）： 没有变量声明的提升，（其实var、let、 const、 function、function*、class 都存在变量声明提升， var变量提升会将变量初始化位undefined， let没有初始化）

​	3）：暂时死区

​	4）：不能重复声明

##### 3: 块级作用域与函数声明

- 允许在块级作用域内声明函数。

- 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。

- 同时，函数声明还会提升到所在的块级作用域的头部。

  ```
  // 浏览器的 ES6 环境
  function f() { console.log('I am outside!'); }

  (function () {
    if (false) {
      // 重复声明一次函数f
      function f() { console.log('I am inside!'); }
    }

    f();
  }());
  // Uncaught TypeError: f is not a function

  上面的代码在符合 ES6 的浏览器中，都会报错，因为实际运行的是下面的代码。
  // 浏览器的 ES6 环境
  function f() { console.log('I am outside!'); }
  (function () {
    var f = undefined;
    if (false) {
      function f() { console.log('I am inside!'); }
    }

    f();
  }());
  // Uncaught TypeError: f is not a function
  ```

考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

```
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```



 4:const

- ​	const声明一个只读常量，常量的值不能改变。
- const的作用域与let命令相同， 只在声明所在的块级作用域内有效。
- 暂时性死区， 只能在声明的位置后面使用。

##### 5: 顶层对象

​	顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的。

​	es6: `var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性。

5: this

- ​	无论是否在严格模式下，在全局执行上下文中（在任何函数体外部）`this` 都指代全局对象。
- ​         在函数内部，`this`的值取决于函数被调用的方式。
  - ​	不在严格模式， 函数内部的this 默认指向全局对象。
  - 在严格模式下，`this`将保持他进入执行上下文时的值，所以下面的`this`将会默认为`undefined`。
  - call、apply    将一个对象作为call和apply的第一个参数，this会被绑定到这个对象
  - bind  调用`f.bind(someObject)`会创建一个与`f`具有相同函数体和作用域的函数，但是在这个新函数中，`this`将永久地被绑定到了`bind`的第一个参数，无论这个函数是如何被调用的

##### 6: 编码方式

- ​	整个Unicode字符集的大小现在是（2的21次方）

- Utf-32 **最直观的编码方法是，每个码点使用四个字节表示，字节内容一一对应码点**

- **UTF-8是一种变长的编码方法，字符长度从1个字节到4个字节不等。**越是常用的字符，字节越短。

- ucs-2  使用2个字节表示已经有码点的字符。（javaScript use） **由于JavaScript只能处理UCS-2编码，造成所有字符在这门语言中都是2个字节，如果是4个字节的字符，会当作两个双字节的字符处理。**JavaScript的字符函数都受到这一点的影响，无法返回正确结果。

- Es6  使用‘ \u{ 码点}’ ， 加强了对unicode的支持

- 字符串的遍历器接口，for….of 可以识别大于oxFFFF的码点，传统的for循环无法识别这样的码点。

  ​

##### 7:字符串

​	7.1 JavaScript 只有`indexOf`方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。

​	**includes()**：返回布尔值，表示是否找到了参数字符串。

​	**startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。

​	**endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

7.2: 模版字符串

​	`  There are <b>${basket.count}</b> items   in your basket, <em>${basket.onSale}</em>  are on sale!`

- ​	使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中	

  - ​模板字符串中嵌入变量，需要将变量名写在`${}`之中.

  - ​大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。

  - 模板字符串之中还能调用函数

  - “标签模板”的一个重要应用，就是过滤 HTML 字符串，防止用户输入恶意内容。

    ​

##### 8 : 数值

​	8.1 二进制数值前缀0b（0B）； 8进制数值前缀0o (0O)

​	8.2 Number.isFinite() 用来检查一个数值是否为有限的（finite），即不是`Infinity`,   只对数值有效，

​		Number.isNaN() 用来检查一个值是否为`NaN`，只对数值有效，

​		传统的全局方法`isFinite()`和`isNaN()`， 是先调用了Number()将非数值转化成数值，再判断。

​	8.3 Number.parseInt(), Number.parseFloat() ES6 将全局方法`parseInt()`和`parseFloat()`，移植到`Number`对象上面，行为完全保持不变，目的是逐步减少全局性方法，使得语言逐步模块化。

​	8.4 Number.isInteger()  用来判断一个数值是整数。

​	8.5 Number.EPSILON  是一个极小的常量，用于设置能够接受的误差范围。

​		Number.EPSILON === Math.pow(2, -52)

​		(0.1+0.2-0.3 )<Number.EPSILON

​	8.6: Math 新增17个方法

​		math.trunc() 方法用于去除一个数的小数部分，返回整数部分。

```
		Math.trunc(4.1) // 4
		对于非数值，Math.trunc内部使用Number方法将其先转为数值。
		Math.trunc('123.456') // 123

		Math.trunc(true) //1

		Math.trunc(false) // 0

		Math.trunc(null) // 0

```

​		`Math.sign（）`方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

```
- 参数为正数，返回`+1`；
- 参数为负数，返回`-1`；
- 参数为 0，返回`0`；
- 参数为-0，返回`-0`;
- 其他值，返回`NaN`。		
```

​		`Math.cbrt`方法用于计算一个数的立方根。方法内部也是先使用Number方法将其转化为数据。

​	8.7 指数运算符

```
2**2 //4
2**3 //8
2 ** 3 ** 2 //  2**(3**2）
```

##### 9: 变量的解构赋值

​	ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值。

​	9.1: 数组的解构赋值

​		let [a, b, c] = [1, 2, 3];

​		解构赋值允许指定默认值

​		let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'

​	9.2: 对象的解构赋值

​		let { foo, bar } = { foo: "aaa", bar: "bbb" };

​		对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

```
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
foo是匹配的模式，baz才是变量。真正被赋值的是变量baz，而不是模式foo
```

​	9.3: 字符串的解构赋值

​		const [a, b, c, d, e] = 'hello';

​	9.4: 变量解构赋值的用途： 

​		函数返回多个值,   let {foo, bar }= example();

​		提取json数据：  let  {id, status, data} = jsondata;

​		输入模块的指定方法： const { SourceMapConsumer, SourceNode } = require("source-map");

​		

​		

##### 10: 函数的扩展

​	10.1 es6允许为函数的参数设置默认值，直接写在参数定义的后面

​		function log(x, y = 'World') {  console.log(x, y);}

​		参数默认值，应该是函数的尾参数， 比较容易看出，省略了哪些参数。

​		利用参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。

```
function throwIfMissing() { 
	 throw new Error('Missing parameter');
}

function foo(mustBeProvided = throwIfMissing()) {  
	return mustBeProvided;
}
foo() // Error: Missing parameter
```

​	10.2  rest 参数

​		ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。

​		rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

​		ES2016 做了一点修改，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

​		两种方法可以规避这种限制。第一种是设定全局性的严格模式，这是合法的。

```
'use strict';

function doSomething(a, b = a) {
  // code
}

```

第二种是把函数包在一个无参数的立即执行函数里面。

```
const doSomething = (function () {
  'use strict';
  return function(value = 42) {
    return value;
  };
}());
```

​		

​	10.3 箭头函数

​		箭头函数可以绑定`this`对象，大大减少了显式绑定`this`对象的写法（`call`、`apply`、`bind`）。

​	10.4 双冒号运算符

​		函数绑定运算符是并排的两个冒号（`::`），双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即`this`对象），绑定到右边的函数上面。

```
foo::bar;
// 等同于
bar.bind(foo);	
```

​	10.5: 尾调用优化

​	尾调用（Tail Call），指某个函数的最后一步是调用另一个函数。

```
function f(x){
  return g(x);
}
```

​	尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。



##### 11 数组的扩展

​	11.1扩展运算符

​		扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```
console.log(...[1, 2, 3])
// 1 2 3
```

```
function add(x, y) {
  return x + y;
}
const numbers = [4, 38];
add(...numbers) // 42
```

​	 应用：

​		1: 数组的复制

​			const a1 = [1, 2];   const a2 = […a1]; //写法1

​			const  […a2] = a1; //写法2

​		2:合并数组

​			const arr1 = ['a', 'b'];  const arr2 = ['c'];  const arr3 = ['d', 'e'];

​			[...arr1, ...arr2, ...arr3]

​			// [ 'a', 'b', 'c', 'd', 'e' ]

​		3: 与解构赋值结合

​			const [first, ...rest] = [1, 2, 3, 4, 5];

​			first // 1

​			rest  // [2, 3, 4, 5]

​			如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

​		4: 字符串

​			[...'hello'] // [ "h", "e", "l", "l", "o" ]

​	11.2 Array.from()

​		`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

​		

##### 12 Iterator

​	每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。

​	ES6 的有些数据结构原生具备 Iterator 接口（比如数组），即不用任何处理，就可以被`for...of`循环遍历。原因在于，这些数据结构原生部署了`Symbol.iterator`属性。

原生具备 Iterator 接口的数据结构如下。

- Array    -> for of  返回数字索引的值

- Map.  ->  for of. 返回数组，数组的两个成员为当前Map成员的键名和键值

- Set  ->  返回值

- String

- TypedArray

- 函数的 arguments 对象

- NodeList 对象

  ​JavaScript 原有的`for...in`循环，只能获得对象的键名，不能直接获取键值。ES6 提供`for...of`循环，允许遍历获得键值。`for...of`循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性。这一点跟`for...in`循环也不一样。	

  ​ES6 的数组、Set、Map 都部署了以下三个方法，调用后都返回遍历器对象。

- `entries()` 返回一个遍历器对象，用来遍历`[键名, 键值]`组成的数组。对于数组，键名就是索引值；对于 Set，键名与键值相同。Map 结构的 Iterator 接口，默认就是调用`entries`方法。

- `keys()` 返回一个遍历器对象，用来遍历所有的键名。

- `values()` 返回一个遍历器对象，用来遍历所有的键值。

  类似数组的对象包括好几类。下面是`for...of`循环用于字符串、DOM NodeList 对象、`arguments`对象的例子。

  ​

##### 13Generator函数

​	形式上，Generator 函数是一个普通函数，但是有两个特征。一是，`function`关键字与函数名之间有一个星号；二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）	 				

​	调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束

##### 14 Promises异步编程

- promise是一个对象，对象和函数的区别就是对象可以保存状态，函数不可以（闭包除外）

  ###### Promise有三个状态：Pending（进行中）、Resolved或Fulfilled（已完成）、Rejected（已失败）

  ​	其中：Pending为Promise的初始状态；当Resolved成功时，会调用onFulfilled方法；当Rejected失败时，会调用onRejected方法。 并且：状态只能从Pending转换为Resolved状态，或者从Pending转换为Rejected状态，不存在其他状态间的转换

  ​**Then方法**

  ​	Promise必须提供一个then方法，用以访问当前值、最终的返回值以及失败的原因。

  ​	最基本的then方法接受两个函数参数 promise.then(onFulfilled, onReject)，对应于状态变化到Resolved	和Rejected时的函数调用。

  ​**new Promise(func)**

  ​通过实例化构造函数成一个promise对象，构造函数中有个函数参数，函数参数为(resolve, reject)的形式，供以函数内resolve成功以及reject失败的调用

  ​**.then(onFulfilled, onRejected)**

  ​	then方法，方法带两个参数，可选，分别为成功时的回调以及失败时的回调

  ​**.catch(onRejected)**

  ​	catch方法，方法带一个参数，为失败时的回调。其实.catch方法就是 .then(undefined, onRejected)的简化版，通过例子看看它的特点

##### fetch

`fetch`规范与`jQuery.ajax()`主要有两种方式的不同，牢记：

- 当接收到一个代表错误的 HTTP 状态码时，从 `fetch()`返回的 Promise **不会被标记为 reject，** 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 `ok` 属性设置为 false ），仅当网络故障时或请求被阻止时，才会标记为 reject。

- 默认情况下，`fetch` **不会从服务端发送或接收任何 cookies**, 如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 [credentials](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch#%E5%8F%82%E6%95%B0) 选项）。自从2017年8月25日后，默认的credentials政策变更为`same-origin`Firefox也在61.0b13中改变默认值

- ```
  fetch('https://example.com', {
    credentials: 'include'  
  })

  ```

### 异步编程

#### 回调函数：

```
fs.readFile('/etc/passwd', 'utf-8', function (err, data) {
  if (err) throw err;
  console.log(data);
});

```

#### generator 函数：

Generator 函数是协程在 ES6 的实现，最大特点就是可以交出函数的执行权（即暂停执行）。

由于 Generator 函数返回的遍历器对象，只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield`表达式就是暂停标志。	

```
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }

```

### async：

[参考](http://es6.ruanyifeng.com/#docs/async)

async 函数是什么？一句话，它就是 Generator 函数的语法糖。

async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。

```
onst fs = require('fs');

const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

//generator函数实现
const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
//改写成async， *->async， yield -> await
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};

```


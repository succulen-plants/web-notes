### Console:

##### 1:

##### console.info("这是info");

　　console.debug("这是debug");

　　console.warn("这是warn");

　　console.error("这是error");

#### 2:console.assert()

​		console.assert()用来判断一个表达式或变量是否为真。如果结果为否，则在控制台输出一条相应信息，并且抛出一个异常。比如，下面两个判断的结果都为否。

```
	var result = 0;

　　console.assert( result );

　　var year = 2000;

　　console.assert(year == 2011 );

```

#### 3: console.trace()

​	console.trace()用来追踪函数的调用轨迹。

```
function add(a,b){ console.trace(); return a+b}

function add3(a,b){ add2(a,b);}

function add2(a,b){ add1(a,b);}

function add1(a,b){ add(a,b);}
var x = add3(1,1);

打印结果：
VM885:1 console.trace
add @ VM885:1
add1 @ VM885:7
add2 @ VM885:5
add3 @ VM885:3
(anonymous) @ VM885:8
```

#### 4: 计时功能：

​	console.time()和console.timeEnd()，用来显示代码的运行时间。

```
　console.time("计时器一");

　　for(var i=0;i<1000;i++){

　　　　for(var j=0;j<1000;j++){}

　　}
　　console.timeEnd("计时器一");
　　
　　打印结果：
　　计时器一: 4.910888671875ms
```

#### 5: 性能分析

​	性能分析（Profiler）就是分析程序各个部分的运行时间，找出瓶颈所在，使用的方法是console.profile()

```
function Foo(){

　　　　for(var i=0;i<10;i++){funcA(1000);}

　　　　funcB(10000);

　　}

　　function funcA(count){

　　　　for(var i=0;i<count;i++){}

　　}

　　function funcB(count){

　　　　for(var i=0;i<count;i++){}

　　}
　　console.profile('性能分析器一');

　　Foo();

　　console.profileEnd();
```


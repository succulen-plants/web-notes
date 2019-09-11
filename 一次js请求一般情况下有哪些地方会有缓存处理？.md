

#### 一次js请求一般情况下有哪些地方会有缓存处理？

##### a. 浏览器端存储: cookie、localstorage、sessionstroage

​	区别：

​	1）cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递；而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存；

​	2）cookie数据有路径（path）的概念，可以限制cookie只属于某个路径下；

​	3）存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识；sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大；

​	4）数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭；

​	5）作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的；

##### b. 浏览器端文件缓存

##### C. Cache-control策略

页面 F5 刷新和强刷时，缓存将失效

c. HTTP缓存304

d. 服务器端文件类型缓存

e. 表现层&DOM缓存

浏览器第一次请求：

![img](http://images.cnblogs.com/cnblogs_com/skynet/201211/201211281402437422.png)

浏览器第二次请求：

![img](http://images.cnblogs.com/cnblogs_com/skynet/201211/201211281402442505.png)


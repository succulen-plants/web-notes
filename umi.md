### umi

#### 原理

1: umi 没有使用webpack-dev-server, 自己开发了umi-server， umi-server 是基于node的express，umi-server中express: app.use(require('umi-mock').createMiddleware()) 实现了 mock测试数据。

2: webpack-dev-server 也是基于 express实现

3: 路由实现 let Router = require('dva/router').routerRedux.ConnectedRouter; 

4:懒加载实现：  import _dvaDynamic from 'dva/dynamic'
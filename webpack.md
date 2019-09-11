## webpack

​	4.1: output  告诉webpack 在哪里输出它所创建的bundles， 命名、路径。

​	4.2: loader：*loader* 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）

​	4.3: plugin:  `plugins` 选项用于以各种方式自定义 webpack 构建过程。webpack 附带了各种内置插件，可以通过 `webpack.[plugin-name]` 访问这些插件。

​	4.4:  externals:    **防止**将某些 `import` 的包(package)**打包**到 bundle 中，而是在运行时(runtime)再去从外部获取这些*扩展依赖(external dependencies)*。

​	4.5: dependencies   是需要发布到生产环境的

​		devDependencies. 里面的插件只用于开发环境，不用于生产环境

​	4.6:  webpack打包第三方库

方法一： 不推荐

```
entry: {
     		index: './app/main.jsx',
       		vendor: ['react', 'react-dom', 'react-router', 'classnames'] //第三方库入口
   		 },
plugins: [
        new webpack.optimize.CommonsChunkPlugin({
            names: ['vendor', 'manifest'],
        }),
    ]

```

方法二：

```
webpac.dll.config.js:

const webpack = require('webpack');
const path    = require('path');

const vendors = [
    'antd',
    // 'isomorphic-fetch',
    'react',
    'react-dom',
    'react-redux',
    'react-router',
    'redux',
    // 'redux-promise-middleware',
    'redux-thunk',
    // 'superagent',
    'd3',
    'echarts',
    // 'echarts-gl',
    'echarts-for-react',
    // './app/dist/echarts.min',
    // './app/dist/echarts-gl.min',
    'highcharts',
    // 'echarts-gl',
    // 'g6',
    '@antv/g6',
    'jquery',
];
function resolve(relatedPath) {
    return path.join(__dirname, relatedPath)
}
module.exports = {
    output: {
        // path: './app',
        path: resolve('../dist'),
        filename: '[name].[hash:4].js',
        library: '[name]',
    },
    entry: {
        vendor: vendors,
    },
    plugins: [
        new webpack.DllPlugin({
            path: resolve('../dist/manifest.json'),
            name: '[name]',
            context: __dirname,
        }),
    ],
};

webpack.base.config.js
plugins:[
  new webpack.DllReferencePlugin({
            context: __dirname,
            manifest: require('../dist/manifest.json'),
            // name:'vendor'
        }),
]


```

配置DllPlugin前后： 打包时间减小一半

![屏幕快照 2018-08-06 上午10.44.00](/Users/luotengzhan/typora/屏幕快照 2018-08-06 上午10.44.00.png)

![屏幕快照 2018-08-06 上午10.44.00](/Users/luotengzhan/typora/屏幕快照 2018-08-06 上午11.00.55.png)

4.7 代码分离

​	1: 入口起点： 

​		使用 [`entry`](https://webpack.docschina.org/configuration/entry-context) 配置手动地分离代码；

​		 使用DllPlugin手动分离打包。

​	2:阻止重复： 使用[`CommonsChunkPlugin`](https://webpack.docschina.org/plugins/commons-chunk-plugin) 去重和分离 chunk

​		plugins:[

​				new webpack.optimize.CommonsChunkPlugin({    name: 'common', // 入口文件名    

​					filename: 'common.[hash:4].js', // 打包后的文件名    

​					minChunks: 2 

​				}),

​		]

​	使用前client.0194.js     使用后： client.b017.js 、common.b017.js

![屏幕快照 2018-08-07 上午9.41.26](/Users/luotengzhan/typora/屏幕快照 2018-08-07 上午9.41.26.png)

​		

minChunks:  number

经过我测试，发现minChunks是指某个模块**最少被多少个入口文件**依赖。
当大于等于minChunks设定的值时，该模块就会被打包到公用包中。
小于这个值时，该模块就会被和每个入口文件打包在一起。

搞懂了minChunks的number属性，Infinity属性就很好理解了。也就是不会把任何依赖的模块提取出来打包公用。

4.8  webpack - react 懒加载实现：require.ensure + react-route

​	react-route: 

​	`getComponent(location, callback)` 与 `component` 一样，但是是异步的，对于 code-splitting 很有用。

```
<Route path="courses/:courseId" getComponent={(location, cb) => {

	  // 做一些异步操作去查找组件 

	 cb(null, Course)}}/>
```

```
const barGraph = (location, cb) => {require.ensure([], require => {cb(null, require('graphs').default)}, 'barGraph')}
<Route path="/barGraph" getComponent={barGraph} />
```

​	CommonJS致力于为浏览器之外的 JavaScript 指定一个生态系统,`require.ensure()` 是 webpack 特有的，已经被 `import()` 取代。

```
require.ensure(dependencies: String[], callback: function(require), errorCallback: function(error), chunkName: String)
给定 dependencies 参数，将其对应的文件拆分到一个单独的 bundle 中，此 bundle 会被异步加载。当使用 CommonJS 模块语法时，这是动态加载依赖的唯一方法。意味着，可以在模块执行时才运行代码，只有在满足某些条件时才加载依赖项。

dependencies：字符串构成的数组，声明 callback 回调函数中所需的所有模块。
callback：只要加载好全部依赖，webpack 就会执行此函数。require 函数的实现，作为参数传入此函数。当程序运行需要依赖时，可以使用 require() 来加载依赖。函数体可以使用此参数，来进一步执行 require() 模块。
errorCallback：当 webpack 加载依赖失败时，会执行此函数。
chunkName：由 require.ensure() 创建出的 chunk 的名字。通过将同一个 chunkName 传递给不同的 require.ensure() 调用，我们可以将它们的代码合并到一个单独的 chunk 中，从而只产生一个浏览器必须加载的 bundle。

```


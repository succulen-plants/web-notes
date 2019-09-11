## canvas

#### 原理理解

首先，找到 <canvas> 元素:

```
var c=document.getElementById("myCanvas");
```

然后，创建 context 对象：

```
var ctx=c.getContext("2d");
```

getContext("2d") 对象是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。

getContext(in [DOMString](https://developer.mozilla.org/zh-CN/DOM/DOMString)contextId)  返回canvas的绘制上下文,如果指定的上下文ID不支持,则返回null.一个绘制上下文可以让你在canvas上绘图.目前可接受的参数有"2d"和"experimental-webgl"."experimental-webgl"上下文只在那些实现了[WebGL](https://developer.mozilla.org/zh-CN/WebGL)的浏览器上可用.调用getContext("2d")会返回一个 `CanvasRenderingContext2D`对象,调用getContext("experimental-webgl")会返回一个`WebGLRenderingContext`对象.



1:获取canvas鼠标坐标

#### getBoundingClientRect()

这个方法返回的对象有六个属性，如下表：

| 属性     | 描述             |
| ------ | -------------- |
| top    | 元素上边界距窗口最上边的距离 |
| left   | 元素左边界距窗口最左边的距离 |
| bottom | 元素下边界距窗口最上边的距离 |
| right  | 元素右边界距窗口最左边的距离 |
| width  | 元素的宽度          |
| height | 元素的高度          |

```
var canvas = document.getElementById(this.state.id);
var context = canvas.getContext("2d");
// 监听点击事件
canvas.addEventListener("mousemove", function(event) {
    this.setState({coord:getMousePos(canvas, event)})
}.bind(this));

function getMousePos(canvas, event) {

    var rect = canvas.getBoundingClientRect();
    
    var x = (event.clientX - rect.left) * (canvas.width / rect.width);
    var y = (event.clientY - rect.top) * (canvas.height / rect.height);
    // rect.left * (canvas.width / rect.width)
    // 之所以要乘一个(canvas.width / rect.width)，是为了保证画布的像素值和显示在页面上的尺寸不一致的情况下，也能够正确获取到鼠标相对于canvas中的坐标。
    return `x:${x}   y:${y}`;
}
```

2:把一幅图像放置到画布上

**CanvasRenderingContext2D.drawImage()**

```
void ctx.drawImage(image, dx, dy);  
image绘制到上下文的元素。允许任何的 canvas 图像源(CanvasImageSource)，例如：HTMLImageElement，HTMLVideoElement，或者 HTMLCanvasElement。
```

```
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
var img=document.getElementById("scream");
ctx.drawImage(img,10,10);
```

### 绘制路径

**当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合**。

```
	ctx.beginPath();  //新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径
    ctx.moveTo(75, 50);   //将笔触移动到指定的坐标x以及y上
    ctx.lineTo(100, 75);  //绘制一条从当前位置到指定x以及y位置的直线
    ctx.lineTo(100, 25);
    ctx.fill();  //通过填充路径的内容区域生成实心的图形。
  	 =======
    ctx.beginPath();
    ctx.moveTo(75, 50);
    ctx.lineTo(100, 75);
    ctx.lineTo(100, 25);
    ctx.closePath(); //闭合路径之后图形绘制命令又重新指向到上下文中。
 	ctx.stroke(); //通过线条来绘制图形轮廓。
 	
 	//这里从调用beginPath()函数准备绘制一个新的形状路径开始。然后使用moveTo()函数移动到目标位置上。然后下面，两条线段绘制后构成三角形的两条边
   
```

#### 圆弧

[`arc(x, y, radius, startAngle, endAngle, anticlockwise)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arc)

##### startAngle

圆弧的起始点， x轴方向开始计算，单位以弧度表示

##### endAngle

圆弧的终点， 单位以弧度表示， 一个完整的圆的弧度2*Math.PI

##### anticlockwise

可选的[`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean)值 ，如果为 `true`，逆时针绘制圆弧，反之，顺时针绘制

```
ctx.beginPath();
ctx.arc(75, 75, 50, 0, 2 * Math.PI);
ctx.stroke();
```

#### colors



#### ImageData 对象

[`ImageData`](https://developer.mozilla.org/zh-CN/docs/Web/API/ImageData)对象中存储着canvas对象真实的像素数据；

1. width 图片宽度， 单位是像素

- `height`

  图片高度，单位是像素

- `data`

  [`Uint8ClampedArray`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray)类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）。

每个像素用4个1bytes值(按照红，绿，蓝和透明值的顺序; 这就是"RGBA"格式) 来代表。每个颜色值部份用0至255来代表。每个部份被分配到一个在数组内连续的索引，左上角像素的红色部份在数组的索引0位置。像素从左到右被处理，然后往下，遍历整个数组。

[`Uint8ClampedArray`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray)  包含高度 × 宽度 × 4 bytes数据，索引值从0到(`高度`×宽度×4)-1。

例如，要读取图片中位于第50行，第200列的像素的蓝色部份，你会写以下代码：

```
blueComponent = imageData.data[((50*(imageData.width*4)) + (200*4)) + 2];
```


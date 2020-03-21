# Canvas总结

## 第一章	Canvas创建

*  Canvas API（画布）是在HTML5中新增的标签用于在网页实时生成图像，并且可以操作图像内容，基本上它是一个可以用JavaScript操作的位图（bitmap）




#### 创建一个canvas画布

* HTML部分：

```html
<canvas>当前浏览器不支持canvas，请更换浏览器再试</canvas>
```

> canvas 是一个二维网格。canvas 的左上角坐标为 (0,0)
>
> canvas 元素本身是没有绘图能力的。所有的绘制工作必须在 JavaScript 内部完成
>
> canvas 标签中的内容只有当canvas画布无法显示时显示

* JavaScript部分：

```javascript
var canvas = document.querySelector('canvas')
canvas.width = 1024
canvas.height = 768
```

> canvas 标签的宽度和高度需在 js 代码中设置，css代码中设置的宽高仅仅只是拉伸 canvas 画布



#### 获取绘图工具

```javascript
var context = canvas.getContext('2d')
```

> getContext("2d")  对象是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。



## 第二章	canvas的路径

#### 绘制路径

* 在Canvas上画线，我们将使用下列方法：

  ```javascript  
  context.moveTo(x,y)	//	定义线条开始坐标
  context.lineTo(x,y) //	定义线条结束坐标
  
  context.fillStyle   //	着色的样式(一般写颜色)
  //  color、gradient、image、canvas、video
  
  context.strokeStyle //	线条样式(一般写颜色)
  //  fillStyle适用的样式都适用于strokeStyle
  
  context.stroke()    //	绘制
  context.fill()      //	着色
  ```

> 通过写多次lineTo可以绘制折线，或绘制多边形




#### 绘制多个路径

```javascript
context.beginPath()	//  写了该条语句后，若不写moveTo语句, 默认第一条lineTo语句为moveTo
	//	绘制一个路径
context.closePath()   //  结束当前路径，若路径不是封闭路径，则将路径封闭 
```


> 该两条语句成对出现可以绘制封闭图形，且不会存在瑕疵
>
> context.closePath()	为可写方法，不添加则不封闭路径



## 第三章	线条属性

#### 线条宽度

* context.lineWidth 

* 属性值
  * 数字，不带单位



#### 线条两端样式

* context.lineCup

* 属性值

  * butt(默认值)
  * round
  * square // 可以解决封闭路径的瑕疵问题，但一般用closePath()

  > square 可以解决封闭路径的瑕疵问题，但一般用closePath()
  >
  > lineCup 不能用于线段连接处



####线段相交处形状样式

* context.lineJoin

  * miter(默认值)

    * context.miterLimit

      > 这个值默认为10: 当内角和外角的距离超过 10 时, 多余的长度不显示
      >
      >  (暂时这么理解，事实上 miterLimit 计算方式比较复杂)

  * bevel	斜边

    > 像一条长纸条折叠后的效果(顶部平的)
  
  * round



## 第四章	绘制圆形

####context.arc()

* 参数

  * centerX,centerY,  // 圆心坐标
  * radius,
  * startingAngle,endingAngle, 
    * 这里的角度以PI为单位, 下为0.5PI, 左为PI, 上为1.5PI,右为2PI
  * anticlosewise = false  
    * 默认为顺时针绘制

  > 实际上我们在绘制圆形时使用了 "ink" 的方法, 比如 stroke() 或者 fill().

```javascript
var c=document.getElementById("myCanvas"); 
var ctx=c.getContext("2d"); 
ctx.beginPath(); 
ctx.arc(95,50,40,0,2*Math.PI); 
ctx.stroke();
```



## 第五章	矩形与刷新

#### 绘制矩形

```javascript
context.rect(x,y,width,height)
  ```



#### 清空刷新操作

​```javascript
context.clearRect(startX, startY, width, height)
  ```

> 对矩形空间内的画布进行清空操作，有刷新的效果



#### 矩形填充

```javascript
context.fillRect(x, y, width, height)
```

> 也具有刷新画布的效果



#### 绘制矩形边框

```javascript
context.strokeRect(x, y, width, height)
```

> 用stoke对绘制矩形的边框



## 第六章	图形变换

#### 平移

```javascript
translate(x, y)
```

* 将基坐标(0, 0)移动至(x, y)

* 多次使用translate(x, y)会有叠加效果，故需要在每次使用之后结尾添加 translate(-x, -y) 语句

  > 或使用save(), restore()语句



#### 缩放

```javascript
scale(sx, sy)
```

* 在横向进行sx倍的缩放，在纵向进行sy倍的缩放

  > 副作用：还会缩放基点(x，y)的坐标，lineWidth也会缩放



#### 倾斜

```javascript
skew(sx, sy)   
```

* sx: 将画布在x方向上倾斜相应的角度，sx为倾斜角度的tan值
* sy: 将画布在y方向上倾斜相应的角度，sy为倾斜角度的tan值



#### 旋转

```javascript
rotate(deg)
```

* deg为弧长表示



#### 变换矩阵

​	|a  c  e|   a 水平缩放(1) c 垂直倾斜(0) e 水平位移(0)

​	|b  d  f|   b 水平倾斜(0) d 垂直缩放(1) f 垂直位移(0)

​	|0  0 1| 

```javascript
tansform(a, b, c, d, e, f)	//	可以直接改变变换矩阵设置图形变换

setTransform(a, b, c, d, e, f)	//	忽略之前设置的变换，以此次变换为准
```



## 第七章	环境状态保存与恢复

#### 保存

* context.save()
  * 保存之前的环境状态



#### 恢复

* context.restore()
  * 返回save()保存的所有环境状态
  > save和restore一般成对出现，中间可以随意更改状态而不影响后续状态



## 第八章	渐变

####线性渐变

* 设置渐变线

  * var grd = context.createLinearGradient(xStart, yStart, xEnd, yEnd)

* 设置中间色彩

  * grd.addColorStop(start, color1) start开始为 0

  * grd.addColorStop(stop, color2) stop结束为 1

    > 中间的色彩stop值可以改为(0,1)之间

* 填充渐变色
  
  * context.fillStyle = grd

```javascript
var c=document.getElementById("myCanvas"); 
var ctx=c.getContext("2d"); 
// Create gradient 
var grd=ctx.createLinearGradient(0,0,200,0); 
grd.addColorStop(0,"red"); 
grd.addColorStop(1,"white"); 
// Fill with gradient 
ctx.fillStyle=grd; 
ctx.fillRect(10,10,150,80);
```



#### 径向渐变

* 设置径向渐变圆
  * var grd = context.createRadialGradient(x0, y0, r0, x1, y1, r1) 
  * 参数
    * x0：表示渐变的开始圆的 x 坐标
    * y0：表示渐变的开始圆的 y 坐标
    * r0：表示开始圆的半径
    * x1：表示渐变的结束圆的 x 坐标
    * y1：表示渐变的结束圆的 y 坐标
    * r1：表示结束圆的半径

* 设置中间色彩
  * grd.addColorStop(stop, color)
* 填充颜色
  * context.fillStyle = grd

```javascript
var c = document.getElementById("myCanvas"); 
var ctx = c.getContext("2d"); 

// Create gradient 
var grd = ctx.createRadialGradient(75,50,5,90,60,100); 
grd.addColorStop(0,"red"); 
grd.addColorStop(1,"white"); 

// Fill with gradient 
ctx.fillStyle = grd; 
ctx.fillRect(10,10,150,80);
```



## 第九章	使用图片、画布或 video

#### 使用图片

* context.createPattern(img, repeat-style)

  > createPattern和css背景图片使用类似

```javascript
var backgroundImg = new Image();
backgroundImg.src = '图片路径';
var pattern = context.createPattern(backgroundImg, 'no-repeat');
context.fillStyle = pattern;
```



####使用画布

* context.createPattern(canvas, repeat-Style)

  > 第一个参数除了图片还可以填写 画布 作为背景图片



####使用 video

* context.createPattern(video, repeat-style)

  > 还可以把 视频 当成背景



## 第十章	绘制曲线

####圆弧线

* context.arc()
* context.arcTo(x1, y1, x2, y2, radius)
  * (x1, y1)为控制点 (x2, y2)为结束点(曲线终点不一定为结束点)
  * (x0, y0)为其他线段结束点的坐标(arc,moveTo,lineTo结束点位置)
  * arcTo绘制一条与直线(x0,y0)(x1,y1)相切,且与另一条直线(x1,y1)(x2,y2)也相切，半径为radius的圆弧线



#### 贝塞尔曲线

* 二次贝塞尔曲线

  * context.quadraticCurveTo(x1, y1, x2, y2)
  
  > 不同于arcTo, (x2, y2)就是二次贝塞尔曲线的终止点

* 三次贝塞尔曲线

  * context.bezierCurveTo(x1, y1, x2, y2, x3, y3)
  
  > 三次曲线为两个控制点, 1 和 2
>
  > 可以通过控制两个控制点的位置画出波浪线的效果，还可以绘制花瓣的效果



#### 绘制椭圆

* context.ellipse(x, y, radiusX, radiusY, rotation, startAngle, endAngle, anticlosewise)



## 第十一章	文本渲染

#### 文本属性

* context.font	//	可以设置5个值(最少需要font-size和font-family)

  * font-style

    * normal(默认)
    * italic // 斜体字
    * oblique // 倾斜字体

  * font-variant

    * normal(默认)
    * small-caps // 大写字体写成小写书写模式

  * font-weight

    * lighter
    * normal(默认)  // 400
    * bold  // 700
    * bolder
    * 100~900

  * font-size

    * 20px(默认)
    * 2em
    * 150%
    * xx-small
    * x-small
    * medium
    * large
    * x-large
    * x-large

  * font-family

    > 设置多种字体备选
    >
    > 支持@font-face
    >
    > Web安全字体

  ```javascript
  context.font = 'bold 40px Arial'
  ```

  

#### 字体填充

* context.fillText(string, x, y， [maxlen]) 

  >string是书写的字符串, (x, y)为位置 
  >
  >[maxlen]可选，为string的最大宽度，string太多，会被强制压缩



#### 字体描边

* context.strokeText(string, x, y) 

  > 书写string, 文字有描边



#### 文字水平对齐

* context.textAlign

  * left(默认)
  * center
  * right

  > left为文字左边对齐于文字垂线（起始）



####文字基线

* context.textBaseline
  * top
  * middle
  * bottom
  * alphabetic(默认)  // 基于拉丁字母的语言
  * ideographic     // 基于方块文字, 比如汉字，日文等等(基线位于汉字下方)
  * hanging       // 基于印度语(基线大概在i上的点的位置)



####文本度量

* context.measureText(string).width  
  * 文本的度量，获取即将书写文本的宽度



## 第十二章	阴影

#### 阴影颜色

* context.shadowColor



####水平位移

* context.shadowOffsetX
  * 阴影水平位移，向右为正



#### 垂直位移

* context.shadowOffsetY
  * 阴影垂直位移，向下为正



####模糊程度

* context.shadowBlur
  * 阴影模糊程度  越大越模糊



## 第十三章	全局透明度与全局重叠效果 

#### 全局透明度

* globalAlpha
  * 默认值为1



#### 全局重叠效果

* globalCompositeOperation

  * source-over(默认)
    * 覆盖，source指后绘制的图形，over是指叠在上面
  * destination-over
    * destination指先绘制的图形，这里先绘制的图形在上面

  > globalCompositeOperation属性值一览
  >
  > source-over   destination-over  ligter
  >
  > source-atop   destination-atop  copy
  >
  > source-in    destination-in   xor
  >
  > source-out   destination-out   

* globalCompositeOperation属性值一览
| source-over | destination-over | ligter |
| ----------- | ---------------- | ------ |
| source-atop | destination-atop | copy   |
| source-in | destination-in   | xor |
|source-out|destination-out||

> atop  // 底下的图形会划定一个界限，覆盖在上面的图形超过底下图形的部分会不显示(底下的图形会显示)
>
> in // 底下的图形会划定一个界限，覆盖在上面的图形超过底下图形的部分会不显示(底下的图形不会显示)
>
> out //底下的图形会划定一个界限，覆盖在上面的图形不会显示，超过底下图形的部分会显示(底下的图形不会显示)
>
> ligter // 图形都会显示，但重叠部分的颜色会叠加
>
> copy  // 只显示最后一个图形
>
> xor   // 异或，重叠部分不显示



## 第十四章	拓展补充

#### 剪辑区域

* context.clip()

  * clip之前需要绘制一次封闭的路径，接下来使用clip语句后，后面的内容在前一次封闭路径中显示

  > 即 前一次绘制路径成为绘制环境
  >
  > 注意：若要实现动画效果，如果不用save，restore的话，接下来的绘制环境将仅在clip里面执行



#### 非零环绕原则 

* 图形区域内一点向外引一条射线，射线与图形的路径会有交点，定义一个方向为 1，另一个方向为 -1，若每个交点上的1或-1的值相加的和，为 0 时意为在图形外面，若为非0意为在图形里面，当使用 fill() 时，会对图形里面 (非0区域) 上色，图形外面 (0区域) 不上色



#### isPointInPath

* context.isPointInPath(x, y)
  * 如果(x, y)在上一次绘制的封闭路径内，则返回true



####扩充canvas的绘图函数 

* CanvasRenderingContext2D

  * getContext('2d')的原型对象

  > 可将自己写的绘制函数添加到该对象的原型中



## 第十五章	绘制图像与获取图像像素

### 绘制图像

* context.drawImage(image, dx, dy, dw, dh)

  * dw, dh为可选参数, d 为 destination

  > 一般写在image图像加载完之后，即 image.onload 中

* context.drawImage(image, sx, sy, sw, sh, dx, dy, dw, dh)

  * s 为 source, 为原图像内部的一小块图像的位置

* 离屏canvas

  * context.drawImage(canvas, dx, dy)

  > drawImage中可以传入canvas元素



### 获取图像像素 

####ImageData对象

* imageData = context.getImageData(x, y, w, h)
  
* 含有三个属性值，width、height、data
  
* var pixelData = imageData.data

  > 第x行、第y列的像素
  >
  > i = x * width + y
  >
  > r —— pixelData[4 * i + 0]
  >
  > g —— pixelData[4 * i + 1]
  >
  > b —— pixelData[4 * i + 2]
  >
  > a —— pixelData[4 * i + 3]



####putImageData

* context.putImageData(imageData, dx, dy, dirtyX, dirtyY, dirtyW, dirtyH)

> imageData最后位于(dx + dirtyX, dy + dirtyY)



#### 灰度滤镜

```javascript
var grey = r * 0.3 + g * 0.59 + b * 0.11    //  然后将灰度赋值给每r g b 即可实现灰度滤镜
```



#### 黑白滤镜

* 在灰度滤镜的基础上 value = grey > 255 / 2 ? 255 : 0，然后将value赋值给每r g b 即可



#### 反色滤镜

* 将每个通道上的r g b 赋值 255-原来值，即可



#### 模糊滤镜

* 将一个点的像素值连同周围的8个点，共9个点，计算平均值后赋值给该像素

  > (因为关联到其他的像素点，故需要保留一个修改前的模板图像作参考)
  >
  > 若想更模糊，可设定一个模糊半径R，同样计算半径R内的像素点平均值



#### 马赛克滤镜

* 与模糊滤镜相似，但赋值的不是一个像素，而是一个方格内的所有像素



### 创建imageData 

* imageData = context.createImageData(w, h)
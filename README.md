# Canvas
理解&lt;canvas>元素、 绘制简单的 2D 图形 、使用 WebGL 绘制 3D 图形
## 基本用法
必须先为canvas设置 width 和 height 属性，指定可以绘图的区域大小。出现在开始和结束标签中的内容是后备信息，如果浏览器不支持<canvas>元素，就会显示这些信息。`<canvas id="drawing" width=" 200" height="200">A drawing of something.</canvas> `

* **getContext('2d')**：获取 2D 上下文对象，原点坐标是(0,0)，位于左上角。
* **toDataURL()**：接受一个参数，即图像的 MIME 类型格式，可以导出在<canvas>元素上绘制的图像，但是他有跨域限制。

## 2D 上下文
### 填充和描边
* **fillStyle**：填充颜色。
* **strokeStyle**：描边颜色。
### 绘制矩形
* **fillRect(x,y,w,h)**：用指定填充的颜色来填充绘制的矩形。填充颜色通过 fillStyle 属性指定
* **strokeRect(x,y,w,h)**：用指定描边颜色来描边绘制的矩形。描边颜色通过strokeStyle属性指定。
* **clearRect(x,y,w,h)**：清除画布上的矩形区域。

### 绘制线条
* **lineWidth**：描边线条的宽度。
* **lineCap**：线条末端的形状是平头、圆头还是方头（"butt"、"round"或"square"）
* **lineJoin**：线条相交的方式是圆交、斜交还是斜接（"round"、"bevel"或"miter"）
### 绘制路径
要绘制路径，首先必须调用 beginPath()方法，表示要开始绘制新路径。然后，再通过调用下列方法来实际地绘制路径。创建了路径后，接下来有几种可能的选择。如果想绘制一条连接到路径起点的线条，可以调用closePath()。如果路径已经完成，你想用 fillStyle 填充它，可以调用 fill()方法。另外，还可以调用 stroke()方法对路径描边，描边使用的是 strokeStyle。最后还可以调用 clip()，这个方法可以在路径上创建一个剪切区域。

* **arc(x, y, radius, startAngle, endAngle, counterclockwise)**：以(x,y)为圆心绘制一条弧线，弧线半径为 radius，起始和结束角度（用弧度表示）分别为 startAngle 和endAngle。最后一个参数表示 startAngle 和 endAngle 是否按逆时针方向计算，值为 false表示按顺时针方向计算。
* **arcTo(x1, y1, x2, y2, radius)**：从上一点开始绘制一条弧线，到(x2,y2)为止，并且以给定的半径 radius 穿过(x1,y1)。
* **bezierCurveTo(c1x, c1y, c2x, c2y, x, y)**：从上一点开始绘制一条曲线，到(x,y)为止，并且以(c1x,c1y)和(c2x,c2y)为控制点。
* **lineTo(x, y)**：从上一点开始绘制一条直线，到(x,y)为止。
* **moveTo(x, y)**：将绘图游标移动到(x,y)，不画线。
* **quadraticCurveTo(cx, cy, x, y)**：从上一点开始绘制一条二次曲线，到(x,y)为止，并且以(cx,cy)作为控制点。
* **rect(x, y, width, height)**：从点(x,y)开始绘制一个矩形，宽度和高度分别由 width 和height 指定
* **isPointInPath(x,y)**：用于在路径被关闭之前确定画布上的某一点是否位于路径上

### 绘制文本

* **font**：表示文本样式、大小及字体，用 CSS 中指定字体的格式来指定，例如"10px Arial"。
* **textAlign**：表示文本对齐方式。可能的值有"start"、"end"、"left"、"right"和"center"。建议使用"start"和"end"，不要使用"left"和"right"，因为前两者的意思更稳妥，能同时适合从左到右和从右到左显示（阅读）的语言。
* **textBaseline**：表示文本的基线。可能的值有"top"、"hanging"、"middle"、"alphabetic"、"ideographic"和"bottom"。
* **fillText**(text,x,y,可选的最大像素宽度)：fillText()方法使用
fillStyle 属性绘制文本。
* **strokeText**(text,x,y,可选的最大像素宽度)：strokeText()方法使用 strokeStyle 属性为文本描边。
* **measureText**(text)：确定文本大小。返回一个 TextMetrics
对象。返回的对象目前只有一个 width 属性，但将来还会增加更多度量属性。

### 变换
如果你知道将来还要返回某组属性与变换的组合，可以调用 save()方法。调用这个方法后，当时的所有设置都会进入一个栈结构，得以妥善保管。然后可以对上下文进行其他修改。等想要回到之前保存的设置时，可以调用 restore()方法，在保存设置的栈结构中向前返回一级，恢复之前的状态。连续调用 save()可以把更多设置保存到栈结构中，之后再连续调用 restore()则可以一级一级返回。
* **rotate(angle)**：围绕**坐标原点**旋转图像 angle 弧度。
* **scale(scaleX, scaleY)**：缩放图像，在 x 方向乘以 scaleX，在 y 方向乘以 scaleY。scaleX和 scaleY 的默认值都是 1.0。
* **translate(x, y)**：将**坐标原点**移动到(x,y)。执行这个变换之后，坐标(0,0)会变成之前由(x,y)表示的点。
* **transform(m1_1, m1_2, m2_1, m2_2, dx, dy)**：直接修改变换矩阵，方式是乘以如下矩阵。
    m1_1 m1_2 dx
    m2_1 m2_2 dy
    0 0 1
* **setTransform(m1_1, m1_2, m2_1, m2_2, dx, dy)**：将变换矩阵重置为默认状态，然后再调用 transform()。

### 绘制图像
有以下三种不同的参数组合，t表示目标，s表示源图像。image可以为传入 HTML <img>元素或者可以通过toDataURL()方法获得，toDataURL()有同域限制。

* drawImage(image,tx,ty)
* drawImage(image,tx,ty,tw,th)
* drawImage(image,sx,sy,sw,sh,tx,ty,tw,th)：：要绘制的图像、源图像的 x 坐标、源图像的 y 坐标、源图像的宽度、源图像的高度、目标图像的 x 坐标、目标图像的 y 坐标、目标图像的宽度、目标图像的高度。

### 阴影

* **shadowColor**：用 CSS 颜色格式表示的阴影颜色，默认为黑色。
* **shadowOffsetX**：形状或路径 x 轴方向的阴影偏移量，默认为 0。
* **shadowOffsetY**：形状或路径 y 轴方向的阴影偏移量，默认为 0。
* **shadowBlur**：模糊的像素数，默认 0，即不模糊。

### 渐变

* **createLinearGradient(x1,y1,x2,y2)**：要创建一个新的线性渐变颜色，可以赋值给fillStyle。
* **createRadialGradient(x1,y1,r1,x2,y2,r2)**：创建径向渐变。接收 6 个参
数，对应着两个圆的圆心和半径。前三个参数指定的是起点圆的原心（x 和 y）及半径，后三个参数指定的是终点圆的原心（x 和 y）及半径。可以把径向渐变想象成一个长圆桶，而这 6 个参数定义的正是这个桶的两个圆形开口的位置。如果把一个圆形开口定义得比另一个小一些，那这个圆桶就变成了圆锥体，而通过移动每个圆形开口的位置，就可达到像旋转这个圆锥体一样的效果。
* **addColorStop(色标位置,CSS 颜色值)**：创建了渐变对象后，用它来指定色标，色标位置是一个 0（开始的颜色）到 1（结束的颜色）之间的数字

### 模式

* **createPattern**()：两个参数：一个 HTML <img>元素和一个表示如何重复图像的字符串，包括"repeat"、"repeat-x"、"repeat-y"和"no-repeat"。可以赋值给fillStyle。

### 使用图像数据

* **getImageData(x,y,w,h)**：返回的对象是 ImageData 的实例。每个 ImageData 对象都有三个属性：width、height 和data。其中 data 属性是一个数组，保存着图像中每一个像素的数据。在 data 数组中，每一个像素用4个元素来保存，分别表示红、绿、蓝和透明度值。
* **putImageData(ImageData,x,y)**：把图像数据绘制到画布上。

### 合成

* **globalAlpha**：一个介于 0 和 1 之间的值（包括 0 和 1），用于指定所有绘制的透
明度。默认值为 0。
* **globalCompositionOperation**：表示后绘制的图形怎样与先绘制的图形结合。这个属性的值是字符串，可能的值如下。参考：https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation
    * source-over（默认值）：后绘制的图形位于先绘制的图形上方。
    * source-in：后绘制的图形与先绘制的图形重叠的部分可见，两者其他部分完全透明。
    * source-out：后绘制的图形与先绘制的图形不重叠的部分可见，先绘制的图形完全透明。
    * source-atop：后绘制的图形与先绘制的图形重叠的部分可见，先绘制图形不受影响。
    * destination-over：后绘制的图形位于先绘制的图形下方，只有之前透明像素下的部分才可见。
    * destination-in：后绘制的图形位于先绘制的图形下方，两者不重叠的部分完全透明。
    * destination-out：后绘制的图形擦除与先绘制的图形重叠的部分。
    * destination-atop：后绘制的图形位于先绘制的图形下方，在两者不重叠的地方，先绘制的图形会变透明。
    * lighter：后绘制的图形与先绘制的图形重叠部分的值相加，使该部分变亮。
    * copy：后绘制的图形完全替代与之重叠的先绘制图形。
    * xor：后绘制的图形与先绘制的图形重叠的部分执行“异或”操作。

## WebGL 
WebGL 是针对 Canvas 的 3D 上下文。

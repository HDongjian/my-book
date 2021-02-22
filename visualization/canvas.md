## 页面游戏开发基础

理解并掌握使用画布元素开发游戏的步骤，技巧和开发过程，并能手动的使用画布开发一个页面游戏

### 绘制线条

#### 画布的作用

Canvas元素作为HTML5标准的一部分，允许你通过脚本动态渲染图像，每一个canvas都有一个上下文环境对象，在其中可以绘制任意图形

#### 绘制过程

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制直线</title>
  <style>
    canvas{
      border:1px solid pink;
    }
  </style>
</head>
<body>
  <canvas id="cvs"></canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var cxt = cvs.getContext('2d')

  //定位开始点
  cxt.moveTo(30,30)

  //拖动并结束点
  cxt.lineTo(60,60) //路径保存到内存

  //绘制点
  cxt.stroke()
</script>
</html>
```

### 绘制三角形

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制三角形</title>
  <style>
    canvas{
      border:1px solid pink;
    }
  </style>
</head>
<body>
  <canvas id="cvs"></canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var cxt = cvs.getContext('2d')

  //定位开始点
  cxt.moveTo(50,50)

  //绘制第二个点
  cxt.lineTo(150,50)

  //绘制第三个点
  cxt.lineTo(80,100)

  // cxt.lineTo(50,50) // 连接原点
  cxt.closePath()//形成闭合路径
  //绘制点
  cxt.stroke()
</script>
</html>
```
### 绘制不同线条颜色的三角形

重置路径：`beginPath`开始新的节点，切断当前路径

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制不同线条三角形</title>
  <style>
    canvas{
      border:1px solid pink;
    }
  </style>
</head>
<body>
  <canvas id="cvs"></canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var cxt = cvs.getContext('2d')

  //绘制图形颜色
  cxt.strokeStyle = 'red' // 放在stroke之前即可

  //绘制线条宽度
  cxt.lineWidth = 5
  //定位开始点
  cxt.moveTo(50,50)

  //绘制第二个点
  cxt.lineTo(150,50)
  cxt.stroke()
  //重置路径
  cxt.beginPath()
  //绘制图形颜色
  cxt.strokeStyle = 'blue'
  //绘制第二条线的起始点
  cxt.moveTo(150,50)
  //绘制第三个点
  cxt.lineTo(80,100)
  // 绘制线条
  cxt.stroke()
  //重置路径
  cxt.beginPath()
  //绘制第三个点
  cxt.moveTo(80,100)
  cxt.strokeStyle = 'yellow'
  cxt.lineTo(50,50)
  //绘制点
  cxt.stroke()
</script>
</html>
```

### 文字绘制

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制文字</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs"></canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var cxt = cvs.getContext('2d')

  //设置样式
  cxt.font = 'bold 20px 黑体'

  // 水平对齐方式
  cxt.textAlign = 'center'
  //垂直对齐方式
  cxt.textBaseline = 'bottom'
  //绘制填充文字
  cxt.fillText('画布', 30, 30)

  //绘制线条文字
  cxt.strokeText('画布', 30, 80)
  
  //将绘制的内容保存为图片
  let image = cvs.toDataURL('image/png',1)
  console.log(image)
</script>

</html>
```

### 绘制矩形和圆形和图片

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制文字</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs"></canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var cxt = cvs.getContext('2d')
  //先绘制路径
  //cxt.rect(10,10,50,50) //(x,y,w,h)
  //描边
  // cxt.stroke()
  //填充
  // cxt.fill()

  // 调用一次性方法
  // cxt.strokeRect(10,10,50,50)
  // cxt.fillRect(10,10,50,50)

  //清楚区域内面积
  // cxt.clearRect(10,10,50,50)

  // 绘制圆形
  // cxt.arc(60,60,50,0,Math.PI,true) // (x,y,r,sAngle,eAngle,clockwise)(起始角度，结束角度，逆时针还是顺时针)
  // cxt.stroke()
  // cxt.fill()
  
  // 绘制图片
  //定义图片对象
  var img = new Image();
  img.src = './1.png'
  img.onload = function(){
    // 绘制图片
    // cxt.drawImage(this,10,10) // (img,x1,y1,w1,h1,x2,y2,w2,h2)
    cxt.drawImage(this,10,10,10,10,10,10,30,30)
  }

</script>

</html>
```
### 实现刮刮乐效果

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制文字</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs"></canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var cxt = cvs.getContext('2d')
  //定义一个中奖信息的数组
  var arrGisf = ['一等奖', '二等奖', '三等奖', '谢谢惠顾', ]
  //随机生成一个中奖信息
  var rdmgif = arrGisf[Math.floor(Math.random() * arrGisf.length)]
  cxt.font = 'bold 25px 黑体'
  cxt.textAlign = "center"
  cxt.textBaseLine = 'middle'
  cxt.fillStyle = 'red'

  //将获取的中奖信息绘制到画布
  cxt.fillText(rdmgif, cvs.width / 2, cvs.height / 2)

  //将绘制的中奖信息保存为图片并作为画布元素的背景图片
  var dataUrl = cvs.toDataURL('image/png', 1)
  cvs.style.background = 'url(' + dataUrl + ')'

  //绘制一个与画布同高宽的矩形覆盖中奖信息
  cxt.clearRect(0, 0, cvs.width, cvs.height)

  //设置矩形的颜色
  cxt.fillStyle = '#ddd'

  //绘制覆盖的区域
  cxt.fillRect(0, 0, cvs.width, cvs.height)

  //设置一个变量控制出发

  var flag = false

  cvs.addEventListener('mousedown', function () {
    flag  = true
    //将绘制区域设为透明
    cxt.globalCompositeOperation = 'destination-out'
  })
  cvs.addEventListener('mousemove', function (e) {
    if(!flag) return
    var x = e.clientX
    var y = e.clientY
    cxt.fillStyle = 'red'
    cxt.fillRect(x,y,10,10)
  })
  cvs.addEventListener('mouseup', function () {
    flag  = false
  })
</script>

</html>
```
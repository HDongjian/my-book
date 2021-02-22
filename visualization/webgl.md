## 页面高性能图形编程

>掌握在页面中使用WebGL绘制3D效果的图形

### 课程介绍

#### WebGL

WebGL解决了用户在页面中绘制和渲染3D图形的功能，且使用通过页面与三维交互成为课程，这项技术将在下一代开发用户易用且直观的界面中发挥重要作用

#### 技术栈

- HTML HTML5
- JavaScript
- GLSL ES语言

### 初始WebGL

#### 绘制WebGL图形

- 添加画布元素
- 获取到画布元素的基于WebGL的上下文对象
- 使用对象中的API方法实现图形的绘制

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制图形</title>
  <style>
    canvas{
      border:1px solid pink;
    }
  </style>
</head>
<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')

  //设置绘制图形的颜色
  gl.clearColor(1.0, 0.0, 0.0, 1.0)

  //调用缓存中的值填充图形
  gl.clear(gl.COLOR_BUFFER_BIT)
</script>
</html>
```

#### 使用着色器绘制图形

> 着色器是使用OpenGL ES Shading Language语言编写的程序，负责记录像素点的位置和颜色，并由顶点着色器和片段着色器组成，通过GLSL编写这些着色器，并将代码文本传递给WebGL执行时编译

- 顶点着色器：是将输入顶点从原始坐标转换到WebGL使用的缩放空间坐标系，每个轴的坐标范围从-1到0到1，顶点着色器对顶点坐标进行必要的转换后，保存在名称为gl_Position的特殊变量中备用
- 片段着色器：是在顶点着色器处理完图形的顶点后，会被要绘制的每个图形的每个像素点调用一次，它的功能是确定像素的颜色值，并保存到名称为gl_FragColor的特殊变量中，改颜色值将最终绘制到图形像素的对应位置中


```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>绘制图形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'void main() {' +
    '  gl_Position = vec4(0.1, 0.0,0.0,1.0); ' + //定义点的坐标并转换成变量保存
    '  gl_PointSize = 10.0; ' + //设置缩放距离的直径
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'void main() {' +
    '  gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  // 绘制指定位置图形
  gl.drawArrays(gl.POINTS, 0, 1) //(绘制的类型，开始的位置，结束的位置)
</script>

</html>
```

### 绘制三角形

#### 多点绘制的方法

- 什么attribute变量
    - 它是一种存储限定符，表示定义一个attribute的全局变量，这种变量的数据将由外部向顶点着色器内传输，并保存顶点相关的数据，只有顶点着色器才能使用它
- 使用attribute变量
    - 在顶点着色器中，声明一个attribute变量
    - 将attribute变量赋值给gl_Position变量
    - 向attribute变量传输数据
- 使用缓存区关联attribute变量
    - 创建缓存区对象 
    - 绑定缓存区对象
    - 将数据写入对象
    - 将缓存区对象分配给attribute变量
    - 开启attribute变量

#### 绘制三角形方法

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>着色器绘制三角形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'attribute vec4 a_Position;' + //使用存储限定符定义一个接受顶点坐标的变量
    'void main() {' +
    '  gl_Position = a_Position; ' + //定义点的坐标并转换成变量保存
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'void main() {' +
    '  gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  //定义一个类型数组保存顶点坐标值
  var vertices = new Float32Array([ // 类型数组代表的是平台字节顺序为32位的浮点数型数组
    0.0,0.5,
    -0.5,-0.5,
    0.5,-0.5
  ])
  // 先创建一个缓存对象
  var vertexBuffer = gl.createBuffer()
  // 说明缓存对象保存的类型
  gl.bindBuffer(gl.ARRAY_BUFFER,vertexBuffer)
  // 写入坐标数据
  gl.bufferData(gl.ARRAY_BUFFER,vertices,gl.STATIC_DRAW) //（类型，数据，格式）
  // 获取到顶点着色器的中的变量
  var a_Position = gl.getAttribLocation(shaderProgram,'a_Position') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Position,2,gl.FLOAT,false,0,0)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Position)

  // 绘制指定位置图形
  gl.drawArrays(gl.TRIANGLES, 0, 3) //(绘制的类型，开始的位置，结束的位置)
</script>

</html>
```
### WebGL动画

#### 图形的移动

- 平移原理
    - 为了平移一个三角形，只需要对他的每个顶点进行移动，即每个顶点加一个分量，得到一个新的坐标
    - 只需要着色器中为顶点坐标的没规格分量加一个常量就可以实现，当然这修改在顶点着色器上
- uniform类型变量
    - 用于保存和传输一直的类型，即可用于顶点，也可用于片断

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>2-1-平移三角形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'attribute vec4 a_Position;' + //使用存储限定符定义一个接受顶点坐标的变量
    'uniform vec4 u_Translation;' + //使用存储限定符定义一个接受一致偏移量的值
    'void main() {' +
    '  gl_Position = a_Position + u_Translation; ' + //定义点的坐标并转换成变量保存
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'void main() {' +
    '  gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  //定义一个类型数组保存顶点坐标值
  var vertices = new Float32Array([ // 类型数组代表的是平台字节顺序为32位的浮点数型数组
    0.0, 0.5,
    -0.5, -0.5,
    0.5, -0.5
  ])
  // 先创建一个缓存对象
  var vertexBuffer = gl.createBuffer()
  // 说明缓存对象保存的类型
  gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer)
  // 写入坐标数据
  gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW) //（类型，数据，格式）
  // 获取到顶点着色器的中的变量
  var a_Position = gl.getAttribLocation(shaderProgram, 'a_Position') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Position, 2, gl.FLOAT, false, 0, 0)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Position)


  // 定义个坐标点的统一偏移量
  var Tx = 0.2,
    Ty = 0.3,
    Tz = 0.0
  // 获取到顶点着色器中的uniform变量
  var u_Translation = gl.getUniformLocation(shaderProgram, 'u_Translation')
  // 将多个偏移量赋值给uniform变量
  gl.uniform4f(u_Translation, Tx, Ty, Tz, 0.0)

  // 绘制指定位置图形
  gl.drawArrays(gl.TRIANGLES, 0, 3) //(绘制的类型，开始的位置，结束的位置)
</script>

</html>
```

#### 图形的旋转

- 旋转轴：是X轴还是Y轴
- 旋转的方向：顺时针还是逆时针，负值为顺时针，正值为逆时针
- 旋转的角度：图形经过的角度

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>旋转三角形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'attribute vec4 a_Position;' + //使用存储限定符定义一个接受顶点坐标的变量
    'uniform float u_CosB, u_SinB;' +
    'void main() {' +
    'gl_Position.x = a_Position.x * u_CosB - a_Position.y * u_SinB;' +
    'gl_Position.y = a_Position.x * u_SinB + a_Position.y * u_CosB;' +
    'gl_Position.z = a_Position.z;' +
    'gl_Position.w  = 1.0;' +
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'void main() {' +
    '  gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  //定义一个类型数组保存顶点坐标值
  var vertices = new Float32Array([ // 类型数组代表的是平台字节顺序为32位的浮点数型数组
    0.0, 0.5,
    -0.5, -0.5,
    0.5, -0.5
  ])
  // 先创建一个缓存对象
  var vertexBuffer = gl.createBuffer()
  // 说明缓存对象保存的类型
  gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer)
  // 写入坐标数据
  gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW) //（类型，数据，格式）
  // 获取到顶点着色器的中的变量
  var a_Position = gl.getAttribLocation(shaderProgram, 'a_Position') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Position, 2, gl.FLOAT, false, 0, 0)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Position)

  // 设置需要旋转的角度
  var ANGLE = 90.0
  // 角度转成弧度用于函数的计算
  var radian = Math.PI * ANGLE / 180.0
  // 计算并保存正弦和余弦的值
  var cosB = Math.cos(radian)
  var sinB = Math.sin(radian)

  // 从顶点着色器中分别取出变量的物理地址并保存
  var u_CosB = gl.getUniformLocation(shaderProgram, 'u_CosB')
  var u_SinB = gl.getUniformLocation(shaderProgram, 'u_SinB')

  // 将保存好的函数值赋值给变量
  gl.uniform1f(u_CosB, cosB) //（旧的，新的）
  gl.uniform1f(u_SinB, sinB)

  // 绘制指定位置图形
  gl.drawArrays(gl.TRIANGLES, 0, 3) //(绘制的类型，开始的位置，结束的位置)
</script>

</html>
```

#### 图形的缩放

- 通过改变原有图形中的矩阵值，实现图形的拉大和缩小效果

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>缩放三角形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'attribute vec4 a_Position;' + //使用存储限定符定义一个接受顶点坐标的变量
    'uniform mat4 u_xformMatrix;' + // mat4 矩阵类型
    'void main() {' +
    '  gl_Position = a_Position * u_xformMatrix; ' + //定义点的坐标并转换成变量保存
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'void main() {' +
    '  gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  //定义一个类型数组保存顶点坐标值
  var vertices = new Float32Array([ // 类型数组代表的是平台字节顺序为32位的浮点数型数组
    0.0, 0.5,
    -0.5, -0.5,
    0.5, -0.5
  ])
  // 先创建一个缓存对象
  var vertexBuffer = gl.createBuffer()
  // 说明缓存对象保存的类型
  gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer)
  // 写入坐标数据
  gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW) //（类型，数据，格式）
  // 获取到顶点着色器的中的变量
  var a_Position = gl.getAttribLocation(shaderProgram, 'a_Position') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Position, 2, gl.FLOAT, false, 0, 0)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Position)

  // 设置缩放的距离值
  var Sx = 0.5,
    Sy = 0.5,
    Sz = 1.0
  // 定义一个4*4的矩阵
  var xformMatrix = new Float32Array([
    Sx, 0.0, 0.0, 0.0,
    0.0, Sy, 0.0, 0.0,
    0.0, 0.0, Sz, 0.0,
    0.0, 0.0, 0.0, 1.0,
  ])
  //获取到顶点着色器中的矩阵变量
  var u_xformMatrix = gl.getUniformLocation(shaderProgram, 'u_xformMatrix')
  // 将设置的值赋值给变量
  gl.uniformMatrix4fv(u_xformMatrix, false, xformMatrix)

  // 绘制指定位置图形
  gl.drawArrays(gl.TRIANGLES, 0, 3) //(绘制的类型，开始的位置，结束的位置)
</script>

</html>
```

#### 图形的动画旋转

- 屏幕刷新频率
    - 图形在屏幕上的更新的速度，也即屏幕上的图像每秒钟出现的次数，一般是60HZ的屏幕一般16.7ms刷新一次 
- 动画原理
    - 图像被刷新时，引起以连贯的，平滑的方式进行过度变化 
- 核心方法

```js
requestAnimationFrame(callback)
//执行一个动画，并在下次给绘制前调用callback回调更新动画
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>动画旋转三角形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'attribute vec4 a_Position;' + //使用存储限定符定义一个接受顶点坐标的变量
    'uniform float u_CosB, u_SinB;' +
    'void main() {' +
    'gl_Position.x = a_Position.x * u_CosB - a_Position.y * u_SinB;' +
    'gl_Position.y = a_Position.x * u_SinB + a_Position.y * u_CosB;' +
    'gl_Position.z = a_Position.z;' +
    'gl_Position.w  = 1.0;' +
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'void main() {' +
    '  gl_FragColor = vec4(0.0, 1.0, 0.0, 1.0);' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  //定义一个类型数组保存顶点坐标值
  var vertices = new Float32Array([ // 类型数组代表的是平台字节顺序为32位的浮点数型数组
    0.0, 0.5,
    -0.5, -0.5,
    0.5, -0.5
  ])
  // 先创建一个缓存对象
  var vertexBuffer = gl.createBuffer()
  // 说明缓存对象保存的类型
  gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer)
  // 写入坐标数据
  gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW) //（类型，数据，格式）
  // 获取到顶点着色器的中的变量
  var a_Position = gl.getAttribLocation(shaderProgram, 'a_Position') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Position, 2, gl.FLOAT, false, 0, 0)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Position)

  // 设置需要旋转的角度
  var ANGLE = 90.0

  function draw(ANGLE) {
    // 角度转成弧度用于函数的计算
    var radian = Math.PI * ANGLE / 180.0
    // 计算并保存正弦和余弦的值
    var cosB = Math.cos(radian)
    var sinB = Math.sin(radian)

    // 从顶点着色器中分别取出变量的物理地址并保存
    var u_CosB = gl.getUniformLocation(shaderProgram, 'u_CosB')
    var u_SinB = gl.getUniformLocation(shaderProgram, 'u_SinB')

    // 将保存好的函数值赋值给变量
    gl.uniform1f(u_CosB, cosB) //（旧的，新的）
    gl.uniform1f(u_SinB, sinB)

    // 绘制指定位置图形
    gl.drawArrays(gl.TRIANGLES, 0, 3) //(绘制的类型，开始的位置，结束的位置)
  }

  // 计算每秒钟绘制的角度
  // 获取旋转前时间
  var cur_time = Date.now()
  // 旋转角度（度/秒）
  var ANGLE_STEP = -60.0
  // 初始状态角度值
  var ANGLE_INIT = 20.0
  // 执行时的角度值
  var ANGLE_ACT = 0.0

  function animate(c1, a1, a2) {
    //计算距离上次经过多长时间
    var act_time = Date.now()
    // 得到这次调用和上一次调用的时间间隔
    var dif_time = act_time - c1
    c1 = act_time
    var ANGLE_NEW = a1 + a2 * (dif_time / 1000.0);
    // 返回一个始终小于360°的角度
    return ANGLE_NEW %= 360
  }

  function tick() {
    //获取每次旋转的角度
    ANGLE_ACT = animate(cur_time, ANGLE_INIT, ANGLE_STEP)
    draw(ANGLE_ACT)
    // setInterval(tick, 100);
    window.requestAnimationFrame(tick)
  }
  tick()
</script>

</html>
```

### WebGL颜色

#### 操作步骤介绍

- 在顶点着色器中定义一个接受外部传入颜色变量a_Color和用于传输到的颜色值变量v_Color
- 在片段着色器中定义个同一类型的v_Color变量接受传入顶点的值
- 重新传入到顶点坐标和颜色值的类型化数组
- 将数组值传入缓存中并取出，赋值给顶点的两个变量
- 接受缓存值并绘制图形和颜色

#### vertexAttribPointer方法


参数 | 说明
---|---
第1个参数 | 指定待分配attribute变量的存储位置
第2个参数 | 指定缓存区中每个顶点的分量个数
第3个参数 | 类型有，无符号字节，短整数，无符号短整数，整型，无符号整型，浮点型
第4个参数 | 表示是否将非浮点型的数据归到0.1区间
第5个参数 | 相邻两个顶点的字节数，默认为0
第6个参数 | 表示缓存区对象的偏移量（以字节为单位），attribute变量从缓存区中的何处开始存储

#### 着色器编译和图像绘制

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>多顶点颜色三角形</title>
  <style>
    canvas {
      border: 1px solid pink;
    }
  </style>
</head>

<body>
  <canvas id="cvs" width="200" height="200">你的浏览器不支持画布元素</canvas>
</body>
<script>
  var cvs = document.getElementById('cvs')
  // 获取工具集
  var gl = cvs.getContext('webgl')
  // 顶点着色器
  var VSHADER_SOURCE =
    'attribute vec4 a_Position;' + //使用存储限定符定义一个接受顶点坐标的变量
    'attribute vec4 a_Color;' +
    'varying vec4 v_Color;' +
    'void main() {' +
    '  gl_Position = a_Position; ' + //定义点的坐标并转换成变量保存
    '  v_Color = a_Color; ' +
    '} '

  //片段着色器
  var FSHADER_SOURCE =
    'precision mediump float;' +
    'varying vec4 v_Color;' +
    'void main() {' +
    '  gl_FragColor = v_Color ;' +
    '}'

  // 新建一个用于装载顶点字符串的着色器对象
  var vertShader = gl.createShader(gl.VERTEX_SHADER)
  // 加载保存好的顶点变量
  gl.shaderSource(vertShader, VSHADER_SOURCE)
  //编译顶点着色器
  gl.compileShader(vertShader)

  // 新建一个用于装载片段字符串的着色器对象
  var fragShader = gl.createShader(gl.FRAGMENT_SHADER)
  //加载保存好的片段代码字符串变量
  gl.shaderSource(fragShader, FSHADER_SOURCE)
  //编辑片段着色器
  gl.compileShader(fragShader)

  //新建一个程序
  var shaderProgram = gl.createProgram()
  // 分别附加两个已编译好的着色器对象
  gl.attachShader(shaderProgram, vertShader)
  gl.attachShader(shaderProgram, fragShader)

  // 连接两个附加好的着色器程序
  gl.linkProgram(shaderProgram)
  // 开启程序的使用
  gl.useProgram(shaderProgram)

  //定义一个类型数组保存顶点坐标值
  var vertices = new Float32Array([ // 类型数组代表的是平台字节顺序为32位的浮点数型数组
    0.0, 0.5, 1.0, 0.0, 0.0,
    -0.5, -0.5, 0.0, 1.0, 0.0,
    0.5, -0.5, 0.0, 0.0, 1.0
  ])
  // 先创建一个缓存对象
  var vertexBuffer = gl.createBuffer()
  // 说明缓存对象保存的类型
  gl.bindBuffer(gl.ARRAY_BUFFER, vertexBuffer)
  // 写入坐标数据
  gl.bufferData(gl.ARRAY_BUFFER, vertices, gl.STATIC_DRAW) //（类型，数据，格式）

  //获取到数组中单个元素的字节
  var FSIZE = vertices.BYTES_PER_ELEMENT

  // 获取到顶点着色器的中的变量
  var a_Position = gl.getAttribLocation(shaderProgram, 'a_Position') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Position, 2, gl.FLOAT, false, FSIZE*5, 0)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Position)

  // 获取到顶点着色器的中的变量
  var a_Color = gl.getAttribLocation(shaderProgram, 'a_Color') //变量位置
  // 将坐标值赋值给变量
  gl.vertexAttribPointer(a_Color, 3, gl.FLOAT, false, FSIZE*5, FSIZE*2)
  // 开启变量值得使用
  gl.enableVertexAttribArray(a_Color)

  // 绘制指定位置图形
  gl.drawArrays(gl.TRIANGLES, 0, 3) //(绘制的类型，开始的位置，结束的位置)
</script>

</html>
```
# webgl基础
webgl(web graphics library)，通常来说被认为是3d API。实际上， webgl只是一个光栅化引擎，它基于我们的代码绘制点、线、三角。通过这些基础的图形组合起来完成我们的需求。
webgl运行在电脑的gpu，所以我们的代码也运行在gpu中。需要注意的是我们需要提供成对的函数给webgl，这两个函数分别被称为顶点着色器(vertex shader)和片段着色器(fragment shader)，书写他们的语言被称为GLSL(GL shader language)，语法类似C/C++。两个一起被称为一个程序。
顶点着色器的任务是计算顶点位置，webgl基于这些位置光栅化多种原始图形比如点、线、三角等。当光栅化这些原始图形的时会调用片段着色器，片段着色器的任务是为当前正在渲染的原始图形的每个像素点计算颜色。
几乎所有的webgl api都关于这些成对的函数运行设置状态。在绘制一些图形的时候我们会设置很多的状态，然后在gpu中通过`gl.drawArrays`或者`gl.drawELements`来执行成对的函数（shader）。
# 数据处理
当我们需要我们的方法访问一些数据的时候，必须把这些数据提供给gpu，下面会介绍四种shader获取数据的方法。
## attributes、buffers和vertex array。
`buffers`是一个二进制数据的数组，通常包含位置(positions)，法线(normals)，纹理坐标(texture coordinate)以及定点颜色(vertex color)。用来保存我们需要的信息。
`attributes`用来规定如何从`buffers`中获取数据。比如我们需要`attribute`来指定从buffer的哪个位置开始获取数据以及获取数量等。
## uniforms(全局变量)
全局变量在shader程序执行前声明，运行中全局有效。
## textures(纹理)
纹理数据是在shader程序中可被随机访问的数组，大多数情况都是把图片数据放入纹理中，但是纹理只是数据，可以很轻松的存放颜色以外的数据。
## 可变量(varyings)
可变量是顶点着色器向片段着色器传递数据的一种方式。这取决于什么正在被渲染如点，线或者三角形，通过顶点着色器设置的可变量会在执行片段着色器时获得不同的插值。





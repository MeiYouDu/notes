# document object model
类似的这样一个dom结构
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649688818963-6f71257e-0633-4e2d-825e-3b2e7aa2b652.png#averageHue=%23f1f1f1&clientId=u11c87387-2760-4&from=paste&id=u91a2ecf8&originHeight=632&originWidth=1964&originalType=url&ratio=1&rotation=0&showTitle=false&size=132281&status=done&style=none&taskId=u9f555089-4445-4210-825e-108e0b27c4c&title=)
可以通过这样的dom树来表示。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649688831681-783f0631-2cca-4c0d-9f91-139b3c8ad433.png#averageHue=%23fdfcfc&clientId=u11c87387-2760-4&from=paste&id=ufeed2967&originHeight=800&originWidth=1446&originalType=url&ratio=1&rotation=0&showTitle=false&size=258480&status=done&style=none&taskId=uc8063a61-c193-46f8-805a-59552cc90fa&title=)

   - element，dom的元素。
   - 根结点：node tree的顶层。在html文件中顶层节点总是html元素。其他的svg，xml文件有不同的根结点。
   - 子节点：一个节点在一个另一个节点的内部。
   - descendent，后代节点：就上上面图片中展示的那样，img是setion的子节点，是body节点的后代节点。
   - 父节点。
   - sibling兄弟节点。
   - text文本节点：一个包含文本字符串的节点。解释是换行也会产生文本节点。

上面这个domtree用形象的方式展示了抽象dom结构。（ast树是不是也可以用这种方式展示。）
# fetch data from server
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649688816492-40883a38-5c06-4303-9010-fa88f94b6ef2.png#averageHue=%23dac39b&clientId=u11c87387-2760-4&from=paste&id=u1d96e058&originHeight=766&originWidth=1124&originalType=url&ratio=1&rotation=0&showTitle=false&size=126782&status=done&style=none&taskId=u6e13c6e4-1d74-4444-afc6-01167c2f38a&title=)
上面这个图展示了更好的获取数据的方式。
db缓存数据，如果服务端数据没有更新，则返回缓存的数据，如果服务端数据更新了，则请求数据后缓存并吧最新的数据返回。
# drawing graphics
主要讲述怎样通过canvas绘制二维图形，三维图形需要专门学习webgl。有时间在学习图形学opengl相关的知识。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649688817268-09d209e1-c6e9-4e39-80e5-120746a2a782.png#averageHue=%232a2d39&clientId=u11c87387-2760-4&from=paste&id=udec4d65e&originHeight=650&originWidth=988&originalType=url&ratio=1&rotation=0&showTitle=false&size=75873&status=done&style=none&taskId=ufc562f58-d533-4d02-beb0-b3c49522a01&title=)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649688818968-a9f3f8a6-644e-4e1c-919a-782330dace24.png#averageHue=%232b2c3a&clientId=u11c87387-2760-4&from=paste&id=u1e824588&originHeight=576&originWidth=976&originalType=url&ratio=1&rotation=0&showTitle=false&size=109539&status=done&style=none&taskId=u48061238-23a7-4736-9dcd-2a9bb64a072&title=)
注意：通常情况下通过html标签或者操作dom属性来设置图片的size，可以用css来设置，但是设置size会在画布渲染之后被设置，这会导致画布图片像素化和扭曲。
## 2d canvas basics
所有的图形绘制都是通过操作[CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D) 这个对象，他通过canvas.getContext("2d")来获取。绘制图形的时候需要提供坐标告诉他我们要在什么位置开始绘制图形。用从左到右和从上到下的坐标绘制点。
绘制图形的时候需要注意，图形之间会彼此覆盖，而且我们没办法改变它，所以需要考虑清楚绘制图形的命令的执行顺序。
## 绘制矩形和边框（stroke）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649688841746-56deb221-1be1-4bf8-8218-919b8c8fdc0a.png#averageHue=%232c2d3b&clientId=u11c87387-2760-4&from=paste&id=u07fe2bd6&originHeight=1594&originWidth=950&originalType=url&ratio=1&rotation=0&showTitle=false&size=288520&status=done&style=none&taskId=u8756a1e6-bdff-4df7-8240-cd4f250c445&title=)
## 绘制路径
如果想绘制比矩形更复杂的图形的话就需要通过绘制路径来实现了。
canvas包含绘制直线、圆、贝塞尔曲线等方法。

- [beginPath()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/beginPath) — start drawing a path at the point where the pen currently is on the canvas. On a new canvas, the pen starts out at (0, 0).（笔当前在画布上的位置，表示开始绘制点，如果创建一个新的画布，这个点会以（0，0）为起点）。
- [moveTo()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/moveTo) — move the pen to a different point on the canvas, without recording or tracing the line; the pen "jumps" to the new position.（直接让画笔跳到这个点而不会描绘和记录这这条线）。
- [fill()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fill) — draw a filled shape by filling in the path you've traced so far.（用你之前描绘的路径绘制一个实心的图形）。
- [stroke()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/stroke) — draw an outline shape by drawing a stroke along the path you've drawn so far.（根据之前描绘的路径绘制一个空心的图形）。
- You can also use features like lineWidth and fillStyle/strokeStyle with paths as well as rectangles.（可以在路径上使用这些特性就像在矩形上）。
# client-side storage

- 过去：经常使用cookies来缓存用户数据，id，token等信息。
- 现在：经常使用web storage API，indexedDB API来实现。
   - webstorage API：语法简单，用来存放和获取简单的数据，例如：名字、登陆信息、主题颜色等。
   - indexedDB API：提供了一个完整的浏览器端数据库系统，用来保存复杂的数据，比如完整的用户数据或者音视频文件等。
## 缓存api特征
一些现代的浏览器支持新的缓存api，这些api被设计用来缓存http请求的返回值，对于离线状态下非常有作用。缓存api通常和web worker api组合使用。
## webstorage API
webstorage的语法就不再重复了，需要注意的是缓存的数据会存在不同的域（不同的网址不能访问到彼此缓存的数据）。
## indexedDB API
使用方法类似web worker，详细的使用教程。

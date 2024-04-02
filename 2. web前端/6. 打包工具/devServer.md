webpack创建一个开发环境的服务，把代码后的代码运行在这个服务上
# Watch
```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "build": "webpack --config ./webpack.config.js",
  "type-check": "tsc",
  "watch": "webpack --watch"
},
```
## 缺点

1. 每次修改代码需要对所有的代码都进行编译。
2. 编译后或生成新的文件。需要进行文件操作
3. 需要配合vs-code的live-server使用，这并不是webpack推荐的解决方案，live-server会刷新整个页面
4. Webpack-dev-server
## 优点

1. 不生成新的编译之后的代码文件，会通过memfs把编译后的代码放在内存中，然后通过node服务加载
2. 可以利用HMR热模块更新，不需要每次更新代码就刷新整个项目
3. Webpack-dev-middleware通过这个中间件配合express搭建的一个开发模式服务。
# HMR原理
Webpack-dev-server提供了两个服务

1. express做静态资源服务
2. socket服务

服务器监听到文件变化之后会生两个文件，.json（manifast文件）和.js（update chunk）
通过长链接将文件主动发送给客户端
浏览器拿到之后通过HMR runtime机制加载两个文件。并针对修改的模块进行更新。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649653916840-423ed47d-00ea-4a4b-8aea-5a5725a78452.png#averageHue=%23b9dae7&clientId=u8db3b1cb-3261-4&from=paste&id=u4522fe6c&originHeight=992&originWidth=1481&originalType=url&ratio=1&rotation=0&showTitle=false&size=492119&status=done&style=none&taskId=ua5515583-7bc8-4c6e-a428-bf5746a475a&title=)
# Proxy的一些细节
changeOrigin,因为有些后台服务器会做防止代理爬取数据的功能，如果用代理的服务请求之后，后台会校验返回的host是否跟请求的一致，通过changeOrigin会把返回的host替换成代理的host。也就绕开了后端的校验

# 2022-05-07
关于江涛上午给我打电话问的关于服务端的websocket服务关闭（类似情况），前端怎么是实现当服务端websocket重启之后再次重连。
## 解决方案
### 大致思路
websocket断开（服务端关闭），前端可以通过这个webapi监听到。
```javascript
// WebSocket.onclose
// WebSocket.onclose 属性返回一个事件监听器，这个事件监听器将在 WebSocket 连接的readyState 变为 CLOSED时被调用，它接收一个名字为“close”的 CloseEvent 事件。
WebSocket.onclose = function(event) {
  console.log("WebSocket is closed now.");
};
```

1. 当监听到关闭事件之后，我们可以开启一个轮询，轮询中要访问服务端（http连接），检测服务端是否恢复（服务端不启动，我们肯定报错）。
2. 当连接上之后，服务端还要确认websocket服务是否启动（如果是微服务，有可能不在一个服务上，多个服务是逐步启动的。）。
3. 服务端返回ok，我们再发起websocket连接。（重连成功）
### 例子
其实可以参考我们前端开发中各种开发工具的做法。比如：webpack、vite、rollup等。下面以webpack作为工程化环境为例。
webpack实现HMR(hot-module-replacement)用到了一个叫webpack-dev-server的库，vue的脚手架也用了这个库。当我们启动开发模式服务时会启动一个
websocket服务。
#### 首次建立连接
当我们通过浏览器访问这个ip地址，浏览器端会做两件事情。

- 发一个http请求，用来跟服务端确认状态，判断websocket服务是否启动。
- 如果服务已经启动，则发起websocket请求。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1651893627741-ad2178e0-c12c-4e90-8df3-b806d851b198.png#clientId=u7ec53ca1-8a12-4&from=paste&height=546&id=ucbe9e3f7&originHeight=1364&originWidth=2424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8960869&status=done&style=none&taskId=u2e04a43a-e6da-4bad-9a54-f520e6f9c60&title=&width=969.6)
#### 连接断开
当服务端关闭
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1651894550882-5c3ce4e2-0a57-4c34-aa77-fa850bd567f2.png#averageHue=%23586063&clientId=ue2c8b67a-0a04-4&from=paste&height=371&id=u79380e98&originHeight=928&originWidth=1938&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4081826&status=done&style=none&taskId=u8ed6812a-f075-490b-85c5-5778fcbaedf&title=&width=775.2)
上图中我关闭了开发模式服务。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1651894626229-cb6f09e5-2bfb-4cd5-a806-f5dcb40e3bc7.png#clientId=ue2c8b67a-0a04-4&from=paste&height=498&id=u80a81cb9&originHeight=1244&originWidth=3229&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10448001&status=done&style=none&taskId=ua9c5657b-bd8c-4ef4-a4ae-b904711ac47&title=&width=1291.6)
然后我在chrome的devtools中可以看到前端在不断的轮询发起一个http请求，这个http请求就是上面提到的用来确认服务端状态。如上图所示：
当我重新启动服务。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1651894698290-3327c4a1-887a-4030-8a6d-c981b547588a.png#clientId=ue2c8b67a-0a04-4&from=paste&height=463&id=u32fec0fb&originHeight=1158&originWidth=1994&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4778474&status=done&style=none&taskId=u1375fe08-7e0c-49bd-9dd4-9730679475d&title=&width=797.6)
服务启动后不要刷新浏览器，然后可以看到下个http请求访问成功，下图所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1651895085195-97167d25-4e2c-4947-930f-1852538ab2dd.png#clientId=ue2c8b67a-0a04-4&from=paste&height=351&id=u6080fe87&originHeight=877&originWidth=3206&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7500886&status=done&style=none&taskId=u48940853-8df4-45d6-8e6b-687c9e0dd38&title=&width=1282.4)
然后可以看到新的websocket被建立了（重连成功），下图所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1651895110644-945485dd-cbbf-4015-84f8-54bb4403e869.png#clientId=ue2c8b67a-0a04-4&from=paste&height=316&id=uccb021c1&originHeight=791&originWidth=3590&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8493539&status=done&style=none&taskId=ue7f0dc31-6bca-4d77-8b39-8ba32c8dd4c&title=&width=1436)
### 最后
这个思路和例子只是实现这个需求的一种方式，不一定是最优解，大家有更好的方法或者问题可以再交流。

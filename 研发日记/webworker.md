## webworker使用（2021-11-24）
1. webworker只接受服务上的路径，而不接受本地文件。也就是说webworker的构造函数传入的url指向的文件必须被服务端代理。(worker创建一个专用Web worker，它只执行URL指定的脚本。使用 [Blob URL](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 作为参数亦可。)
2. webworkder的文件中不能操作dom等，但是可以发起ajax请求，Worker 可以使用 [XMLHttpRequest](https://developer.mozilla.org/en-US/DOM/XMLHttpRequest) 发送请求，但是请求的  responseXML 与 channel 两个属性值始终返回 null （fetch 仍可正常使用，没有类似的限制）。

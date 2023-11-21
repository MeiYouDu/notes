# 简介
`iframe`元素表示一个浏览器上下文，它可以嵌入其他的HTML页面到当前的上下文。
每个被嵌入的浏览器上下文都有它自己的独立的，嵌入了其他的浏览器上下文的上下文被称为父级浏览器上下文，如果没有父级则表示为顶级上下文，顶级上下文通常是浏览器window，又`window`对象表示。
:::info
注意：因为每个浏览器上下文都具有一个完整的document环境，每个`iframe`都需要内存和其他的计算资源，我们可以随便使用`iframe`但是需要注意性能问题。
:::
# 标签
请参考[标签](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attributes)的文档，这里不再赘述
# scripting
行内`iframe`，比如`<iframe>`元素，都被包含在`window.frames`里面（一个数组）。
使用DOM的`HTMLIFrameElement`对象，scripts可以访问通过`contentWindow`属性访问framed资源的`window`对象，`contentDocument`属性类似于`iframe`里面的`document`，就像`contentWindow.document`。
从frame的内部，通过`window.parent`可以获取到它的父级window对象。
使用脚本访问frame的内容遵循同源策略。如果加载的是不同源的资源，从其他的`window`获取到的属性是有限的。在子frame中访问父级frame，不同源的通信可以通过`Window.postMessage`实现。



## keep-alive
## 深层路由
路由层级比较深的时候需要缓存更深层的路由，同时避免深层路由不刷新的bug，需要跳转的时候在router中删除多余的路由信息，只保留根路由信息以及当前路由信息，这样只缓存第二层，缓存比较容易管理。这时页面信息也可以被正常的缓存。（注册一个全局路由守卫，打印to中的match可以获取路由层级，且keep-alive也只根据这个信息来缓存的，所有只要改变这里面的信息，这样keep-alive也就会认为只有两层）。
## incluede和excluede标签
这个组件中有include和exclude两个属性，include 和 exclude prop 允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649739736972-bacbc159-85b5-403e-87d9-527c789092ae.png#averageHue=%23f7f8f7&clientId=ueee66c8e-d139-4&from=paste&id=u3b1ccaf6&originHeight=826&originWidth=1444&originalType=url&ratio=1&rotation=0&showTitle=false&size=317357&status=done&style=none&taskId=ua0377b33-3ede-49ae-bfe3-3eb7c6dc84a&title=)
匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称 (父组件 components 选项的键值)。匿名组件不能被匹配。（需要注意的是，前文中的name指的是组件中的name属性而不是路由中的name属性）。

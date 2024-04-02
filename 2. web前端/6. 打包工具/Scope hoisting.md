作用域提升，让webpack打包之后代码变得更小，且运行速度更快
比如一个函数声明在一个作用域中，通过闭包，又将这个函数再另一个作用域调用。
这样的效率会很低
通过作用域提升。让函数的声明以及调用在同一个作用域。提高代码的执行效率。
这一功**能的实现依赖esmodule对模块的静态分析。**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649654629252-45b64639-eac2-4fe3-b248-2454625de152.png#averageHue=%23ddc29b&clientId=u88525dae-2bdf-4&from=paste&id=u07e93e49&originHeight=478&originWidth=1057&originalType=url&ratio=1&rotation=0&showTitle=false&size=112358&status=done&style=none&taskId=u837a3036-f8eb-4398-aa6b-975aa62eb40&title=)

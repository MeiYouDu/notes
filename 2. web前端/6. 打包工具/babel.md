通常情况下
代码转化成AST树，然后转化成bytecode，之后就可以被引擎解析并运行。
babel也做了类似的事情。
balbel把es6转变成原AST，然后转变成新的AST，最后把新的AST通过codeGenerator生成es5
babel主要的工作流程：

1. Parsing
2. Transiformation
3. Code generation

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649653078911-6465211f-6c45-430e-8588-7248dda09e59.png#averageHue=%23c7d7c5&clientId=u5080e165-81cb-4&from=paste&id=u1585277b&originHeight=678&originWidth=1921&originalType=url&ratio=1&rotation=0&showTitle=false&size=517062&status=done&style=none&taskId=ufa690743-22d1-4360-a381-bc582c1b217&title=)

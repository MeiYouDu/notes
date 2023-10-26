loader其实就是个导出为函数的模块。
同时loader会有两种，一个是default loader一种是pitchloader。
webpack源码中通过递归调用的方式调用两种loader。
Pitch loader按照从上到下的顺序调用，如果是defaultloader则是从下往上调用。
通常情况下都是default loader。
loader函数会有三个参数。content，map,  meta。
# loader执行顺序
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655467489-b509618e-864b-470a-aec2-48cfb8cbd629.png#averageHue=%23dcc195&clientId=u3a425ee3-0fdd-4&from=paste&id=u4a47b0f5&originHeight=1073&originWidth=1106&originalType=url&ratio=1&rotation=0&showTitle=false&size=224772&status=done&style=none&taskId=ua39399aa-66da-450f-bf58-2dc21d1f8d8&title=)
# loader三种返回值的方式。

- 直接return。
- 通过this.callback方法
- 异步loader，通过this.async方法获取callback，然后再用callback返回。
# loader获取option的方法。

- 通过loader-utils这个库
- Webpack5之后内置了this.getOptions。
# loader的option验证

- 通过schema-loader验证。
- 通过this.getOptions中传入json，json格式根据schema-loader推荐的格式一样。

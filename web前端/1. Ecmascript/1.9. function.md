# length属性
函数的length属性，返回该函数没有设置默认值的参数的个数。如果设置了默认值或者参数是rest参数，length都认为不存在这些参数。
# 作用域
函数作用域就不多说了，这里主要解释一下函数参数的作用域。
函数声明的时候，如果参数中有默认值，则函数声明初始化的时候有会生成一个单独的作用域，等到初始化结束这个作用域就会消失。如果不设置默认值，这种语法行为不会出现。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726598844-4fa77246-7665-479f-8821-ec202dd50738.png#averageHue=%232e3141&clientId=ud6c5676d-4444-4&from=paste&id=ue5ca7a06&originHeight=410&originWidth=1544&originalType=url&ratio=1&rotation=0&showTitle=false&size=124291&status=done&style=none&taskId=u74fc3d2e-fe74-43b2-ad92-c19571bb7c1&title=)
下面是对这一特性的应用：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726598845-462c4ee8-e9fe-42e8-9268-5701209691f5.png#averageHue=%23292c39&clientId=ud6c5676d-4444-4&from=paste&id=u23ffead9&originHeight=362&originWidth=1330&originalType=url&ratio=1&rotation=0&showTitle=false&size=70043&status=done&style=none&taskId=u84b1a122-8593-4d36-9019-dcb75fdcf10&title=)
默认值直接指向函数的调用。如果入参为空则会报错。
# rest参数
rest参数必须是该函数的最后一个参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726598742-18ed6581-6aa0-4182-b95f-22bd53704b47.png#averageHue=%23333546&clientId=ud6c5676d-4444-4&from=paste&id=u70ec75d5&originHeight=228&originWidth=548&originalType=url&ratio=1&rotation=0&showTitle=false&size=37043&status=done&style=none&taskId=u64f607c1-aee3-471d-bb72-8ea19f7d401&title=)
# 严格模式
es6之后函数体内部可以设置严格模式，但是以下情况不能在函数体内部设置严格模式：

   - 函数参数中有默认值，如果在函数体内部设置严格模式则会报错。
   - 函数参数中有rest参数。
   - 函数使用了扩展运算符。

因为：严格模式在函数参数的作用域和函数体中都生效，函数是先执行参数然后在执行函数体，只有运行到函数体部分才会知道是否执行了严格模式，这是不合理的。
用下面两种方式可以规避这一问题：

   1. 全局的严格模式
   2. 把原函数包裹在一个无参数的立即执行函数里面，在这个立即执行函数的函数体里面使用严格模式

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726598770-dc37532d-9c0a-4610-bd94-6722696c2c53.png#averageHue=%232e313e&clientId=ud6c5676d-4444-4&from=paste&id=ub7c369a1&originHeight=264&originWidth=640&originalType=url&ratio=1&rotation=0&showTitle=false&size=35547&status=done&style=none&taskId=u575136b2-6a1c-4b33-9057-b02bfdda503&title=)
# arrow箭头函数
箭头函数用的很多，主要记录一些细节。

   - 箭头函数的箭头指向一个值可以表示return这个值，但是箭头函数如果想返回一个对象，不能直接用箭头指向这个对象，因为对象的大括号会被引擎识别成一个块级作用域，然后把大括号内部的东西当作语句执行。所以如果想实现这个需求可以用小括号包裹这个对象。如下：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726598787-274a2fd5-829a-4e8e-b4bf-a63d11ed0620.png#averageHue=%23292b38&clientId=ud6c5676d-4444-4&from=paste&id=ubf2ec241&originHeight=56&originWidth=450&originalType=url&ratio=1&rotation=0&showTitle=false&size=11711&status=done&style=none&taskId=u99a890db-2d4d-45c3-8119-a0a5138eb53&title=)

   - 箭头函数没有自己的this对象。
   - 箭头函数不能当作构造函数（因为他没有this对象）。
   - 箭头函数内部没有arguements对象，为了实现同样的效果可以利用rest参数代替。
   - 箭头函数内部不能使用yield指令，所以也就不能当作Generate函数。
   - 对象中声明函数最好不要用箭头函数，箭头函数中的this并不指向该对象。
# 尾调用
函数执行的最后一步如果是调用函数则被称为尾调用。
如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726599367-cd2d44f7-8d6f-4f47-a51b-1db55dc402e1.png#averageHue=%23292b38&clientId=ud6c5676d-4444-4&from=paste&id=ued3391a2&originHeight=276&originWidth=250&originalType=url&ratio=1&rotation=0&showTitle=false&size=20667&status=done&style=none&taskId=ue34daaaa-a8ff-4bb2-b00c-c095e46738b&title=)
下面举几个反例：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726599746-77604015-4614-4fd0-83eb-0b772974663e.png#averageHue=%232b2d3b&clientId=ud6c5676d-4444-4&from=paste&id=uf2de0e9b&originHeight=746&originWidth=374&originalType=url&ratio=1&rotation=0&showTitle=false&size=57952&status=done&style=none&taskId=ucdacb0ca-4b9a-4a67-891e-d4d2bb41cec&title=)
以上三种情况都不属于尾调用。
## 尾调用的特殊性
函数调用会在内存中保留一个“调用记录”又称为“调用帧”，保存调用位置和内部变量等信息，如果在函数A的内部调用函数B，那么在A的调用帧上方，还会形成一个B的调用帧。
等到B运行结束，将结果返回到A，B的调用帧才会消失。如果函数B内部还调用函数C，那就还有一个C的调
用帧，以此类推。所有的调用帧，就形成一个“调用栈”（callstack）。
尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息
都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。（这样就不会出现栈溢出的问题）。
尾递归的本质实际上就是将方法需要的上下文通过方法的参数传递进下一次调用之中，以达到去除上层依赖。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726600132-71367fed-8fe8-4506-bfdd-49d48d4726bd.png#averageHue=%232d303f&clientId=ud6c5676d-4444-4&from=paste&id=ufe79191b&originHeight=798&originWidth=1664&originalType=url&ratio=1&rotation=0&showTitle=false&size=205043&status=done&style=none&taskId=uc4d5fc9f-5043-41c9-9a1a-9000d94e7cf&title=)
上面的例子是对阶乘的不同方式的实现，尾调的方式实现可以减少复杂度，需要注意的是尾调优化只会在严格模式中被触发。
尾调递归也是如此，需要吧函数所有的内部变量写进函数的参数里面。
尾调优化有的时候被起用，所以我们需要自己实现，尤其是尾调递归，可以考虑用循环替换递归，不让他产生调用栈，也就不存在栈溢出问题。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726600176-382205c0-6e05-4a71-9db5-f275bda075da.png#averageHue=%232b2d3a&clientId=ud6c5676d-4444-4&from=paste&id=u1e7c62af&originHeight=1254&originWidth=1232&originalType=url&ratio=1&rotation=0&showTitle=false&size=209282&status=done&style=none&taskId=u3f317779-6864-4b86-9355-feb96fb7cf1&title=)
## 尾调用的缺点
因为尾调用中抛弃了调用帧（应该是只保留一个调用帧），所以很难在调用过程中调试代码。

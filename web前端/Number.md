# 二进制和八进制表示法
通过0bxxxx表示二进制（binary）。
通过0oxxxx表示八进制（octonary）。
实例：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726421318-78319fd7-59cc-44dc-90d0-e794943aba26.png#averageHue=%23313342&clientId=u19cccf01-f3ba-4&from=paste&id=ud2f1e90e&originHeight=252&originWidth=1456&originalType=url&ratio=1&rotation=0&showTitle=false&size=50390&status=done&style=none&taskId=u309f4daa-8eb3-48a1-8b84-7872e4bbf15&title=)
# 数值分隔符
数值分隔符有几个使用注意点：

- 不能放在数值的最前面（leading）或最后面（trailing）。
- 不能两个或两个以上的分隔符连在一起。
- 小数点的前后不能有分隔符。
- 科学计数法里面，表示指数的e或E前后不能有分隔符。

实例：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726421330-8e5e5221-c5d9-4bda-8291-9cca2057e145.png#averageHue=%233b3b4e&clientId=u19cccf01-f3ba-4&from=paste&id=uff3ca775&originHeight=108&originWidth=544&originalType=url&ratio=1&rotation=0&showTitle=false&size=15635&status=done&style=none&taskId=u9eb19c4d-1dcb-45e7-b255-236964178b4&title=)
# Number.isFinite()，Number.isNaN()
它们与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，（Number.isFinite和Numer.isNaN会对类型进行判断,如果类型不对也会返回false，而同名的全局方法是一律转变成number然后判断）Number.isFinite()对于非数值一律返回false, Number.isNaN()只有对于NaN才返回true，非NaN一律返回false。
# Numer.isInteger()
判断一个数值是否是整数。也会判断数据类型是否是number，如果类型不是数值都会返回false。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726421347-724e63a1-1b05-42ca-b75a-9f5a7844453a.png#averageHue=%232b2c38&clientId=u19cccf01-f3ba-4&from=paste&id=ub2ef7513&originHeight=72&originWidth=1340&originalType=url&ratio=1&rotation=0&showTitle=false&size=22822&status=done&style=none&taskId=u74e9a8d9-d34a-4c16-a76b-6ad676125a3&title=)
这种情况浮点数和整数会认为是相等的，因为js内部中整数和浮点数采用的是同样的储存方法。
注意，由于 JavaScript 采用 IEEE 754 标准，数值存储为64位双精度格式，数值精度最多可以达到 53 个二进制位（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃，这种情况下，Number.isInteger可能会误判。
Number.isInteger(3.0000000000000002) // true
上面代码中，Number.isInteger的参数明明不是整数，但是会返回true。原因就是这个小数的精度达到了小数点后16个十进制位，转成二进制位超过了53个二进制位，导致最后的那个2被丢弃了。
类似的情况还有，如果一个数值的绝对值小于Number.MIN_VALUE（5E-324），即小于 JavaScript 能够分辨的最小值，会被自动转为 0。这时，Number.isInteger也会误判。
Number.isInteger(5E-324) // false
Number.isInteger(5E-325) // true
上面代码中，5E-325由于值太小，会被自动转为0，因此返回true。
总之，如果对数据精度的要求较高，不建议使用Number.isInteger()判断一个数值是否为整数。
# Number.EPSILON
表示一个极小的常量，他表示1与大于1的最小浮点数的差值。对于 64 位浮点数来说，大于 1 的最小浮点数
相当于二进制的1.00..001，小数点后面有连续 51 个零。这个值减去 1 之后，就等于 2 的 -52 次方。
Numer.EPSILON是JS可以表示的最小精度，如果有误差小于这个值则可以被认为没有误差。
这个常量被引入的目的是帮助我们在计算浮点数的时候设置一个误差范围：
0.1 + 0.2 === 0.3 // false;
因为：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726421414-b4dd468f-3170-4235-b1e5-286005c6bdec.png#averageHue=%23373a4c&clientId=u19cccf01-f3ba-4&from=paste&id=u5fb2b99d&originHeight=144&originWidth=1320&originalType=url&ratio=1&rotation=0&showTitle=false&size=58437&status=done&style=none&taskId=ua5d2c5c3-72ff-40a1-9ea5-2b3bc3ae937&title=)
因为0.1 + 0.2 - 0.3 的值小于Number.EPSILON，所以可以认为0.1 + 0.2 等于 0.3 他们之间的误差可以忽略。
# 安全整数和Number.isSafeInteger()
JavaScript 能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表
示这个值。
Number.isSafeInteger()方法会判断整数是否在这个区间内，判断数字大小前会先进行类型判断，如果不是数
字类型则直接返回false。
ES6 引入了Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER这两个常量，用来表示这个范围的
上下限。
# Math类的静态方法

- Math.trunc()，去除数的小数部分。返回整数部分。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649726421604-432d4b1b-b20b-4d61-81c6-9df9b68de542.png#averageHue=%236b6b6b&clientId=u19cccf01-f3ba-4&from=paste&id=u03c2808f&originHeight=884&originWidth=1168&originalType=url&ratio=1&rotation=0&showTitle=false&size=167839&status=done&style=none&taskId=u9fb909c4-a57f-42ac-8d83-e6f6159323d&title=)

- Math.sign()判断一个数是正数，负数，还是零。对于非数值，会先将他转换成数值

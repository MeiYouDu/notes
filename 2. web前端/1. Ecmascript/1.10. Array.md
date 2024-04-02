# 扩展运算符
扩展运算符（spread）是三个点（...）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序
列。
扩展运算符后面可以跟三元运算符：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649727702315-c9d1e6a9-c77f-49b8-b725-77e1b72401d5.png#averageHue=%23303140&clientId=u67c47a4c-49de-4&from=paste&id=u1a5d9049&originHeight=226&originWidth=562&originalType=url&ratio=1&rotation=0&showTitle=false&size=24567&status=done&style=none&taskId=u2856c79a-a953-41af-a1de-d2eaeed05a2&title=)
任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组。
# Array.from()
Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。
## Array.from的基本用法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649727702388-a11ff0ce-6044-4761-9209-22634be581da.png#averageHue=%232a2c39&clientId=u67c47a4c-49de-4&from=paste&id=u5af64208&originHeight=360&originWidth=302&originalType=url&ratio=1&rotation=0&showTitle=false&size=23282&status=done&style=none&taskId=u4d6fe702-7356-459d-89ea-3b7b45e4569&title=)

- ES6用Array.from的写法

Array.from方法可以把部署了iterator接口的数据结构转变成真正的数组。扩展运算符也可以把部署了iterator接口的数据结构，但是扩展运算符不能把类数组对象转变成数组，就像上面那个对象。但是Array.from可以把这个这种类数组对象转变成真正的数组，类数组对象的关键就是要有length属性。如果浏览器没有部署这个方法可以使用Array.prototype.slice方法代替。slice方法可以将类数组对象转变成真正的数组。

- ES5用Array.prototype.slice()方法也可以实现类似效果
- Array.from把字符串转变成数组

因为它能正确处理各种Unicode字符，可以避免JavaScript将大于\uFFFF的Unicode字符算作两个字符的bug，所以他可以正确的将各种字符串转变成数组。
## Array.from的参数

1. arrayLike：一个要被转化成数组的类似数组对象或者可以迭代对象。
2. mapFn: 类似map方法的用法。遍历每个元素的时候都会调用这个方法。
3. this: 可以指定调用mapFn时的this。
# Array.of()
这个方法用于将一组数值转换成数组。
Array.of可以弥补Array的不足，Array直接调用是会因为参数的个数导致行为不一致。比如：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649727702424-a5eb3776-c967-4285-a2b4-0767af991a29.png#averageHue=%23343547&clientId=u67c47a4c-49de-4&from=paste&id=u83fc9d5a&originHeight=138&originWidth=542&originalType=url&ratio=1&rotation=0&showTitle=false&size=34592&status=done&style=none&taskId=u319e15ba-21b5-40cb-84c1-934ce59e1bf&title=)
# Array.prototype.at()
在这个方法实现之前，如果javascript想访问数组的最后一个元素一般是通过arr[arr.length - 1]来访问。
为什么不通过arr[-1]来访问呢，因为js的对象可以用-1当作索引，例如通过obj[-1]来访问键名为字符串-1的值，如果通过-1来访问数组的最后一位，会导致语义冲突，所以实现了at方法。
通过[1, 2, 3, 4].at(-1)来访问这个数组的最后一位。
# 空位处理
对空位的处理
Array.from方法会把空位处理成undefined
map方法会跳过空位
find，findIndex这些方法会把空位处理成undefined
因为对待空位的规则太不统一，所以要避免空位的出现
# Array.propterty.sort()



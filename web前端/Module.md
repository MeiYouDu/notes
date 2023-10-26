# 基本语法
待补充
# `.mjs` vesurs `.js`
[v8浏览器推荐使用](https://v8.dev/features/modules#mjs)`.mjs`，它有如下优点。

- 它更清晰标识出哪些文件是module哪些文件是普通的js文件。
- 它让运行时比如`node`或者构建工具`bable`更清晰哪些文件应该解析成模块。

然而大多时候我们使用`.js`，因为很多服务器无法正确的代理`.mjs`文件，服务在代理这些文件的时候请求头的`content-type`中必须包含一个javascript的`mime-type`比如`text/javascript`。如果服务端不包含则会返回类型错误**"The server responded with a non-JavaScript MIME type"，**并且不会执行脚本。
# 在html中应用模块
直接通过`<script>`标签中增加src然后引入，`type`设置为`module`。
```html
<script type="module" src="main.js"></script>
```
当`script`标签设置`type="module"`的时候会自动开启`defer`属性。`defer`属性的[意义](#Q9y88)
# script标签
`script`标签允许我们在html中引入js脚本。默认情况下，浏览器是同步加载 JavaScript 脚本，即渲染引擎遇到`<script>`标签就会停下来，等到执行完脚本，再继续向下渲染。如果是外部脚本，还必须加入脚本下载的时间。如果脚本体积很大，下载和执行的时间就会很长，浏览器就会堵塞，会让人觉得卡死了，所以浏览器还支持异步加载，通过在`script`上设置`async`和`defer`这两个属性。
## async和defer区别概述
`defer`要等到整个页面在内存中正常渲染结束（`DOM` 结构完全生成，以及其他脚本执行完成），才会执行；`async`一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，`defer`是“渲染完再执行”，`async`是“下载完就执行”。另外，如果有多个`defer`脚本，会按照它们在页面出现的顺序加载，而多个`async`脚本是不能保证加载顺序的。
## script标签的属性
### `async`

- 对于普通的`script`标签，使用了`async`标签的脚本都会同步加载解析，并在可用的时候尽可能快的评估。
- 对于`module`，如果使用了`script`标签，那么`script`所引入的js以及它本身所依赖的其他模块会在`defer`队列中被执行，并且这个脚本会被同步加载解析，并在可以用的时候尽可能快的评估。

这个属性允许消除解析阻塞的javascript，浏览器必须在继续解析之前加载并评估。`defer`在这种情况下也有类似的效果。
### `defer`
## `script`引入javascript的过程
### 1. 静态语义：早期错误
### 2. 静态语义：是否启用严格模式
### 3. 运行时语义：评估
### 4. 脚本记录
脚本记录(script records)，它记录被评估的脚本的信息，记录的信息如下。

| Field Name | Value Type | Meaning |
| --- | --- | --- |
| [[Realm]] | a [Realm Record](https://tc39.es/ecma262/#realm-record) or undefined | The [realm](https://tc39.es/ecma262/#realm) within which this script was created. undefined if not yet assigned. |
| [[ECMAScriptCode]] | a [Parse Node](https://tc39.es/ecma262/#sec-syntactic-grammar) | The result of parsing the source text of this script using [Script](https://tc39.es/ecma262/#prod-Script)
 as the [goal symbol](https://tc39.es/ecma262/#sec-context-free-grammars) |
| [[HostDefined]] | anything (default value is empty) | Field reserved for use by [host environments](https://tc39.es/ecma262/#host-environment) that need to associate additional information with a script. |

### 5. 解析脚本
#### 注意
解析脚本和早期错误分析都优先于脚本源码的`parsescript`评估，但是，任何报错信息都要推迟到真正对源文本运行`parseScript`的时候。
### 6. 脚本评估
### 7. 全局声明实例
## 模块引入js的过程和细节
### 模块语义
来源于[ecamscript-262](https://tc39.es/ecma262/#sec-importedlocalnames)
#### 1. 静态语义：早期错误
# `module`和`script`之间的区别
# esmodule的优点
本标题下的内容都摘录自[这里](https://exploringjs.com/es6/ch_modules.html#sec_advantages-es6-modules)
## esmodule新的特征

- 更加简洁的语法
- 静态模块结构（可以帮助去除死代码，代码优化，静态分析等）
- 原生支持循环依赖（循环引用？）
## esmodule的特点

- 代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
- 模块脚本自动采用严格模式，不管有没有声明use strict。
- 模块之中，可以使用import命令加载其他模块（.js后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用export命令输出对外接口。
- 模块之中，顶层的this关键字返回undefined，而不是指向window。也就是说，在模块顶层使用this关键字，是无意义的。
- 同一个模块如果加载多次，将只执行一次。
## esmodule希望统一模块标准

- 用module替换全局对象以及`navigation`的属性。
- 用模块替换命名空间，之前的`Math`、`JSON`都是通过命名空间定义的，以后诸如此类的功能都可以通过`module`来定义。
# esmodule的行为
## import做了什么

1. 解析：读取模块的源代码并且检查语法错误。
2. 加载：递归的加载所有被导入的模块，这个步骤目前目前尚未被标准化。
3. 关联： 对于每个新加载的模块，实现了创建一个模块域并且用该模块中所有的声明填充，其中包含了其他模块的导入。如果从一个模块中导入一个属性，但是那个模块中并没有声明则会抛出语法错误。
4. 运行：实现了在每个新加载的模块的`body`中运行这些语句，同时`import`过程也完成了，所以当执行到`import`被声明的这行代码的时候不会发生任何事情....

这个系统没有说明加载的工作原理，我们可以通过查看源码的`import`声明语句从而提前知道每个模块的依赖	项，ES6的实现可以在编译时自由的完成所有工作并且打包我们的全部模块成一个文件然后再网络中发送他们。
webpack打包的作用是减少了很多网络请求，而且有可能重复请求多次，webpack打包的话不仅可以让我们使。用`es6`的module，还可以拥有工程的好处——没有运行时的性能损耗。
## 静态和动态
javascript的是一门动态语言，但是他的模块系统是静态的。

- `import`和`export`都只在module的顶层使用，不能再条件中使用也不能在函数作用域中使用。
- 所有的导出标识符都必须在源码中按名称现式的导出，不能通过编程的方式遍历数组并数据驱动的方式导出。
- 模块对象被冻结时就不允许在对象中插入新的属性。
- 所有模块的依赖都必须在模块代码被运行之前被加载、解析、关联，`import`不支持任何懒加载语法。
- `import`不会报错，因为我们无法在`try...catch`中捕获运行`import`，即使`import`报错也变得没有任何意义。另外，因为模块系统是静态的，打包工具可以在编译时抛出这些错误。
- 模块不能在他加载依赖项之前运行一些代码。模块也不能控制他的依赖项加载的方式。
# es6模块与commomjs模块的差异

- commonjs模块是对值的拷贝，esmodule是对值的引用。
- commonjs模块在运行时加载，esmodule在静态语法分析阶段就已经创建了引用。commonjs模块是加载`module.exports`对象，这个 对象只有在运行时才会生成。esmodule会在加载时就会根据`import`加载被导入的模块，并根据依赖关系建立引用，当模块被使用的时候再根据引用去模块中取值。
- commonjs模块是运行时同步加载，esmodule是异步加载，有独立的模块[解析阶段](#QsGTk)
# es6模块的循环加载
ES6处理“循环加载”与CommonJS有本质的不同。ES6模块是动态引用，如果使用import从一个模块中加载变量（即import foo from ′foo′），那么，变量不会被缓存，而是成为一个指向被加载模块的引用，需要开发者保证在真正取值的时候能够取到值。
```javascript
// a.mjs
import {bar} from './b';
console.log('a.mjs');
console.log(bar);
export let foo = 'foo';
```
```javascript
// b.mjs
import {foo} from './a';
console.log('b.mjs');
console.log(foo);
export let bar = 'bar';
```
```shell
$ node --experimental-modules a.mjs
b.mjs
ReferenceError: foo is not defined
```

1. 上面的代码执行的时候先执行a.mjs，引擎第一步做的是解析如果有语法问题则直接抛出错误，
2. 递归的加载子模块（也就是加载了b.mjs）。
3. 关联依赖关系，根据export以及import创建索引。
4. 执行b.mjs，执行到第四行的时候打印`foo`，因为a.mjs还没有执行完，所以根据索引`foo`从a.mjs模块中查询不到`foo`，就会报错，`foo is not defined`。

如果想让上面的代码顺利执行。
可以使用下面这种方法。
```javascript
import {bar} from './b.mjs';
console.log('a.mjs');
console.log(bar());
// 函数声明提升，所以在b.mjs中可以顺利调用。
function foo() { return 'foo' }
// 下面通过字面赋值的方式声明函数，因为声明和赋值不是同时进行的，所以b.mjs在执行中找到的foo是undefined。
// 也得益于var的变量提升，如果是let或者const声明的，b.mjs中会直接报错
// var foo = function () { return 'foo' }
export {foo};
```
```javascript
import {foo} from './a.mjs';
console.log('b.mjs');
console.log(foo());
function bar() { return 'bar' }
export {bar};
```
```shell
node src/module/a.mjs                                                                                                  1 х │ 11:41:12 下午 
b.mjs
foo
a.mjs
bar
```
# 动态模块加载


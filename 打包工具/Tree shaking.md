表示消除死代码（dead_code）。
这个想法最早来源于lisp，用于消除未调用的代码。（纯函数无副作用）。
JavaScript的tree shaking是源于rollup。
Tree shaking依赖于esmodule的静态语法分析（不执行任何代码，就可以明确知道模块之间的依赖关系）。
# webpack实现tree shaking的两种方法。

1. userdExports

这个选项可以标记那些函数时多余的，然后terser可以解析这个标记然后完成treeshaking。

1. sideEffects

判断一个文件是否有副作用，如果有副作用则不会被tree shaking。如果是一个纯粹的模块，且引入之后并没有使用，则会被tree shaking。
# Tree shaking的依赖约束

- 使用 ES2015 模块语法（即 import 和 export）。
- 确保没有编译器将您的 ES2015 模块语法转换为 CommonJS 的（顺带一提，这是现在常用的 @babel/preset-env 的默认行为，详细信息请参阅[文档](https://babeljs.io/docs/en/babel-preset-env#modules)）。
- 在项目的 package.json 文件中，添加 "sideEffects" 属性。
- 使用 mode 为 "production" 的配置项以启用[更多优化项](https://webpack.docschina.org/concepts/mode/#usage)，包括压缩代码与 tree shaking。
# css的tree shaking
可以利用PurgeCssPlugin

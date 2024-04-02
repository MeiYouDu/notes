# Compiler（执行器）
compiler 是 webpack 的核心执行器，它负责管理整个编译过程，包括初始化参数、创建 compilation 对象、加载插件、执行 run 方法等。compiler 对象只会在 webpack 启动时创建一次，并且在整个编译过程中保持不变。compiler 对象提供了很多钩子（hook）来让插件注册回调函数，在不同的阶段执行不同的任务。
# Compilation（编译器）
compilation 是 webpack 的核心编译器，它负责具体的编译工作，包括创建模块、分析依赖、转换内容、生成资源等。compilation 对象会在每次编译时创建一个新的实例，并且在每次编译过程中保持不变。compilation 对象也提供了很多钩子（hook）来让插件注册回调函数，在不同的阶段执行不同的任务
# 总结
总的来说，compiler 负责整个 Webpack 构建过程的控制，而 compilation 则代表了单次构建过程的具体内容和状态。你可以通过监听 compiler 的事件和钩子来了解整个构建过程，也可以在插件中操作 compilation 对象来对具体的编译过程进行干预和定制。

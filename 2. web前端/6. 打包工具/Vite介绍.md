# vite主要由两部分组成
1. 一个开发服务器，基于原生esmodule提供了丰富的内建功能，HMR的速度非常快。
2. 通过rollup打包打开代码，并且不需要任何配置，因为vite已经基于rollup进行了很多预配置。
# ESBuild构建工具
esbuild使用go语言编写，可以直接转变成机器码，而无需经过字节码。
esbuild会启动一个进程，然后一个进程中会启动多个线程，尽可能的充分利用cpu的多核心，且尽可能饱和运行。
esbuild从零开始编写，而不是使用第三方的库，从一开始就考虑很多性能问题。

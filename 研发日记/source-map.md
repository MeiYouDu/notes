# 问题描述
当在以下环境时，vue的SFC文件通过`source-map`不能正常还原源码，也不能正常断点调试。
## 环境
webpack5，
vue3.3.x或者vue3.2.x
esbuild-loader
# 解决方案
使用babel-loader替换esbuild-loader。也就是用babel作为代码编辑器，从而替换esbuild。
esbuild本身与webpack生态的兼容性不够好。


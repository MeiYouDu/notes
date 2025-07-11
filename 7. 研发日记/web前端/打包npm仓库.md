# 2022-04-27
目前打包的是一个typescript开发的项目，目的是将本项目打包生成通用的js文件和类型声明文件，然后发不到npm代码仓库中。
## 首先解决的是编译的问题。
### a 通过tsc编译
直接编译出js文件以及类型声明文件。
### b 通过webpack编译
webpack编译出js文件，tsc只输出类型声明文件。
### c rollup编译
rollup编译出文件，并且利用types.config.json的配置生成声明文件。

a 方案配置最简单，但是生成的js文件没有经过压缩和tree-shaking，保留原本的目录结构，理论上打包出来的代码体积最大。

b 方案配置最复杂，需要引入很多plugin，可以生成支持不同运行环境的js代码（有待测试），打包出来的是经过丑化的代码，通过一些配置应该可以打包出可读性更好的代码。

c 方案配置相对简单，可以不同运行环境下的js代码，支持commonjs，esmodule，以及直接通过script标签引入。代码体积小的同时保留的打包之后代码的可读性，同时打包时可以利用ts的配置文件，整合代码能力更好。
总结：目前c方案是编译ts项目的最佳实践。

## 上传npm仓库
上传npm仓库前还需要做一些准备工作。
### 配置package.json文件，配置效果如下。
```json
{
  "name": "arbor-cesium-drawer", // 库的名称
  "version": "0.0.8", // 库的版本
  "description": "cesium地图作业库",
  "private": false, // 如果npm账户没有创建私有仓库权限，就不能是true
  "main": "dist/package/index.cjc.js", // 库commonjs入口
  "module": "dist/package/index.es.js", // 库esmodule入口
  "global": "dist/package/index.global.js",
  "types": "dist/types/index.d.ts", // 类型文件路径，也可以是typings，效果一样。
}
```
### 管理依赖
管理依赖，假如项目中依赖了别的库，我们并不希望把别的库打包到我们的库中，这时我们需要把依赖的库设为“devDependencies”，这样在打包时就不会包含进去，而使用本仓库的开发者会在项目中自动安装依赖。（这样还可以避免出现一些错误，比如：如果开发者本身就安装我们依赖的一个仓库，而且我们发布的仓库中也包含这个依赖，就会造成不同源的问题，例如创建对象实例，对象实例的构造函数有可能不同源，导致代码中的某些逻辑失效；换个思路去想这件事，本身cesium对于我们开发这个库就是只在dev时才会用到，我们打包编译并不需要用到cesium，所以cesium理应归于devDependencies）。
### 配置需要上传的文件
配置需要上传的文件，有两种方式

- 配置package.json中的files属性，配置哪些文件要上传。
- 配置.npmignore文件，配置哪些文件不上传。
### npm账户
注册npm账户并登陆，注册npm账户就不说了，登陆npm账户需要通过命令行如下。
```shell
# log in, linking the scope to the custom registry
npm login --scope=@mycorp --registry=https://registry.mycorp.com

# log out, removing the link and the auth token
npm logout --scope=@mycorp
```
### 发布
发布npm包如下，发布和登陆都需要注意registry源，因为我们有可能设置了不同的镜像源。
```shell
npm publish
```
 


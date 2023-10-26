# 模块解析
resolver 帮助 webpack 从每个 require/import 语句中，找到需要引入到 bundle 中的模块代码。 当打包模块时，webpack 使用 [enhanced-resolve](https://github.com/webpack/enhanced-resolve) 来解析文件路径。
# webpack中的解析规则
使用enhanced-resolve可以解析三种文件路径

- 绝对路径
- 相对路径
- 模块路径

在 [resolve.modules](https://webpack.docschina.org/configuration/resolve/#resolvemodules) 中指定的所有目录检索模块。 你可以通过配置别名的方式来替换初始模块路径，具体请参照 [resolve.alias](https://webpack.docschina.org/configuration/resolve/#resolvealias) 配置选项。

- 如果 package 中包含 package.json 文件，那么在 [resolve.exportsFields](https://webpack.docschina.org/configuration/resolve/#resolveexportsfields) 配置选项中指定的字段会被依次查找，package.json 中的第一个字段会根据 [package导出指南](https://webpack.docschina.org/guides/package-exports/)确定 package 中可用的 export。

一旦根据上述规则解析路径后，resolver 将会检查路径是指向文件还是文件夹。如果路径指向文件：

- 如果文件具有扩展名，则直接将文件打包。
- 否则，将使用 [resolve.extensions](https://webpack.docschina.org/configuration/resolve/#resolveextensions) 选项作为文件扩展名来解析，此选项会告诉解析器在解析中能够接受那些扩展名（例如 .js，.jsx）。

如果路径指向一个文件夹，则进行如下步骤寻找具有正确扩展名的文件：

- 如果文件夹中包含 package.json 文件，则会根据 [resolve.mainFields](https://webpack.docschina.org/configuration/resolve/#resolve-mainfields) 配置中的字段顺序查找，并根据 package.json 中的符合配置要求的第一个字段来确定文件路径。
- 如果不存在 package.json 文件或 [resolve.mainFields](https://webpack.docschina.org/configuration/resolve/#resolvemainfields) 没有返回有效路径，则会根据 [resolve.mainFiles](https://webpack.docschina.org/configuration/resolve/#resolvemainfiles) 配置选项中指定的文件名顺序查找，看是否能在 import/require 的目录下匹配到一个存在的文件名。
- 然后使用 [resolve.extensions](https://webpack.docschina.org/configuration/resolve/#resolveextensions) 选项，以类似的方式解析文件扩展名。

webpack 会根据构建目标，为这些选项提供合理的[默认](https://webpack.docschina.org/configuration/resolve)配置。

1. commonjs模块化实现原理

commonjs模块被webpack打包之后的转化
```javascript
(() => {
var __webpack_modules__ = ({
"./src/js/common-js-child.js": ((module) => {
function print1() {
console.log('print1')
}
function print2() {
console.log('print2')
}
//5. 给module.exports赋值
module.exports = {
print1,
print2
}
})
});
var __webpack_module_cache__ = {}; // 缓存
function __webpack_require__(moduleId) {
//2. 检查缓存中是否存在如果存在则直接从缓存中取值
var cachedModule = __webpack_module_cache__[moduleId];
if (cachedModule !== undefined) {
return cachedModule.exports;
}
//3. 连续赋值，指向同一内存地址。
var module = __webpack_module_cache__[moduleId] = {
exports: {}
};
//4. 找到对应的函数传入三个参数并执行。
__webpack_modules__[moduleId](module, module.exports, __webpack_require__);
//6. 返回module.exports
return module.exports;
}
var __webpack_exports__ = {};
(() => {
//1. 执行这个函数。将路径传入这个函数后得到返回值并结构
const {
print1,
print2
} = __webpack_require__("./src/js/common-js-child.js")
//7. 结构之后执行
print2()
print1()
})();
})();
```

1. Es module模块化实现原理
```javascript
(() => { // webpackBootstrap

"use strict";
// 声明被引入的模块内容合集
var __webpack_modules__ = ({
"./src/js/es_math.js":
((__unused_webpack_module, __webpack_exports__, __webpack_require__) => {

__webpack_require__.r(__webpack_exports__);
// 给exports增加属性，分别指向下面生命的print1和print2
__webpack_require__.d(__webpack_exports__, {
/* harmony export */
"print1": () => ( /* binding */ print1),
/* harmony export */
"print2": () => ( /* binding */ print2)
/* harmony export */
});
function print1() {
return 'print1'
}
function print2() {
return 'print2'
}
})

});
// 缓存
var __webpack_module_cache__ = {};
// 引入模块的函数
function __webpack_require__(moduleId) {
var cachedModule = __webpack_module_cache__[moduleId];
// 检查缓存
if (cachedModule !== undefined) {

return cachedModule.exports;

}
var module = __webpack_module_cache__[moduleId] = {
exports: {}

};
//4. 找到对应的方法并调用
__webpack_modules__[moduleId](module, module.exports, __webpack_require__);
return module.exports;

}
(() => {
__webpack_require__.d = (exports, definition) => {

for (var key in definition) {

if (__webpack_require__.o(definition, key) && !__webpack_require__.o(exports, key)) {

Object.defineProperty(exports, key, {
enumerable: true,
get: definition[key]
});

}

}

};

})();
(() => {

__webpack_require__.o = (obj, prop) => (Object.prototype.hasOwnProperty.call(obj, prop))

})();
(() => {
__webpack_require__.r = (exports) => {

if (typeof Symbol !== 'undefined' && Symbol.toStringTag) {

Object.defineProperty(exports, Symbol.toStringTag, {
value: 'Module'
});

}

Object.defineProperty(exports, '__esModule', {
value: true
});

};

})();
//1. 创建一个module
var __webpack_exports__ = {};
(() => {
//2. 调用r方法，给这个module一个标记表示他是esModule
__webpack_require__.r(__webpack_exports__);
/* harmony import */
//3. 调用方法并且接受返回值
var _js_es_math__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__( /*! ./js/es_math */ "./src/js/es_math.js");

console.log((0, _js_es_math__WEBPACK_IMPORTED_MODULE_0__.print1)())
console.log((0, _js_es_math__WEBPACK_IMPORTED_MODULE_0__.print2)())
})();

})();
//# sourceMappingURL=main.c5ef5bc6.js.map
```

1. commonjs加载esmodule的原理

类似1、2所以不再展示代码了

1. esmodule加载commonjs的原理

类似1、2所以不再展示代码了

一个简单的source-map源码如下
```json
{
  "version": 3,
  "sources": [
    "webpack://04plugin/./src/js/es_math.js",
    "webpack://04plugin/webpack/bootstrap",
    "webpack://04plugin/webpack/runtime/define property getters",
    "webpack://04plugin/webpack/runtime/hasOwnProperty shorthand",
    "webpack://04plugin/webpack/runtime/make namespace object",
    "webpack://04plugin/./src/es_index.js"
  ],
  "names": [ui],
  "mappings": ";;;;;;;;;;;;;;;AAAO;AACP;AACA;AACO;AACP;AACA,C;;;;;;UCLA;UACA;;UAEA;UACA;UACA;UACA;UACA;UACA;UACA;UACA;UACA;UACA;UACA;UACA;UACA;;UAEA;UACA;;UAEA;UACA;UACA;;;;;WCtBA;WACA;WACA;WACA;WACA,wCAAwC,yCAAyC;WACjF;WACA;WACA,E;;;;;WCPA,wF;;;;;WCAA;WACA;WACA;WACA,sDAAsD,kBAAkB;WACxE;WACA,+CAA+C,cAAc;WAC7D,E;;;;;;;;;;;;ACHqB;;AAErB,YAAY,mDAAM;AAClB,YAAY,mDAAM,G",
  "file": "js/main.c5ef5bc6.js",
  "sourcesContent": [
    "export function print1() {\r\n  return 'print1'\r\n}\r\nexport function print2() {\r\n  return 'print2'\r\n}",
    "// The module cache\nvar __webpack_module_cache__ = {};\n\n// The require function\nfunction __webpack_require__(moduleId) {\n\t// Check if module is in cache\n\tvar cachedModule = __webpack_module_cache__[moduleId];\n\tif (cachedModule !== undefined) {\n\t\treturn cachedModule.exports;\n\t}\n\t// Create a new module (and put it into the cache)\n\tvar module = __webpack_module_cache__[moduleId] = {\n\t\t// no module.id needed\n\t\t// no module.loaded needed\n\t\texports: {}\n\t};\n\n\t// Execute the module function\n\t__webpack_modules__[moduleId](module, module.exports, __webpack_require__);\n\n\t// Return the exports of the module\n\treturn module.exports;\n}\n\n",
    "// define getter functions for harmony exports\n__webpack_require__.d = (exports, definition) => {\n\tfor(var key in definition) {\n\t\tif(__webpack_require__.o(definition, key) && !__webpack_require__.o(exports, key)) {\n\t\t\tObject.defineProperty(exports, key, { enumerable: true, get: definition[key] });\n\t\t}\n\t}\n};",
    "__webpack_require__.o = (obj, prop) => (Object.prototype.hasOwnProperty.call(obj, prop))",
    "// define __esModule on exports\n__webpack_require__.r = (exports) => {\n\tif(typeof Symbol !== 'undefined' && Symbol.toStringTag) {\n\t\tObject.defineProperty(exports, Symbol.toStringTag, { value: 'Module' });\n\t}\n\tObject.defineProperty(exports, '__esModule', { value: true });\n};",
    "import {\r\n  print1,\r\n  print2\r\n} from './js/es_math'\r\n\r\nconsole.log(print1())\r\nconsole.log(print2())"
  ],
  "sourceRoot": ""
}
```

- Version

表示当前的source-map的版本，目前是第三个版本，第一个版本的source-map是源码的10倍，现在的差不多是两倍多。

- Sources

记录开发人员写的源代码以及webpack的代码

- Names

记录用过的变量对象函数等等的名字。

- Mappings

是一个base64 VLQ的信息，通过这个信息以及sources可以找到原本的代码

- File

表示打包之后的文件

- sourcesContent

记录了源码

- sourceRoot

sources的根目录

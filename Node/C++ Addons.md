# 简介
扩展（addons）是一个C++编写的动态链接库，`require()`方法可以像普通node module一样加载这个扩展，扩展在Javascripts和C/C++之间提供了一个接口。
这里有三种方式可以实现扩展（addons）：

- Node-API（应用二进制接口 application binary interface）
- nan（一种Node.js原生抽象，native abstractions for Node.js）
- 直接使用内部的V8，libuv，以及其他的Node.js库，除非需要直接访问Node-API中没有暴露的功能，都推荐使用Node-API，

当不使用Node-API的时候，实现扩展非常复杂，涉及以下几个组件的知识。

- V8：Node.js中使用这个C++库来提供Javascript实现，V8提供了创建对象、调用函数的工具。
- libuv：这个C的库实现了Node.js的事件循环，他是实现Node.js中的worker thread和所有的异步行为的平台。它同时是一个跨平台的抽象库，他可以像`POSIX`一样轻松的在很多主要的操作系统中访问很多通用系统任务，比如与文件、sockets、timers、和系统事件交互。libuv还提供了类似`POSIX`线程的线程抽象用来实现超越标准事件循环的更复杂的异步事件。扩展的开发者在进行`I/O`或者耗时的操作时应该避免阻塞事件循环，应该通过libuv的非阻塞系统操作将工作卸载到worker线程或者自定义的libuv线程。
- 内部的Node.js库，Node.js自身暴露了一些扩展插件可以使用的C++ api，其中最重要的就是`node::ObjectWrap`class。
- Node.js还包含其他的静态链接库，包括`openssl`。其他的库都包含在Node.js源码树中的`deps`目录下，只有`libuv, OpenSSL, V8, zlib`这些被Node.js重新导出并且可以被扩展使用。
# 链接到Node.js包含的库
Node.js使用很多静态链接库比如：V8，libuv，以及openss，所有的插件都需要链接到V8或者需要链接到其他的依赖项。比如，一个简单的`#include <...>`语句比如（`#include <v8.h>`），`node-gyp`会自动找到合适的头文件。下面是一些注意事项：

- 当`node-gyp`运行的时候会检测出node的版本并且下载全部的源tarball或者只是头文件。如果完整的源已经下载了，扩展将会完整的访问全部的Node.js依赖。然而，如果仅有Node.js头文件被下载，那么只有被Node.js暴露的符号才可用。
- `node-gyp`可以在运行时使用`--nodedir`来指定本地的node源，使用这个配置帮助扩展插件访问完整的依赖。
# 使用`require`加载扩展插件
扩展插件的扩展名是`.node`，`require()`函数会去查找具有`.node`扩展名的文件并且作为动态链接库初始化他们。
`.node`扩展名可以省略，Node.js可以正常初始化它们，但是要注意的是Node.js自动补充扩展名的机制是有优先级的，比如`.js`文件也可以省略扩展名，而且优先级比`.node`高。
# nodejs的原生抽象
每个Node.js版本更新以及内部的V8更新的时候都会有一些小的更新。每次更新的时候为了继续工作可能需要更新并从新编译扩展插件。Node.js可以尽量保证自身的版本稳定性，保证更新功能只影响自身，但是Node.js不能确保V8也如此。
推荐使用**node原生抽象**工具比如（[nan](https://github.com/nodejs/nan)），提供了一系列的工具，保证过去的V8或者Node.js版本的兼容性。
# Node-API
Node-API是构建原生扩展的API。它不依赖于javascript的运行时（比如V8）并且由Node.js团队自身维护。这个API是跨Node.js稳定版本的应用二进制接口。他致力于隔离扩展与javascript引擎的变化，并且允许模块编译之后运行在低版本的Node.js中且不用重新编译。通过这种方式编译打包成`.node`文件也用到了上文提到的打包工具`node-gyp`。
```cpp
// hello.cc using Node-API
#include <node_api.h>

namespace demo {

napi_value Method(napi_env env, napi_callback_info args) {
  napi_value greeting;
  napi_status status;

  status = napi_create_string_utf8(env, "world", NAPI_AUTO_LENGTH, &greeting);
  if (status != napi_ok) return nullptr;
  return greeting;
}

napi_value init(napi_env env, napi_value exports) {
  napi_status status;
  napi_value fn;

  status = napi_create_function(env, nullptr, 0, Method, nullptr, &fn);
  if (status != napi_ok) return nullptr;

  status = napi_set_named_property(env, exports, "hello", fn);
  if (status != napi_ok) return nullptr;
  return exports;
}

NAPI_MODULE(NODE_GYP_MODULE_NAME, init)

}  // namespace demo
```
## 关于node-api实现跨node版本稳定性的原理
Node-API是一种用于构建原生插件的API，它与底层的JavaScript运行时（例如V8）无关，而是作为Node.js本身的一部分维护。这个API将在Node.js的不同版本之间保持应用程序二进制接口（ABI）的稳定性。这意味着使用Node-API编译的插件可以在不重新编译的情况下，在Node.js的不同主版本之间运行。
Node-API实现跨Node.js版本稳定性的原理是，它提供了一套与JavaScript语言规范（ECMA-262）中定义的概念和操作相对应的API，用于创建和操作JavaScript值。这些API都是C语言编写的，并且不依赖于任何特定的JavaScript引擎。这样，即使Node.js更换或升级了底层的JavaScript引擎，Node-API也不会受到影响，只要它遵循了JavaScript语言规范。
:::danger
也就是只要v8引擎的语言规范和node-api的语言规范一致，就可以保证兼容性。
:::
# 总结
为了V8引擎和各个node版本的兼容性，推荐使用Node-API或者`native abstractions for Node.js`来构建扩展插件，当然如果这些APIs不支持的时候则需要使用C++以及`v8.h`来实现。

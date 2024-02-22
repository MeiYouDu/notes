# 描述

当使用 pnpm 安装 electron 后，执行

```shell
npx electron
```

报错：Library not loaded: @rpath/Electron Framework.framework/Electron Framework

# 原因

pnpm使用了硬链接的方式来管理文件。

electron 会在安装完脚本后再执行一个安装脚本自行缓存二进制预编译文件。

二者缓存策略不一样导致了这个问题

# 解决方案

参考 pnpm 的 issue（[原地址](https://github.com/pnpm/pnpm/issues/7253) ）

```json
"scripts": {
  "prepare": "rm -rf node_modules/electron/dist && node node_modules/electron/install.js",
  ...
}
```

上面是通过在 scripts 里面增加的，可以编写自己的 node 脚本挂载在 postInstall里面执行

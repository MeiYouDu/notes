# 描述

vue3 中使用 tsx，遇到原生 dom 报错报错信息如下

ts 报错编号 ts7026 。

# 原因

ts 配置问题，没有设置的情况下默认会找React.Fragment，因为是基于 vue 使用 tsx 所以肯定找不到react。

# 解决方案

在 tsconfig 里面增加如下配置项

```json
{
  "jsxImportSource": "vue",
}
```


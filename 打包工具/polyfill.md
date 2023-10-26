可以理解为一个补丁。
因为有的时候会使用一些特别的语法和特性，比如promise，generator，symbol等。
很多浏览器并不支持这些属性。
```javascript
module.exports = {
  presets: [
    ["@babel/preset-env", {
      // false 不适用polyfill
      // usage 代码中需要那些polyfill就引入那些
      // entry 手动在入口引入corejs和regenerator-runtime，然后根据目标浏览器引入所有的
      useBuiltIns: "usage",
      corejs: 3
    }]
  ]
}
```


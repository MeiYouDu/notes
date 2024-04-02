cdn优化网络资源请求。提高用户使用体验。
一般情况只需要在生产环境中使用。
在生产环境中配置external。如下
```html
<script
  src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous"
></script>
```
```javascript
module.exports = {
  //...
  externals: {
    jquery: 'jQuery',
  },
};
```

如果要在html中区分生产环境和开发环境
可以写成：
```html
<!-- ejs语法 -->
<% if(process.env.NODE_ENV === "production") { %>
  <script
  src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous"
  ></script>
<% } %>
```
类似这种形式，增加条件判断

# 遇到的问题汇总
## 锚点定位和路由冲突
`href`锚点定位会导致页面路由变化，之后刷新就会导致页面定位失败，使用以下方式解决。
```javascript
// 通过id获取dom
const doc = document.getElementById(this.sideNavConfig[0].text);
// 通过scrollIntoView方法跳转到文档所在位置
doc &&
  doc.scrollIntoView({
    behavior: "smooth",
  });
```
## 刷新时重定位index.html以及虚拟路由重定向

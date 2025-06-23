# qiankun实现微前端的原理

**动态加载资源** + **沙箱隔离** + **统一生命周期管理**

## 动态资源

url匹配调用子应用生命周期，并激活子应用，加载子应用资源

## 沙箱隔离

### js

proxy window

### css

shadow dom

动态样式表，动态创建和移除 style sheet


# 问题描述
当时用SFC的时候，组件之间存在父子关系，且父组件使用了css scoped来防止样式污染，子组件的根结点样式被污染了。
# 问题分析

- 只有子组件的根元素会受到父组件的样式污染（当根节点上使用了与父组件同名的选择器名称时）
- 子组件的其他元素不受影响。
# 解决方案
首先这个并不是一个问题，SFC的css scoped本身就设计如此。
> ### Child Component Root Elements
> With scoped, the parent component's styles will not leak into child components. However, a child component's root node will be affected by both the parent's scoped CSS and the child's scoped CSS. This is by design so that the parent can style the child root element for layout purposes.

上面引用vue官网的一句话，当使用scoped的时候，父组件的样式将不会泄漏到子组件。然而，子组件的根节点会受到父组件的css和他自身的css两者的影响。设计就是这样的，是为了父组件可以改变子组件根节点的布局。

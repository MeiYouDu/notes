# 问题描述
vue3版本：3.2.x
当想针对`ElDialog`二次封装时遇到了一个问题，代码如下
```javascript
import { defineComponent } from "vue";
import { ElDialog } from "element-plus";
/**
 * 子组件
 */

/**
 * 打开窗口
 * @param data
 */
function open(data?: number) {
	console.log(data);
	return data;
}
export default defineComponent({
	name: "TestDialog",
	components: {
		ElDialog,
	},
	setup() {
		return () => {
			return (
				<ElDialog>
					<div>1213</div>
				</ElDialog>
			);
		};
	},
});

```
使用的方式如下：
```javascript
<TestDialog
  ref={TestDialogInstance}
  v-model={showDialog.value}
  title={"test"}
  onOpened={() => {}}>
</TestDialog>
```
但是报错了，ts类型检测认为组件`TestDialog`上不应该存在`title`属性。报错如下图所示![image.png](https://cdn.nlark.com/yuque/0/2023/png/12763837/1683814012172-55e42559-77b2-44cc-9d0e-693c1ea03c9e.png#averageHue=%233a3b3d&clientId=ucb4bdc5b-410c-4&from=paste&height=199&id=uf7e74f08&originHeight=398&originWidth=2008&originalType=binary&ratio=2&rotation=0&showTitle=false&size=161144&status=done&style=none&taskId=u8498c0cc-2b45-474a-b669-8e9902af3b9&title=&width=1004)
# 问题分析

1. 这个问题其实是因为`TestDialog`组件内部没有在`props`中定义`title`属性。类似的报错也同理。
2. 在vue3中的爷孙组件之间，爷组件中在使用父组件的时候，可以在父组件上绑定属性，如果父组件中没有定义对应的`props`，则这些属性会被自动的绑定给父组件的根元素，以此类推。事件监听也遵循这个规则。
3. 如果在`jsx`或者`vue`文件中，可以直接利用这个规则。但是在`tsx`中会进行类型检测，这个时候就会报上面这个错误。
4. 如果我们在父组件的props中显式的定义这些属性（比如引入element-plus定义好的组件props和emits并注册），则可以消除报错，但是会导致这些属性被拦截在父组件这一层，不会主动的向孙组件传递，这与我们的目的不一致。
5. 关于4，我们可以通过`setup`函数获取到`attrs`，并且显式绑定在孙组件上，这样可以解决属性传递的问题，但是会遇到新的问题：
   1. 无法获取到监听事件，也就无法显式的绑定给孙组件。
   2. 因为TSX编译器会解析事件名称，比如v-model会解析成`model或者modelView`。所以会导致传给孙组件的属性有问题，导致属性失效。
# 解决方案
## 通过组件的`-`写法绕开类型检测
比如在使用父组件的时候通过`-`语法来使用。如下
```tsx
<test-dialog
  ref={TestDialogInstance}
  v-model={showDialog.value}
  title={"test"}
  onOpened={() => {}}>
</test-dialog>
```
这种写法可以绕开类型检测，也就不能享受ts带来的类型提示
## ~~通过defineComponent传入类型（推荐）~~
```tsx
export default defineComponent<
  unknown,
  // 如果要定义组件对外暴露的属性，就通过这种方式，同时第四个参数也需要传入
  InstanceType<typeof ElDialog> & { open: typeof open },
  unknown,
  { open: typeof open } // 定义对外暴露的api
  >({
  name: "TestDialog",
  components: {
    ElDialog,
  },
  setup(props, { expose }) {
    expose({
      open,
    });
    return () => {
      return (
        <ElDialog>
          <div>1213</div>
        </ElDialog>
      );
    };
  },
});
```
通过defineComponent传入要继承的类型，第二个参数是要继承的组件类型。
这种写法可以享受ts类型检测，和类型提示。
可能是范型内部传入的类型有问题，可以继承子组件，但是当想自定义属性时，props属性会报错
## 通过更新到vue3.3.x
vue3.3.x使用`defineComponent`api时支持函数签名，这种方式可以更好的支持`TSX`。
```tsx
import type { DialogEmits, DialogProps } from "element-plus";
import { defineComponent, computed } from "vue";

import { ElButton, ElDialog } from "element-plus";

/**
 * testDialog的props
 */
type TestDialogProps = {
  /**
	 * 测试名称
	 */
  testName?: string;
};

/**
 * testDialog的emits
 */
type TestDialogEmits = {
  /**
	 * 测试点击事件
	 * @param data 参数
	 */
  testClick?: (data?: number) => boolean;
};

/**
 * 改组件实例对外暴露的接口
 */
type TestDialogExposes = {
  open: typeof open;
};

/**
 * 打开窗口
 * @param data
 */
function open(data?: number) {
  return data;
}

// 安全tree-shaking
export default /*#__PURE__*/ defineComponent<
  TestDialogProps & Partial<DialogProps>,
  TestDialogEmits & Partial<DialogEmits>
  >(
  (props, { expose, emit }) => {
    const testCompute = computed<string | undefined>(() => props.testName);
    expose({
      open,
    });
    return () => {
      return (
        <ElDialog>
          <div>弹窗内容</div>
          <div>{props.testName}</div>
          <div>{testCompute.value + "+计算属性"}</div>
          <ElButton onClick={() => emit("testClick", 111)}>测试事件</ElButton>
        </ElDialog>
      );
    };
  },
  {
    name: "TestDialog",
    props: {
      testName: {
        type: String,
        default: "",
      },
    },
    emits: {
      testClick(data?: number) {
        return typeof data === "number";
      },
    },
  },
);
export { TestDialogProps, TestDialogEmits, TestDialogExposes };

```
:::danger
注意：因为`defineComponent()`在这里作为一个函数被调用，一些构建工具可能会被认为他会产生副作用，比如：webpack。尽管这个组件从来没有被使用过，但这可能会阻止这个组件被tree-shaking掉。
可以通过添加`/*#__PURE__*/`来告诉webpack这个组件可以安全的tree-shaking。
:::


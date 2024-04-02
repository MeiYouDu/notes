# tapable的hook
- SyncHook,
- SyncBailHook,
- SyncWaterfallHook,
- SyncLoopHook,
- AsyncParallelHook,
- AsyncParallelBailHook,
- AsyncSeriesHook,
- AsyncSeriesBailHook,
- AsyncSeriesWaterfallHook

大致的分类：
## 同步和异步
sync为同步钩子，async为异步
## Bail
当hook的一个监听事件中有返回值时，后续的事件就不再执行了。
## Loop
当hook上注册的消费事件返回值为true，会循环执行当前事件直到该事件返回值为false，然后执行下一个注册的消费事件。
## Waterfall
当hook上注册的消费事件有返回值时，会将返回值传递给下一个注册的消费事件。
## Async
异步的hook分为并行和串行

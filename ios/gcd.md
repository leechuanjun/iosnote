# GCD

###GCD:全称是Grand Central Dispatch，可译为“伟大的中枢调度器”

- GCD中有2个核心概念
 - 任务：执行什么操作
 - 队列：用来存放任务

- 将任务添加到队列中
- GCD会自动将队列中的任务取出，放到对应的线程中执行
- 任务的取出遵循队列的FIFO原则：先进先出，后进后出

```objc
//GCD中有2个用来执行任务的函数
//用同步的方式执行任务
dispatch_sync(dispatch_queue_t queue, dispatch_block_t block);
queue：队列
block：任务

//用异步的方式执行任务
dispatch_async(dispatch_queue_t queue, dispatch_block_t block);
```

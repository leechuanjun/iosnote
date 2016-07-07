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

##GCD的队列可以分为2大类型
- ###并发队列（Concurrent Dispatch Queue）
- 可以让多个任务并发（同时）执行（自动开启多个线程同时执行任务）
- 并发功能只有在异步（dispatch_async）函数下才有效

- ###串行队列（Serial Dispatch Queue）
- 让任务一个接着一个地执行（一个任务执行完毕后，再执行下一个任务）

##同步、异步、并发、串行
 * 同步和异步主要影响：能不能开启新的线程
   * 同步：在当前线程中执行任务，不具备开启新线程的能力
   * 异步：在新的线程中执行任务，具备开启新线程的能力
   
   
 * 并发和串行主要影响：任务的执行方式
   * 并发：多个任务并发（同时）执行
   * 串行：一个任务执行完毕后，再执行下一个任务

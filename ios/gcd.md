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

##并发队列
###GCD默认已经提供了全局的并发队列，供整个应用使用，不需要手动创建
### 使用dispatch_get_global_queue函数获得全局的并发队列

```
dispatch_queue_t dispatch_get_global_queue(
dispatch_queue_priority_t priority, // 队列的优先级
unsigned long flags); // 此参数暂时无用，用0即可

dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0); // 获得全局并发队列

或者创建并发队列
dispatch_queue_t queue = dispatch_queue_create("com.520it.queue", DISPATCH_QUEUE_CONCURRENT);


全局并发队列的优先级
#define DISPATCH_QUEUE_PRIORITY_HIGH 2 // 高
#define DISPATCH_QUEUE_PRIORITY_DEFAULT 0 // 默认（中）
#define DISPATCH_QUEUE_PRIORITY_LOW (-2) // 低
#define DISPATCH_QUEUE_PRIORITY_BACKGROUND INT16_MIN // 后台
```

##串行队列
- 串行队列创建有2种方法
###方法一
```objc
dispatch_queue_create(const char *label, dispatch_queue_attr_t attr); 
const char *label // 队列名称 
dispatch_queue_attr_t attr// 队列属性,当填写NULL的时候，就是填写DISPATCH_QUEUE_SERIAL（串行的意思）
```

```objc
dispatch_queue_t queue = dispatch_queue_create("com.520it.queue", DISPATCH_QUEUE_SERIAL);
或者 
dispatch_queue_t queue = dispatch_queue_create("com.520it.queue", NULL);
```


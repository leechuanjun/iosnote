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

#例子
- ####同步函数 + 主队列：
- <font color = red> 使用sync函数往当前串行队列中添加任务，会卡住当前的串行队列<font>

```objc`
/**
 * 同步函数 + 主队列：
 */
- (void)syncMain
{
    NSLog(@"syncMain ----- begin");
    
    // 1.获得主队列
    dispatch_queue_t queue = dispatch_get_main_queue();
    
    // 2.将任务加入队列
    dispatch_sync(queue, ^{
        NSLog(@"1-----%@", [NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"2-----%@", [NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"3-----%@", [NSThread currentThread]);
    });
    
    NSLog(@"syncMain ----- end");
}

log 打印
//07-GCD的基本使用[1399:122292] syncMain ----- begin

```

- ####异步函数 + 主队列：只在主线程中执行任务，不会新建线程
- <font color = red> 特别注意：异步函数执行顺序是，先执行函数内的代码，后执行队列的代码<font>

```objc
/**
 * 异步函数 + 主队列：只在主线程中执行任务
 */
- (void)asyncMain
{
    // 1.获得主队列
    dispatch_queue_t queue = dispatch_get_main_queue();
    
    // 2.将任务加入队列
    dispatch_async(queue, ^{
        NSLog(@"1-----%@", [NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"2-----%@", [NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"3-----%@", [NSThread currentThread]);
    });
    
    NSLog(@"asyncMain--------end");
}

log 打印
 07-GCD的基本使用[1418:124881] asyncMain--------end
 07-GCD的基本使用[1418:124881] 1-----<NSThread: 0x7fbfe9602c40>{number = 1, name = main}
 07-GCD的基本使用[1418:124881] 2-----<NSThread: 0x7fbfe9602c40>{number = 1, name = main}
 07-GCD的基本使用[1418:124881] 3-----<NSThread: 0x7fbfe9602c40>{number = 1, name = main}
```

- ####同步函数 + 串行队列：不会开启新的线程，在当前线程执行任务。任务是串行的，执行完一个任务，再执行下一个任务

- <font color = red> 特别注意：异步函数执行顺序是，先执行队列的代码，后执行函数内的代码<font>

```objc
/**
 * 同步函数 + 串行队列：不会开启新的线程，在当前线程执行任务。任务是串行的，执行完一个任务，再执行下一个任务
 */
- (void)syncSerial
{
    // 1.创建串行队列
    dispatch_queue_t queue = dispatch_queue_create("com.520it.queue", DISPATCH_QUEUE_SERIAL);
    
    // 2.将任务加入队列
    dispatch_sync(queue, ^{
        NSLog(@"1-----%@", [NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"2-----%@", [NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"3-----%@", [NSThread currentThread]);
    });
    
    NSLog(@"syncSerial--------end");

}

log 打印
-GCD的基本使用[1443:127744] 1-----<NSThread: 0x7f9b697060a0>{number = 1, name = main}
-GCD的基本使用[1443:127744] 2-----<NSThread: 0x7f9b697060a0>{number = 1, name = main}
-GCD的基本使用[1443:127744] 3-----<NSThread: 0x7f9b697060a0>{number = 1, name = main}
-GCD的基本使用[1443:127744] syncSerial--------end
syncSerialsyncSerial
```



- ####异步函数 + 串行队列：会开启新的线程，但是任务是串行的，执行完一个任务，再执行下一个任务

```objc
/**
 * 异步函数 + 串行队列：会开启新的线程，但是任务是串行的，执行完一个任务，再执行下一个任务
 */
- (void)asyncSerial
{
    // 1.创建串行队列
    dispatch_queue_t queue = dispatch_queue_create("com.520it.queue", DISPATCH_QUEUE_SERIAL);
    
    // 2.将任务加入队列
    dispatch_async(queue, ^{
        NSLog(@"1-----%@", [NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"2-----%@", [NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"3-----%@", [NSThread currentThread]);
    });
    
     NSLog(@"asyncSerial--------end");
}

log 打印
GCD的基本使用[1487:132988] asyncSerial--------end
GCD的基本使用[1487:133212] 1-----<NSThread: 0x7fdcb2e35090>{number = 2, name = (null)}
GCD的基本使用[1487:133212] 2-----<NSThread: 0x7fdcb2e35090>{number = 2, name = (null)}
GCD的基本使用[1487:133212] 3-----<NSThread: 0x7fdcb2e35090>{number = 2, name = (null)}
```

- ####异步函数 + 并发队列：可以同时开启多条线程

```objc
/**
 * 异步函数 + 并发队列：可以同时开启多条线程
 */
- (void)asyncConcurrent
{
    // 1.创建一个并发队列
    // label : 相当于队列的名字
//    dispatch_queue_t queue = dispatch_queue_create("com.520it.queue", DISPATCH_QUEUE_CONCURRENT);
    
    // 1.获得全局的并发队列
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    
    // 2.将任务加入队列
    dispatch_async(queue, ^{
        for (NSInteger i = 0; i<10; i++) {
            NSLog(@"1-----%@", [NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (NSInteger i = 0; i<10; i++) {
            NSLog(@"2-----%@", [NSThread currentThread]);
        }
    });
    dispatch_async(queue, ^{
        for (NSInteger i = 0; i<10; i++) {
            NSLog(@"3-----%@", [NSThread currentThread]);
        }
    });
    
    NSLog(@"asyncConcurrent--------end");

}

log 打印
asyncConcurrent--------end
2016-07-06 19:57:35.556 07-GCD的基本使用[1520:136336] 1-----<NSThread: 0x7fd2394218c0>{number = 2, name = (null)}
2016-07-06 19:57:35.556 07-GCD的基本使用[1520:136327] 2-----<NSThread: 0x7fd239707210>{number = 3, name = (null)}
2016-07-06 19:57:35.556 07-GCD的基本使用[1520:136343] 3-----<NSThread: 0x7fd2395031d0>{number = 4, name = (null)}
```

##线程间通信示例
```objc
dispatch_async(
dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    // 执行耗时的异步操作...
      dispatch_async(dispatch_get_main_queue(), ^{
        // 回到主线程，执行UI刷新操作
        });
});
```

##一次性代码
- 使用dispatch_once函数能保证某段代码在程序运行过程中只被执行1次
```objc
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
    // 只执行1次的代码(这里面默认是线程安全的)
});
```

##队列组
- 有这么1种需求
 - 首先：分别异步执行2个耗时的操作
 - 其次：等2个异步操作都执行完毕后，再回到主线程执行操作

- 如果想要快速高效地实现上述需求，可以考虑用队列组
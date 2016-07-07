# NSOperation

##NSOperation的作用
- 配合使用NSOperation和NSOperationQueue也能实现多线程编程

##使用NSOperation子类的方式有3种
- NSInvocationOperation
- NSBlockOperation
- 自定义子类继承NSOperation，实现内部相应的方法

##NSInvocationOperation
- 注意
- 默认情况下，调用了start方法后并不会开一条新线程去执行操作，而是在当前线程同步执行操作
- 只有将NSOperation放到一个NSOperationQueue中，才会异步执行操作

```objc
- (void)test2
{
    NSInvocationOperation *op = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(print:) object:@"hello"];
    
    //不会开新线程
    [op start];
}


log 
-NSOperationQueue[1600:268010] ----------print,hello
-NSOperationQueue[1600:268010] <NSThread: 0x7fb380f00ac0>{number = 1, name = main}
```

- 加入队列会开启新线程

```objc
- (void)test2
{
    
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    
    NSInvocationOperation *op = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(print:) object:@"hello"];
    
    [queue addOperation:op];
}

- (void)print:(NSString *)argv
{
    NSLog(@"----------print,%@",argv);
    NSLog(@"%@",[NSThread currentThread]);
}

log 打印
掌握-NSOperationQueue[1614:271415] ----------print,hello
掌握-NSOperationQueue[1614:271415] <NSThread: 0x7fc610e78a90>{number = 2, name = (null)}
```

##NSBlockOperation
```objc
//创建NSBlockOperation对象
+ (id)blockOperationWithBlock:(void (^)(void))block;

//通过addExecutionBlock:方法添加更多的操作
- (void)addExecutionBlock:(void (^)(void))block;
```

- 注意：只要NSBlockOperation封装的操作数 > 1，就会异步执行操作

```objc
- (void)test3
{
       
    NSBlockOperation *op = [NSBlockOperation blockOperationWithBlock:^{
                NSLog(@"----------NSBlockOperation1");
                NSLog(@"%@",[NSThread currentThread]);
            }];
        
    [op addExecutionBlock:^{
        NSLog(@"----------NSBlockOperation2");
        NSLog(@"%@",[NSThread currentThread]);
    }];
    
    [op addExecutionBlock:^{
        NSLog(@"----------NSBlockOperation3");
        NSLog(@"%@",[NSThread currentThread]);
    }];
    
    [op start];

log 打印
掌握-NSOperationQueue[1651:279652] ----------NSBlockOperation1
掌握-NSOperationQueue[1651:279652] <NSThread: 0x7fcae3505d60>{number = 1, name = main}
掌握-NSOperationQueue[1651:279706] ----------NSBlockOperation2
掌握-NSOperationQueue[1651:279713] ----------NSBlockOperation3
掌握-NSOperationQueue[1651:279706] <NSThread: 0x7fcae360c250>{number = 2, name = (null)}
掌握-NSOperationQueue[1651:279713] <NSThread: 0x7fcae3712400>{number = 3, name = (null)}
}
```

##NSOperationQueue
- NSOperation可以调用start方法来执行任务，但默认是同步执行的
- 如果将NSOperation添加到NSOperationQueue（操作队列）中，系统会自动异步执行NSOperation中的操作

```objc
添加操作到NSOperationQueue中
- (void)addOperation:(NSOperation *)op;
- (void)addOperationWithBlock:(void (^)(void))block;
```

##

- 当队列最大并发数为1时，表示串行
```objc
- (void)test1
{
     
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    //最大并发数
    queue.maxConcurrentOperationCount = 1;
    
    [queue addOperationWithBlock:^{
        NSLog(@"----------NSBlockOperation1");
        NSLog(@"%@",[NSThread currentThread]);

    }];
    
    [queue addOperationWithBlock:^{
        NSLog(@"----------NSBlockOperation2");
        NSLog(@"%@",[NSThread currentThread]);
        
    }];
    
    [queue addOperationWithBlock:^{
        NSLog(@"----------NSBlockOperation3");
        NSLog(@"%@",[NSThread currentThread]);
        
    }];
    
    [queue addOperationWithBlock:^{
        NSLog(@"----------NSBlockOperation4");
        NSLog(@"%@",[NSThread currentThread]);
        
    }];
    
    log 打印

}
```


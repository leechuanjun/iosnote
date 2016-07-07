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
}
```
# NSTimer
* NSTimer叫做“定时器”，它的作用如下
在指定的时间执行指定的任务
每隔一段时间执行指定的任务

* 调用下面的方法就会开启一个定时任务

```objc
+ (NSTimer *)scheduledTimerWithTimeInterval:(NSTimeInterval)ti target:(id)aTarget
	selector:(SEL)aSelector
	userInfo:(id)userInfo
	repeats:(BOOL)yesOrNo;
```

- 每隔ti秒，调用一次aTarget的aSelector方法，yesOrNo决定了是否重复执行这个任务


- 通过invalidate方法可以停止定时器的工作，一旦定时器被停止了，就不能再次执行任务。只能再创建一个新的定时器才能执行新的任务

- (void)invalidate;


##解决定时器在主线程不工作的问题
```objc
NSTimer *timer = [NSTimer timerWithTimeInterval:2 target:self selector:@selector(next) userInfo:nil repeats:YES];
[[NSRunLoop mainRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];
```

## 定时任务
- 方法1：performSelector
```objc
// 1.5s后自动调用self的hideHUD方法
[self performSelector:@selector(hideHUD) withObject:nil afterDelay:1.5];
```
- 方法2：GCD

```objc
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
    // 1.5s后自动执行这个block里面的代码
    self.hud.alpha = 0.0;
});
```
- 方法3：NSTimer

```objc
// 1.5s后自动调用self的hideHUD方法
[NSTimer scheduledTimerWithTimeInterval:1.5 target:self selector:@selector(hideHUD) userInfo:nil repeats:NO];
// repeats如果为YES，意味着每隔1.5s都会调用一次self的hidHUD方法
```


![](images/Snip20150602_110.png)
![](images/Snip20150602_110.png)

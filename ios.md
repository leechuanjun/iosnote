# IOS 事件
####iOS中的事件可以分为3大类型
- 触摸事件
- 加速计事件
- 远程控制事件

####响应者对象
- 在iOS中不是任何对象都能处理事件，只有继承了UIResponder的对象才能接收并处理事件。我们称之为<font size=3 color = red> 响应者对象</font>

- UIApplication、UIViewController、UIView都继承自UIResponder，因此它们都是响应者对象，<font size=3 color = red> 都能够接收并处理事件</font>

####UIResponder内部提供了以下方法来处理事件
- 触摸事件

```objc
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event;
```


- 加速计事件

```objc
- (void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event;
- (void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event;
- (void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event;

```

- 远程控制事件

```objc
- (void)remoteControlReceivedWithEvent:(UIEvent *)event;
```


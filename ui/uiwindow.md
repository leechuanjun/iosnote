# UIWindow
- UIWindow是一种特殊的UIView，通常在一个app中一般都会有一个UIWindow

- IOS程序启动完毕后，创建的第一个视图控件是UIWindiw,接着创建控制器的View，最后将控制器的View添加到UIWindow上，于是控制器的View就显示在屏幕上

- 如果没有UIWindow,就看不见任何UI界面

####1、手动创建UIWindow
- 在程序启动完的方法里创建,并且给appDelegate的window赋值
- 必须调用[self.window makeKeyAndVisible];才能显示窗口。
- 有了窗口,接下来应该把控制器的view显示到窗口上。
- 自定义控制器
- 把控制器的view添加到窗口
- 设置窗口的根控制器rootViewController,会自动把控制器的view添加到窗口。
-
```objc
    //创建window
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];

    // 通过storyboard创建控制器
    // 加载storyboard
    // storyboard文件名，不需要带后缀
    // nil:  [NSBundle mainBundle]
    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main" bundle:nil];

     // instantiateInitialViewController：加载箭头指向的控制器
     // 创建UIViewController控制器，控制器的view并没有创建
    // 控制器的view懒加载：第一次使用的时候才会去加载，并不是创建UIViewController控制器的时候去加载
    UIViewController *viewControl = [storyboard instantiateInitialViewController];

    // 创建窗口的跟控制器
    self.window.rootViewController = viewControl;

    [self.window makeKeyAndVisible];
```

**注意4.addSubView和rootViewController的区别**
- 1、直接用addSubView,控制器会被释放,控制器就不能处理事件
- 2、直接用addSubView,控制器的view不会自动旋转。
- 3、用rootViewController,控制器不会被释放,而且控制器的view会自动旋转
- 4、旋转事件->UIApplication ->Window->rootViewController ->旋转控制器的view


###2、UIWindow补充
- 1、自己创建窗口,窗口显示出来,两个条件。
 - 1.makeKeyAndVisible
 - 2.窗口不要被释放
- 2、 keyWindow: makeKeyAndVisible会让窗口成为主窗口,并且显示出来,打印application.keyWindow

- 3、 创建的窗口交给windows这个数组管理:
 - 在创建一个窗口显示出来,一个应用程序只有一个主窗口,并且显示出来的窗口都会交给application管理,application有个Windows数组,存放显示出来的窗口,有一个例外就是状态栏,状态栏也是一个窗口,但是没有交给application管理。
- 4、还有那些是窗口
 - 键盘也是窗口,创建一个textField成为第一响应者,并且加到最里 显示在最前面,打印application.windows,就知道了。
- 5、 为什么他们会显示在最前面,因为窗口有层级,他们的层级高
- 6、 windowLevel:UIWindowLevelNormal < UIWindowLevelStatusBar < UIWindowLevelAlert
UIWindowLevelNormal : 默认窗口的层级 UIWindowLevelStatusBar : 状态栏,键盘、 UIWindowLevelAlert :UIActionSheet,UIAlearView
- 7、 把window的层级设置为UIWindowLevelAlert ,就会显示在最前面。

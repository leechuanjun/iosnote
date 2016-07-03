# TabBar中间按钮和Navigation常见配置

##TabBar中间按钮方法一

```objc
    //初始化
    [self setupTabBarChildrenWithTitle:@"精华" image:@"tabBar_essence_icon" selectImage:@"tabBar_essence_click_icon" viewController:[[UIViewController alloc] init]];
    
    [self setupTabBarChildrenWithTitle:@"新帖" image:@"tabBar_new_icon" selectImage:@"tabBar_new_click_icon" viewController:[[UIViewController alloc] init]];
    //重点是设置一个占位的控制器
    [self setupTabBarChildrenWithTitle:nil image:nil selectImage:nil viewController:[[UIViewController alloc] init]];
    
    [self setupTabBarChildrenWithTitle:@"关注" image:@"tabBar_me_icon" selectImage:@"tabBar_me_click_icon" viewController:[[UIViewController alloc] init]];
    
    [self setupTabBarChildrenWithTitle:@"我" image:@"tabBar_friendTrends_icon" selectImage:@"tabBar_friendTrends_click_icon" viewController:[[UIViewController alloc] init]];
```

```objc
//中间按钮懒加载
- (UIButton *)centerButton
{
    if (_centerButton == nil) {
        _centerButton = [UIButton buttonWithType:UIButtonTypeCustom];
        _centerButton.backgroundColor = XMGRandomColor;
        [_centerButton addTarget:self action:@selector(centerButtonClick) forControlEvents:UIControlEventTouchUpInside];
        [_centerButton setImage:[UIImage imageNamed:@"tabBar_publish_icon"] forState:UIControlStateNormal];
        [_centerButton setImage:[UIImage imageNamed:@"tabBar_publish_click_icon"] forState:UIControlStateHighlighted];
        _centerButton.frame = CGRectMake(0, 0, self.tabBar.frame.size.width / 5, self.tabBar.frame.size.height);
        _centerButton.center = CGPointMake(self.tabBar.frame.size.width / 2, self.tabBar.frame.size.height / 2);
        
    }
    return _centerButton;
}
```

```objc
/**
 *  初始化UITabBarControll
 *
 *  @param title       标题
 *  @param image       图片
 *  @param selectImage 被选择的图片
 *  @param vc          加入tabBar 的控制器
 */
- (void)setupTabBarChildrenWithTitle:(NSString *)title image:(NSString *)image selectImage:(NSString *)selectImage viewController :(UIViewController *) vc
{
    vc.tabBarItem.title = title;
    vc.view.backgroundColor = XMGRandomColor;
    if (image.length) {
        vc.tabBarItem.image = [UIImage imageNamed:image];
        vc.tabBarItem.selectedImage = [UIImage imageNamed:selectImage];
    }
    [self addChildViewController:vc];
    
}
```

##方法二（自定义UItabBar）
- 因为UITabBarController 里面的 tabBar 属性是只读的，不能修改，所以我们使用KVC

```OBJC
@property(nonatomic,readonly) UITabBar *tabBar NS_AVAILABLE_IOS(3_0); 
```

```objc
  //这样设置就等于self.tabBar = [[ZJCTabBar alloc] init]；
  [self setValue:[[ZJCTabBar alloc] init] forKey:@"tabBar"];
```

- 自定义的UItabBar

```objc
/**
 *  布局子控件
 */
- (void)layoutSubviews
{
    [super layoutSubviews];
    
    // NSClassFromString(@"UITabBarButton") == [UITabBarButton class]
    // NSClassFromString(@"UIButton") == [UIButton class]
    
    /**** 设置所有UITabBarButton的frame ****/
    // 按钮的尺寸
    CGFloat buttonW = self.frame.size.width / 5;
    CGFloat buttonH = self.frame.size.height;
    CGFloat buttonY = 0;
    // 按钮索引
    int buttonIndex = 0;
    
    for (UIView *subview in self.subviews) {
        // 过滤掉非UITabBarButton
        //if (![@"UITabBarButton" isEqualToString:NSStringFromClass(subview.class)]) continue;
        if (subview.class != NSClassFromString(@"UITabBarButton")) continue;
        
        // 设置frame
        CGFloat buttonX = buttonIndex * buttonW;
        if (buttonIndex >= 2) { // 右边的2个UITabBarButton
            buttonX += buttonW;
        }
        subview.frame = CGRectMake(buttonX, buttonY, buttonW, buttonH);
        
        // 增加索引
        buttonIndex++;
    }
    
    /**** 设置中间的发布按钮的frame ****/
    self.publishButton.frame = CGRectMake(0, 0, buttonW, buttonH);
    self.publishButton.center = CGPointMake(self.frame.size.width * 0.5, self.frame.size.height * 0.5);
}

```

```objc
//中间按钮懒加载
- (UIButton *)centerButton
{
    if (_centerButton == nil) {
        _centerButton = [UIButton buttonWithType:UIButtonTypeCustom];
        _centerButton.backgroundColor = XMGRandomColor;
        [_centerButton addTarget:self action:@selector(centerButtonClick) forControlEvents:UIControlEventTouchUpInside];
        [_centerButton setImage:[UIImage imageNamed:@"tabBar_publish_icon"] forState:UIControlStateNormal];
        [_centerButton setImage:[UIImage imageNamed:@"tabBar_publish_click_icon"] forState:UIControlStateHighlighted];
        _centerButton.frame = CGRectMake(0, 0, self.tabBar.frame.size.width / 5, self.tabBar.frame.size.height);
        _centerButton.center = CGPointMake(self.tabBar.frame.size.width / 2, self.tabBar.frame.size.height / 2);
        
    }
    return _centerButton;
}
```

![](屏幕快照 2016-07-02 16.16.10.png)


/Users/zhongjc_bill/ios/work/百思不得姐/Baisi/百思不得姐/百思不得姐/Classes/Other - 其他/Category/UIBarButtonItem+ZJC.m:23:1: Convenience initializer missing a 'self' call to another initializer
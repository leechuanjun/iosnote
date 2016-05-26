# 隐藏Status bar
###在iOS7以后，状态栏默认由控制器决定
```objc
// 在iOS7以后，状态栏默认由控制器决定
// 隐藏状态栏
- (BOOL)prefersStatusBarHidden
{
    return YES;
}

- (UIStatusBarStyle)preferredStatusBarStyle
{
    return UIStatusBarStyleLightContent;
}
```

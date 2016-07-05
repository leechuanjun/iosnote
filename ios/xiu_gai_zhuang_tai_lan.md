# 修改状态栏

## 修改状态栏样式

###修改状态栏有两种方法，第一种旧方法利用 UIApplication 来设置 
- 使用UIApplication来管理


![](Snip20151108_152.png)
```objc
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
```
在Info.plist中做了图中的配置,可能会出现以下警告信息

![](Snip20151108_153.png)
![](Snip20151108_153.png)


##第二种方法
- 使用UIViewController来管理（直接在控制器添加就行）

```objc
@implementation XMGLoginRegisterViewController
- (UIStatusBarStyle)preferredStatusBarStyle
{
    return UIStatusBarStyleLightContent;
}

//隐藏状态栏
- (BOOL)prefersStatusBarHidden
{
  return YES;
}

@end
```
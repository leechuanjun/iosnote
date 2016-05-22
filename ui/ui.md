# UI的变形
- **transform:形变属性，能完成的功能：平移、缩放、旋转**
- 利用动画来实现
```objc
   [UIView animateWithDuration:2.0 animations:^{

       // 缩放
        self.tempView.transform = CGAffineTransformMakeScale(0.5, 0.5);

        // 平移
        self.tempView.transform = CGAffineTransformMakeTranslation(-100, 100);

        // 旋转objc
        self.tempView.transform = CGAffineTransformMakeRotation(-M_PI_4);

        CGAffineTransform translation = CGAffineTransformMakeTranslation(-100, 100);

        CGAffineTransform scaleTranslation = CGAffineTransformScale(translation, 0.5, 0.5);

        CGAffineTransform rotateScaleTranslation = CGAffineTransformRotate(scaleTranslation, M_PI_2);

        self.tempView.transform = rotateScaleTranslation;
   }];
```
- 有一种情况是根据上一个状态来变形的
- eg 根据tempView的上一个状态 加上缩放 1/4
- 注意 1/4 可以写M_PI_4

```objc
 self.tempView.transform = CGAffineTransformRotate(self.tempView.transform, M_PI_4);

```

- 清空transform，以前的平移、缩放、旋转都会消失
```objc
     self.tempView.transform = CGAffineTransformIdentity;
```

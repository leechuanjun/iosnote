# UIGestureRecognizer(手势)
- 为了完成手势识别，必须借助于手势识别器----UIGestureRecognizer
- 利用UIGestureRecognizer，能轻松识别用户在某个view上面做的一些常见手势

- UIGestureRecognizer是一个抽象类，定义了所有手势的基本行为，使用它的子类才能处理具体的手势
```objc
UITapGestureRecognizer(敲击)
每一个手势识别器的用法都差不多，比如UITapGestureRecognizer的使用步骤如下
创建手势识别器对象
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] init];

设置手势识别器对象的具体属性
// 连续敲击2次
tap.numberOfTapsRequired = 2;
// 需要2根手指一起敲击
tap.numberOfTouchesRequired = 2;

添加手势识别器到对应的view上
[self.iconView addGestureRecognizer:tap];

监听手势的触发
[tap addTarget:self action:@selector(tapIconView:)];
UIPinchGestureRecognizer(捏合，用于缩放)
UIPanGestureRecognizer(拖拽)
UISwipeGestureRecognizer(轻扫)
UIRotationGestureRecognizer(旋转)
UILongPressGestureRecognizer(长按)
```

###UITapGestureRecognizer(敲击)
每一个手势识别器的用法都差不多，比如UITapGestureRecognizer的使用步骤如下
创建手势识别器对象
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] init];

设置手势识别器对象的具体属性
// 连续敲击2次
tap.numberOfTapsRequired = 2;
// 需要2根手指一起敲击
tap.numberOfTouchesRequired = 2;

添加手势识别器到对应的view上
[self.iconView addGestureRecognizer:tap];

监听手势的触发
[tap addTarget:self action:@selector(tapIconView:)];

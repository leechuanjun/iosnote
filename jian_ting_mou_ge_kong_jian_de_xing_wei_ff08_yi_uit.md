# 监听某个控件的行为（以UITextField为例）

###改变UITextField 占位字符的颜色

##知识点
## 修改UITextField的光标颜色
```objc
textField.tintColor = [UIColor whiteColor];
```

## UITextField占位文字相关的设置
```objc
// 设置占位文字内容
@property(nullable, nonatomic,copy)   NSString               *placeholder;
// 设置带有属性的占位文字, 优先级 > placeholder
@property(nullable, nonatomic,copy)   NSAttributedString     *attributedPlaceholder;
```

## NSAttributedString
- 带有属性的字符串, 富文本
- 由2部分组成
    - 文字内容 : NSString *
    - 文字属性 : NSDictionary *
        - 文字颜色 - NSForegroundColorAttributeName
        - 字体大小 - NSFontAttributeName
        - 下划线 - NSUnderlineStyleAttributeName
        - 背景色 - NSBackgroundColorAttributeName
- 初始化

```objc
NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
attributes[NSForegroundColorAttributeName] = [UIColor yellowColor];
attributes[NSBackgroundColorAttributeName] = [UIColor redColor];
attributes[NSUnderlineStyleAttributeName] = @YES;
NSAttributedString *string = [[NSAttributedString alloc] initWithString:@"123" attributes:attributes];
```
- 使用场合
    - UILabel - attributedText
    - UITextField - attributedPlaceholder

- ###方法一 利用KVC来做
```objc
static NSString * const XMGPlaceholderColorKey = @"placeholderLabel.textColor";

 [self setValue:[UIColor grayColor] forKeyPath:XMGPlaceholderColorKey];
```

- ##方法二 利用NSAttributedString
```objc
   self.tintColor = [UIColor whiteColor];
    
    NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
    
    attributes[NSForegroundColorAttributeName] = [UIColor whiteColor];
    
    self.attributedPlaceholder  = [[NSAttributedString alloc] initWithString:self.placeholder attributes:attributes];
```

- ##监听某个控件的行为

- ###方法一（如果某个控件继承UIControl,如UITextField）
```objc

```

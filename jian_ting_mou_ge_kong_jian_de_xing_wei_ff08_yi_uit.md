# 监听某个控件的行为（以UITextField为例）

###改变UITextField 占位字符的颜色
- ###方法一 利用KVC来做
```objc
static NSString * const XMGPlaceholderColorKey = @"placeholderLabel.textColor";

 [self setValue:[UIColor grayColor] forKeyPath:XMGPlaceholderColorKey];
```

- ##方法二 利用
```objc
   self.tintColor = [UIColor whiteColor];
    
    NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
    
    attributes[NSForegroundColorAttributeName] = [UIColor whiteColor];
    
    self.attributedPlaceholder  = [[NSAttributedString alloc] initWithString:self.placeholder attributes:attributes];
```
# textFiled
###当输入条太靠近文本框左边，可以设置一个leftView 来设置文本框

```objc
// 设置文本框左边的内容
    UIView *leftView = [[UIView alloc] init];
    leftView.frame = CGRectMake(0, 0, 10, 0);
    self.messageField.leftView = leftView;
    self.messageField.leftViewMode = UITextFieldViewModeAlways;
```


 - 通过UITextField的代理方法能够监听键盘最右下角按钮的点击 成为UITextField的代理 - self.textField.delegate = self; 

- 遵守UITextFieldDelegate协议,实现代理方法
- -(BOOL)textFieldShouldReturn:(UITextField *)textField;
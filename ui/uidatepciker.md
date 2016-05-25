# UIDatePciker
####利用textField 来实现弹出日期选择

1、点击输入框的时候，不可以输入内容

**注意：不能使用enable = NO,因为这样就不能输入内容和监听利用textField的点击**

```objc
// 是否允许用户输入文字
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    return NO;
}
```

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    //开始的初始化
    [self setupBrithTextField];
}

```

2、 新建UIDatePicker控件，然后把TextField.inputView设置为datePicker

```objc

- (void)setupBrithTextField
{
    UIDatePicker *datePicker = [[UIDatePicker alloc] init];

    self.datePicker = datePicker;

    //设置地域语言
    datePicker.locale = [NSLocale localeWithLocaleIdentifier:@"zh"];

    //设置显示时间格式
    datePicker.datePickerMode = UIDatePickerModeDate;

    self.birthTextField.inputView = datePicker;

    [datePicker addTarget:self action:@selector(changeDateToString:) forControlEvents:UIControlEventValueChanged];

}

- (void)changeDateToString:(UIDatePicker *)datePicker
{
    NSDate *date = datePicker.date;

    NSDateFormatter *format = [[NSDateFormatter alloc] init];

     //设置输出时间格式
    format.dateFormat = @"yyyy-MM-dd";

    self.birthTextField.text = [format stringFromDate:date];
}

```

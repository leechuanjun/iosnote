# UIPickView
* 1.UIPickView什么时候用?
通常在注册模块,当用户需要选择一些东西的时候,比如说城市,往往弹出一个PickerView给他们选择。

* 2.UIPickView常见用法,演示实例程序
 - 1、独立的,没有任何关系 =>菜单系统。
 - 2、 相关联的,下一列和第一列有联系=>省会城市选择
 - 3、图文并帽,=>国旗选择。


* 3.UIDatePicker什么时候用? 当用户选择日期的时候,一般弹出一个UIDatePicker给用户选择。


###1.搭建界面
**1、 注意点:PickerView的高度不能改,默认162,PickerView里面每行的高度 可以改,不要弄混淆了。**

###2.pickerView显示数据
- 1、如何使用PickerView展示数据? 进入PickerView头文件,有数据源和代理,联想到UITableView,模仿 UITableView的用法。

- 2、让控制器作为PickerView的数据源,控制器遵守PickerView的数据源方法
  - 2.1>两种方式:1.拖线 2.代码
  - 2.2>系统自带的控件,数据源和代理属性不需要IBOutlet,也能拖 线。自己的属性,想要拖线,必须写IBOutlet。

- 3、 PickerView的数据源方法
   - 1、 numberOfComponentsInPickerView: 返回多少列
   - 2、 pickerView:numberOfRowsInComponent: 返回第component列有多少 行
   - 3、和UITableView的区别,每一行长什么样,是由PickerView的代理决 定的。
   - 4、注意:如果没有返回每一行长什么样子,每行就会显示?,看见?,就 知道没有实现每一行长什么样子的方法。

- 4、 PickerView的代理方法

```objc

//返回第component列第row行长什么样。
//第component列第row行的展示标题
- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component

// 第component列第row行带属性的标题
- (NSAttributedString *)pickerView:(UIPickerView *)pickerView attributedTitleForRow:(NSInteger)row forComponent:(NSInteger) component

//第component列第row行展示的视图
- (UIView *)pickerView:(UIPickerView *)pickerView viewForRow:(NSInteger)row forComponent:(NSInteger)component reusingView:(UIView *)view;

//返回第component列每一行的高度和宽度
- (CGFloat)pickerView:(UIPickerView *)pickerView widthForComponent:(NSInteger)component;
- (CGFloat)pickerView:(UIPickerView *)pickerView rowHeightForComponent:(NSInteger)component;

//选中第component列第row行调用
- (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component;
```

- 5.随机选中某一列的某一行
 - 1、 如何选中某一行 [self.pickerView selectRow:row inComponent:component
animated:YES];</br></br>
 - 2、 先随机选中第0列的某一行,随机数取值范围看第0列总共有多少行,
arc4random_uniform(x)随机0~x-1的数</br></br>
 - 3、 避免随机出来的行数都一样,需要判断下,随机出来的行数和当前选中 的是否一样,一样就重新随机,用while判断,直到随机到不一样,才行。
    -  问题:label没有显示最新选中的一行。
原因:手动调用pickview滚动,选中某一行,不会触发代理,我们自己 主动调用代理,让lebel显示选中哪一行.
注意:只有用户手动滚动才可以触发pickview的代理方法。
  - 4、 每一列都要随机选中,弄个for循序,遍历每一列都随机选中



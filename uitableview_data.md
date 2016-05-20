# UITableView 数据封装

- 既有section,也有row


```objc
/**
 *  告诉tableView第section组有多少行
 */
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    XMGCarGroup *group = self.groups[section];
    return group.cars.count;
}

/**
 *  告诉tableView一共有多少组数据
 */
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    return self.groups.count;
}

/**
 *  告诉tableView第indexPath行显示怎样的cell
 */
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [[UITableViewCell alloc] init];

    XMGCarGroup *group = self.groups[indexPath.section];
    XMGCar *car = group.cars[indexPath.row];

    cell.textLabel.text = car.name;
    cell.imageView.image = [UIImage imageNamed:car.icon];

    return cell;
}
```



```objc
  // 创建模型数据
        XMGCarGroup *group0 = [[XMGCarGroup alloc] init];
        group0.header = @"德系";
        group0.footer = @"德系车666";
        group0.cars = @[
                        [XMGCar carWithName:@"奥迪" icon:@"m_9_100"],
                        [XMGCar carWithName:@"宝马" icon:@"m_3_100"],
                        [XMGCar carWithName:@"奔驰" icon:@"m_2_100"]
                        ];

```


- section 的实体对象

```objc
@interface XMGCarGroup : NSObject
/** 头部标题 */
@property (nonatomic, strong) NSString *header;
/** 尾部标题 */
@property (nonatomic, strong) NSString *footer;
/** 这组所有的车辆模型(这个数组里面存放的都是XMGCar模型) */
@property (nonatomic, strong) NSArray *cars;
@end
```
- row 的实体对象

```objc
@interface XMGCar : NSObject
/** 名字 */
@property (nonatomic, strong) NSString *name;
/** 图标 */
@property (nonatomic, strong) NSString *icon;

+ (instancetype)carWithName:(NSString *)name icon:(NSString *)icon;
@end

```


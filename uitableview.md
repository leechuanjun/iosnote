# UITableview数据更新
##**数据更新的重点是模型的数据的更新**

- 添加数据

```objc
/**
 *  添加数据
 */
- (IBAction)addData {

    ZJCDeal * deal = [[ZJCDeal alloc] init];
    deal.title = @"大家好我是新添加的";
    deal.icon = @"2c97690e72365e38e3e2a95b934b8dd2";
    deal.buyCount = @"100";
    deal.price = @"2222";

    [self.array addObject:deal];

    [self.tableview insertRowsAtIndexPaths:@[[NSIndexPath indexPathForRow:0 inSection:0]] withRowAnimation:UITableViewRowAnimationLeft];

//多行重新读取数据（效率比较低）
//    [self.tableview reloadData];


}
```

- 更新数据

```objc
- (IBAction)updateData {

    ZJCDeal * deal = self.array[0];

    deal.title = @"大家好我是更新的";
    deal.icon = @"2c97690e72365e38e3e2a95b934b8dd2";
    deal.buyCount = @"222";
    deal.price = @"2222";

    [self.tableview reloadData];

}
```
- 删除数据</br>

```objc
/**
 *  删除数据
 */
- (IBAction)deleteData {

//判断该数据数组是否为0
    if (self.array.count != 0) {
        [self.array removeObjectAtIndex:0];
        [self.tableview deleteRowsAtIndexPaths:@[[NSIndexPath indexPathForRow:0 inSection:0]] withRowAnimation:UITableViewRowAnimationRight];
    }

}
```

###删除批量数据
- 方法一

```objc
//更改module 里面的数据（判断语句）

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    [tableView deselectRowAtIndexPath:indexPath animated:YES];

    //取出点击的cell,改变cell的check(YES改为NO,NO改为YES)；
    ZJCDeal *deal =  self.array[indexPath.row];
    deal.check = !deal.isCheck;

    //刷新数据源，显示或者隐藏check图片
    [tableView reloadData];
}

/**
 *  删除数据
 */
- (IBAction)deleteData {

    if (self.array.count != 0) {
        //  新建一个array来保存已经选了check的状态
        NSMutableArray *checkArray = [NSMutableArray array];
        for (ZJCDeal *deal in self.array) {
            if (deal.isCheck) {
                [checkArray addObject:deal];
            }
        }
        //移除数据源的数据
        [self.array removeObjectsInArray:checkArray];
        //更新数据源
        [self.tableview reloadData];
    }

}
```

- 方法二

```objc

/** 即将要删除的 */
@property (nonatomic, strong) NSMutableArray *deletedDeals;

- (IBAction)remove {
    // 删除模型数据
    [self.deals removeObjectsInArray:self.deletedDeals];
    // 刷新表格
    [self.tableView reloadData];
    // 清空数组
    [self.deletedDeals removeAllObjects];
}

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 取消选中这一行
    [tableView deselectRowAtIndexPath:indexPath animated:YES];

    // 取出模型
    XMGDeal *deal = self.deals[indexPath.row];
    //检查删除数组是否包含删除对象，如果包含，就删除对象，如果没有，就添加到删除数组
    if ([self.deletedDeals containsObject:deal]) {
        [self.deletedDeals removeObject:deal];
    } else {
        [self.deletedDeals addObject:deal];
    }

    // 刷新表格
    [tableView reloadData];
}
```

- 编辑模式

```objc
  // 进入编辑模式
    //    self.tableView.editing = YES;
    [self.tableView setEditing:!self.tableView.isEditing animated:YES];

/**
 * 只要实现这个方法，左划cell出现删除按钮的功能就有了
 * 用户提交了添加（点击了添加按钮）\删除（点击了删除按钮）操作时会调用
 */
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{
    if (editingStyle == UITableViewCellEditingStyleDelete) {  // 点击了“删除”
        // 删除模型
        [self.deals removeObjectAtIndex:indexPath.row];

        // 刷新表格
        [self.tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationLeft];
    } else if (editingStyle == UITableViewCellEditingStyleInsert) { // 点击了+
        NSLog(@"+++++ %zd", indexPath.row);
    }
}

/**
 * 这个方法决定了编辑模式时，每一行的编辑类型：insert（+按钮）、delete（-按钮）
 */
- (UITableViewCellEditingStyle)tableView:(UITableView *)tableView editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return indexPath.row % 2 == 0? UITableViewCellEditingStyleInsert: UITableViewCellEditingStyleDelete;
}
```

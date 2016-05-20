# UITableView 不等高cell

- ####**不等高的关键在于算出每个cell的高度，tableView 执行方法的顺序是先执行heightForRowAtIndexPath ，然后在执行 cellForRowAtIndexPath ，但是实现了方法 estimatedHeightForRowAtIndexPath（预计高度方法)之后 ，顺序会变成先执行cellForRowAtIndexPath 然后再执行 heightForRowAtIndexPath ，然后我们只要在set方法中算好每个cell的高度，然后保存在module里面，在heightForRowAtIndexPath 方法中返回高度就行了**


```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatusCell *cell = [XMGStatusCell cellWithTableView:tableView];
    cell.status = self.statuses[indexPath.row];
    return cell;
}

#pragma mark - 代理方法
/**
 *  返回每一行的高度
 */
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    XMGStatus *staus = self.statuses[indexPath.row];
    return staus.cellHeight;
}

/**
 * 返回每一行的估计高度
 * 只要返回了估计高度，那么就会先调用tableView:cellForRowAtIndexPath:方法创建cell，再调用tableView:heightForRowAtIndexPath:方法获取cell的真实高度
 */
- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 200;
}
```

```objc

- (void)awakeFromNib
{
    // 设置label每一行文字的最大宽度
   // 为了保证计算出来的数值 跟 真正显示出来的效果 一致
    self.contentLabel.preferredMaxLayoutWidth = [UIScreen mainScreen].bounds.size.width - 20;
}



- (void)setStatus:(XMGStatus *)status
{
    _status = status;

    if (status.isVip) {
        self.nameLabel.textColor = [UIColor orangeColor];
        self.vipView.hidden = NO;
    } else {
        self.nameLabel.textColor = [UIColor blackColor];
        self.vipView.hidden = YES;
    }

    self.nameLabel.text = status.name;
    self.iconView.image = [UIImage imageNamed:status.icon];
    if (status.picture) {
        self.pictureView.hidden = NO;
        self.pictureView.image = [UIImage imageNamed:status.picture];
    } else {
        self.pictureView.hidden = YES;
    }
    self.contentLabel.text = status.text;

    // 强制布局
    [self layoutIfNeeded];

    // 计算cell的高度
    if (self.pictureView.hidden) { // 没有配图
        status.cellHeight = CGRectGetMaxY(self.contentLabel.frame) + 10;
    } else { // 有配图
        status.cellHeight = CGRectGetMaxY(self.pictureView.frame) + 10;
    }
}
```

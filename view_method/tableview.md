## tableview

**1. 选中效果自动消失:**

`IOS`的tableView所选中的当前行，选中效果会一直存在，如果想再手指点击后让选中效果自动消失（类似于touch out事件），可以用一个很二的方法，点击后立刻让当前行状态变为未选中状态：

```objc
// 选中某一行的事件
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath{

    // 100ms后选中效果消失
    [self performSelector:@selector(deselect) withObject:nil afterDelay:0.1f];
}

- (void)deselect
{
    [self.newsTableView deselectRowAtIndexPath:[self.newsTableView indexPathForSelectedRow] animated:YES];
}
```

**2. 去掉默认tableView边线**

有很多时候可能需要隐藏或者替换掉默认`tableView`的分割线自定义分割线，思路是隐藏默认分割线，然后再自定义一个`view`作为分割线样式插入到cell的`contentView`中：

```objc
// 去掉默认的分割线样式
tableView.separatorStyle = UITableViewCellSeparatorStyleNone;

// cell contentView设置边界线
UIView *dividerLine = [[UIView alloc] init];
dividerLine.frame = CGRectMake(0,0,320,1);
dividerLine.backgroundColor = [UIColor redColor];
[tableView.contentView addSubview:dividerLine];

```

**3. 增加tableView内边距**

当使用代码布局时，布局的自适配需要注意，如果把`tableView`放在`NavigationController`下，你会发现`tableView`的第一个单元格会被`NavigationController`的导航条遮挡一部分，因此需要做一个细节处理：

```objc
// 添加一个内边距，导航控制器默认高度是64
UIEdgeInsets insets = UIEdgeInsetsMake(64, 0, 0, 0);
self.weiBoTable = tableView;
self.weiBoTable.contentInset = insets;
// 设置滚动条也适配，不然导航条看上去很怪异
self.weiBoTable.scrollIndicatorInsets = insets;
[self.view addSubview:tableView];
```


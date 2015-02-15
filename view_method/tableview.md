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

估计只有我这么二的人才会这么干，-_- !


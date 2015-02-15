## Modal 模式

好吧，姑且叫他模态模式吧。

例如：A 切换到 B

在A中要做的是，调用`presentViewController`方法：

```objc
AController *pageA = [[AController alloc] init];

[self presentViewController:pageA animated:YES completion:^{
        NSLog(@"two page");
}];
```

B 回到 A，在B中调用`dismissViewControllerAnimated`:

```objc
[self dismissViewControllerAnimated:YES completion:^{
    // kill myself
}]
```

modal模式切换的`controller`只会切换当前窗口的view，而当前窗口的跟控制器不会变化。就刚才的例子来说，当A->B时，界面view是B的view，但是此时的窗口更控制器确实`AController`，不妨在B中打印当期那控制器试试：

```objc
//B中打印
NSLog(@"%@",self.view.window.rootViewController);
NSLog(@"%@",self.view.window.subviews);
```
可能显示如下信息：

```objc
ModalTut[800:25628] <AController: 0x7fe083592f90>
ModalTut[800:25628] (
    "<UITransitionView: 0x7fe083726190; frame = (0 0; 375 667); autoresize = W+H; layer = <CALayer: 0x7fe0837266f0>>"
)
```
可见当前`Controller`仍然是`AController`只是界面临时换成了`B`界面。

**原理**

虽然切换到B时使用的仍是`AController`，但是`AController`有一个属性性引用着`BController`:

```objc
// A中
[self presentedViewController];
```
同样B中也有来源A的引用：

```objc
// B中
[self presentingViewController];
```

**modal中的数据传递**

数据传递路径：A -> B

如果`modal`控制器是通过`StoryBorad`来建立的，那么可以使用:

```objc
-(void) prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender{
    // 拿到目标控制器，当前是导航控制器包裹了一层
    UINavigationController *toParent = segue.destinationViewController;

    // 找到当前栈顶控制器
    TwoController *pageTwo = (TwoController *)[toParent topViewController];

    // 传值
    pageTwo.TestName = @"data"

}
```

数据传递路径：B -> A

```objc
// 使用delegate即可
```



## segue

**segue类型：**

```objc
1. 自动跳转（直接从控件拖线到目标控制器，自动跳转）
2. 手动跳转（从来源控制器拖线目标控制器）
```
如果调转界面不需要逻辑处理则使用自动跳转，否则使用手动跳转类型。

手动segue需要设置segue的identifier属性，并且由来源控制器调用`perform`方法来执行跳转:

```objc
[self performSegueWithIdentifier:@"abc" sender:nil];
```

## 直接切换`rootViewController`

直接切换`window`的`rootViewController`也可以到达切换控制器的方式：

```objc
// 切换到的页面
NSString *storyboardId = @"MainPage";
UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main" bundle:nil];
MainViewController *initViewController = [storyboard instantiateViewControllerWithIdentifier:storyboardId];

initViewController.loginData = userData;

AppDelegate *appDelegate = [UIApplication sharedApplication].delegate;
appDelegate.window.rootViewController = initViewController;

[appDelegate.window makeKeyAndVisible];

```

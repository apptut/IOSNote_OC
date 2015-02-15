## 动态启动controller

在做应该开发时，常会碰到根据不同条件加载不同的`UIViewController`，如下：

```objc
if (首次登陆){
    // 欢迎界面
}else{
    // 主界面
}
```

在没有引入`storyboard`之前，在`xcode`没有取消`EmptyApplicatoin`模板之前，这些事不麻烦，但是现在`xcode`取消了建立空模板项目的代码，只能构建`single Application`模板，这样生成的代码，默认就是通过`storyboard`加载应用默认以及配置好的`rootViewController`，在`AppDelegate`下的`didFinishLaunchingWithOptions`方法里什么都没有

因此需要改造此方法中的内容：

```objc

BOOL isFirst = ...; // 检测是否第一次启动app

NSString *storyboardId = isLoggedIn ? @"MainIdentifier" : @"LoginIdentifier";

self.window.rootViewController = [self.window.rootViewController.storyboard instantiateViewControllerWithIdentifier:storyboardId];


```

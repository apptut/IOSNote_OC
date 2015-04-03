# NSNotificationCenter

`NSNotificationCenter`是`Foundation` 框架的一个子系统，它向应用程序中注册为某个事件观察者的所有对象发送广播消息（即通知）。（从编程角度而言，它是 NSNotificationCenter 类的实例）。

该事件可以是发生在应用程序中的任何事情，例如进入后台状态，或者用户开始在文本栏中键入。

实现`NSNotificationCenter`的原理是一个观察者模式，获得`NSNotificationCenter`的方法只有一种，那就是`[NSNotificationCenter defaultCenter]` ，通过调用静态方法`defaultCenter`就可以获取这个通知中心的对象了，而`NSNotificationCenter`是一个单例模式，而这个对象会一直存在于一个应用的生命周期。


### 1. 常用系统通知

#### 1.1 软件盘通知

```objc
UIKeyboardWillShowNotification  键盘即将打开
UIKeyboardDidShowNotification   键盘打开
UIKeyboardWillHideNotification  键盘即将关闭
UIKeyboardDidHideNotification   键盘已经关闭

// 更多事件参考 UIWindow类文件

```

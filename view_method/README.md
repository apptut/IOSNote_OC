## view 常用方法

**获取当前屏幕的宽高**

```objc
CGRect screenRect = [[UIScreen mainScreen] bounds];
CGFloat screenWidth = screenRect.size.width;
CGFloat screenHeight = screenRect.size.height;
```

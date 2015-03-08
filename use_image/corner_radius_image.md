## 圆角图片

使用`layer`操作图层

```objc
UIImageView *avatar = [[UIImageView alloc] init];
avatar.layer.cornerRadius = 25;
avatar.clipsToBounds = YES;
```

## 常用动画

#### 1. 透明度

<strong style="color:green">Swift:</strong>

```swift
anim.keyPath = "opacity";
anim.fromValue = NSNumber.init(float:1.0);
anim.toValue = NSNumber.init(float:0.0);
```

<strong style="color:green">Object-C:</strong>

```objc
anim.keyPath = @"opacity";
anim.fromValue = [NSNumber numberWithFloat:1.0];
anim.toValue = [NSNumber numberWithFloat:0.0];
```

### 2. 平移动画

平移动画可以使用`position`

<strong style="color:green">Swift:</strong>

```swift
anim.keyPath = "position";
anim.fromValue = NSValue.init(CGPoint:CGPointMake(0, 0));
anim.toValue = NSValue.init(CGPoint:CGPointMake(100, 100));
```
或者使用 `transform`

```swift
let anim = CABasicAnimation();
anim.duration = 1;
anim.keyPath = "transform";
anim.toValue = NSValue(CATransform3D:CATransform3DMakeTranslation(100, 100, 0));


```

<strong style="color:green">Object-C:</strong>

```objc
anim.keyPath = @"position";
anim.fromValue = [NSValue valueWithCGPoint:CGPointMake(0, 0)];
anim.toValue = [NSValue valueWithCGPoint:CGPointMake(200, 200)];
```
或者使用 `transform`

```objc
anim.keyPath = @"transform";
anim.toValue = [NSValue valueWithCATransform3D:CATransform3DMakeTranslation(100, 100, 0)];

```

### 3. 缩放动画

利用`bounds`来缩放动态改变元素的宽高实现：

<strong style="color:green">Swift:</strong>

```swift
let anim = CABasicAnimation();
anim.duration = 1;
anim.keyPath = "bounds";
anim.toValue = NSValue(CGRect:CGRectMake(0, 0, 150, 150));
```
或者利用`trnasform`和`CATransform3DMakeScale`属性：

```swift
let anim = CABasicAnimation();
anim.duration = 1;
anim.keyPath = "transform";
anim.toValue = NSValue(CATransform3D:CATransform3DMakeTranslation(100, 100, 0));

```
<strong style="color:green">Object-C:</strong>

```objc
anim.keyPath = @"bounds";
anim.toValue = [NSValue valueWithCGRect:CGRectMake(0, 0, 150, 150)];
```
或者利用`trnasform`和`CATransform3DMakeScale`属性：

```objc
anim.keyPath = @"transform";
anim.fromValue = [NSValue valueWithCATransform3D:CATransform3DMakeScale(1, 1, 1)];
anim.toValue = [NSValue valueWithCATransform3D:CATransform3DMakeScale(2, 2, 1)];

```

### 4. 旋转动画

<strong style="color:green">Swift:</strong>

```swift
let anim = CABasicAnimation();
anim.duration = 1;
anim.keyPath = "transform";
anim.toValue = NSValue(CATransform3D:CATransform3DMakeRotation(CGFloat(M_PI_4), 0, 0, 1));

```

<strong style="color:green">Object-C:</strong>

```objc
anim.keyPath = @"transform";
// 旋转180°
anim.toValue = [NSValue valueWithCATransform3D:CATransform3DMakeRotation(M_PI_2, 0, 0, 1)];

```

### 5. 背景颜色
<strong style="color:green">Swift:</strong>

```swift
let anim = CABasicAnimation();
anim.duration = 1;
anim.keyPath = "backgroundColor";
anim.toValue = UIColor.redColor().CGColor;
```
<strong style="color:green">Object-C:</strong>

```objc
anim.keyPath = @"backgroundColor";
anim.toValue = (id)[UIColor redColor].CGColor;
```

### 6. 组合动画

如果同时需要执行多个动画，那么需要使用`CAAnimationGroup`类

<strong style="color:green">Swift:</strong>

```swift
let animBg = CABasicAnimation();
animBg.keyPath = "backgroundColor";
animBg.toValue = UIColor.redColor().CGColor;

let animPos = CABasicAnimation();
animPos.keyPath = "position";
animPos.toValue = NSValue.init(CGPoint:CGPointMake(100, 100));

let animGroup = CAAnimationGroup();
let anims = [animBg,animPos];

animGroup.animations = anims;
animGroup.duration = 1;

animGroup.fillMode = kCAFillModeForwards;
animGroup.removedOnCompletion = false;
```
<strong style="color:green">Object-C:</strong>

```objc
// 改变颜色
CABasicAnimation *animBg = [CABasicAnimation animation];
animBg.keyPath = @"backgroundColor";
animBg.toValue = (id)[UIColor redColor].CGColor;

// 改变位置
CABasicAnimation *animPos = [CABasicAnimation animationWithKeyPath:@"position"];
animPos.toValue = [NSValue valueWithCGPoint:CGPointMake(100, 100)];

// 使用动画组合
CAAnimationGroup *animGroup = [CAAnimationGroup animation];
animGroup.animations = [NSArray arrayWithObjects:animBg,animPos, nil];

// 设置统一的参数
animGroup.duration = 1;
animGroup.fillMode = kCAFillModeForwards;
animGroup.removedOnCompletion = NO;
```


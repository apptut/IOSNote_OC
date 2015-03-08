## CABasiceAnimation

`CABasicAnimation`继承自`CAPropertyAnimation`类，可以实现一些基本的动画效果（由类名就可以看出）。

`CABasicAnimation` 只能使`CALayer`的某个属性从一个值(fromValue)执行动画过渡到另一个值(toValue)。如果需要设置多个动画关键帧，那么需要使用`CAKeyframeAnimation`类。

### 1. 动画平移

动画平移有很多实现方式，可以改变UI对象的`postioin`来达到平移效果：

<strong style="color:green">Swift:</strong>

```swift
@IBOutlet weak var animElem: UIView!;

//---------- 代码分割线 -----------

// 动画执行1秒
anim.duration = 1;

// 动画执行1次
anim.repeatCount = 1;

// 使用imgAnim.position 做动画
anim.keyPath = "position";

// 动画结束值
anim.toValue = NSValue.init(CGPoint:CGPointMake(200, 200));

// 运动后保持状态
anim.removedOnCompletion = fase;
anim.fillMode = kCAFillModeForwards;

// 设置动画缓动模式
anim.timingFunction = CAMediaTimingFunction.init(name:"easeIn");

// 绑定当前动画到imgAnim.layer上
self.animElem.layer.addAnimation(anim, forKey: "translateTag");

```
<strong style="color:green">Object-C:</strong>

```objc
// 图片控件
@property (weak, nonatomic) IBOutlet UIImageView *imgAnim;

//---------- 代码分割线 -----------

// 初始化动画子对象，使用属性动画对象
CABasicAnimation *anim = [CABasicAnimation animation];

// 动画执行1秒
anim.duration = 1;

// 动画执行1次
anim.repeatCount = 1;

// 使用imgAnim.position 做动画
anim.keyPath = @"position";

// 动画起始值
anim.fromValue = [NSValue valueWithCGPoint:self.imgAnim.center];

// 动画结束值
anim.toValue = [NSValue valueWithCGPoint:CGPointMake(100, 100)];

// 运动后保持状态
anim.removedOnCompletion = NO;
anim.fillMode = kCAFillModeForwards;

// 设置动画缓动模式
anim.timingFunction = [CAMediaTimingFunction functionWithName:@"easeInEaseOut"];

// 绑定当前动画到imgAnim.layer上
[self.imgAnim.layer addAnimation:anim forKey:@"translateTag"];

```

`keypath`属性用来指定动画作用的对象，`fromValue`用来指定初始化值，而`toValue`用来指定最终移动的位置。


`addAnimation:forKey:`用于绑定当前动画到图层上，`forKey`表示给动画一个tag名称，便于将来使用CALayer的`removeAnimationForKey`来删除指定动画。



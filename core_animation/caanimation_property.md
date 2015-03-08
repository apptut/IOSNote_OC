## CAAnimation 常用属性

`CAAnimation`继承自`CAMediaTiming`协议，定义了一些列动画基本属性，并且请记住一点，`CAAnimation`是一个抽象类，不能直接使用。

### 常用属性：

```js
1. timingFunction     设置动画缓动模式
2. delegate           监听动画运行的代理
3. duration           动画执行时间，默认为1秒
4. repeatCount        动画重复次数，默认为0
```

#### 1.1 `timingFunction` 参数

timingFunction 内置四种类型可选值：

```
kCAMediaTimingFunctionLinear          匀速动画
kCAMediaTimingFunctionEaseIn          加速动画
kCAMediaTimingFunctionEaseOut         减速动画
kCAMediaTimingFunctionEaseInEaseOut   先加速再减速
```
<strong style="color:green">Swift:</strong>

```swift
anim.timingFunction = CAMediaTimingFunction.init(name:"easeIn");
```
<strong style="color:green">Object-C:</strong>

```objc
elem.timingFunction = [CAMediaTimingFunction functionWithName:@"easeInEaseOut"];
```

#### 1.2 `delegate` 参数

设置动画代理`delegate`可以监听动画开始和结束动作：

<strong style="color:green">Swift:</strong>

```swift
anim.delegate = self;

//---------- 代码分割线 ----------

override func animationDidStart(anim: CAAnimation!) {
    // animation start
}

override func animationDidStop(anim: CAAnimation!, finished flag: Bool) {
    // animatoin stop
}

```
<strong style="color:green">Object-C:</strong>

```objc

// 设置动画代理
anim.delegate = self;

//---------- 代码分割线 ----------

// 动画开始
-(void) animationDidStart:(CAAnimation *)anim{
    NSLog(@"animation start");
}

// 动画结束
-(void) animationDidStop:(CAAnimation *)anim finished:(BOOL)flag{
        NSLog(@"animation stop");
}

```


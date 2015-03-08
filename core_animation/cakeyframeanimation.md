## CAKeyframeAnimation  帧动画

使用`CABasicAnimattion`有个缺点，只能从一个值过渡到另一个值，如果需要设置多个值，例如一个元素要从A运动到B再移动到C，那么这种方式采用`CAKeyframeAnimatoin`更适合。

### `CAKeyframeAnimation`几个重要属性：

**1. path**

这是一个`CGPathRef ` 对象，默认是空的，当我们创建好`CAKeyframeAnimation`的实例的时候，可以通过制定一个自己定义的path来让某一个物体按照这个路径进行动画。这个值默认是nil  当其被设定的时候  values这个属性就被覆盖。

```objc
CGMutablePathRef path = CGPathCreateMutable();
CGPathMoveToPoint(path,NULL,100,100);

//下面5行添加5条直线的路径到path中
CGPathAddLineToPoint(path, NULL, 200, 100);
CGPathAddLineToPoint(path, NULL, 200, 200);
CGPathAddLineToPoint(path, NULL, 100, 200);
CGPathAddLineToPoint(path, NULL, 100, 100);

CAKeyframeAnimation *anim = [CAKeyframeAnimation animationWithKeyPath:@"position"];
anim.duration = 2;
anim.path = path;
anim.fillMode = kCAFillModeForwards;
anim.removedOnCompletion = NO;

// 注意释放内存
CFRelease(path);
```


**2. values**

一个数组，提供了一组关键帧的值，当使用path的时候 values的值自动被忽略。

```objc
CGPoint p1 = CGPointMake(100, 100);
CGPoint p2 = CGPointMake(200, 100);
CGPoint p3 = CGPointMake(200, 200);
CGPoint p4 = CGPointMake(100, 200);
CGPoint p5 = CGPointMake(100, 100);

NSArray *values = [NSArray arrayWithObjects:[NSValue valueWithCGPoint:p1],[NSValue valueWithCGPoint:p2],[NSValue valueWithCGPoint:p3],[NSValue valueWithCGPoint:p4],[NSValue valueWithCGPoint:p5], nil];

CAKeyframeAnimation *anim = [CAKeyframeAnimation animationWithKeyPath:@"position"];
anim.values = values;
anim.duration = 2;

anim.fillMode = kCAFillModeForwards;
anim.removedOnCompletion = NO;
```




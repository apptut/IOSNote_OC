## NSTimer使用



#### 1. 延迟动画一次

```objc
[NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(timeRepeat) userInfo:nil repeats:NO];

- (void) timeRepeat{
    // some code
}
```

#### 2. 重复执行代码

```objc
[NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(timeRepeat) userInfo:nil repeats:YES];
```
<span style="color:#db0424">注意：</span>将计数器的repeats设置为YES的时候，self的引用计数会加1。因此可能会导致self（即viewController）不能release，所以，必须在viewWillAppear的时候，将计数器timer停止，否则可能会导致内存泄露。

#### 3. 取消定时器

```objc
[timer invalidate];
timer = nil; // timer一定要赋值nil
```

#### 4. timer重复使用

先停止，然后再某种情况下再次开启运行timer，可以使用下面的方法：

```objc
[myTimer setFireDate:[NSDate distantFuture]];
```
然后就可以使用下面的方法再此开启这个timer了：

```objc
[myTimer setFireDate:[NSDate distantPast]];
```



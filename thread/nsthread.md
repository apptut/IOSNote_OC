## NSThread的使用

`NSThread`现在已很少有人使用，因为它需要手动管理线程的开启，关闭，状态，所以管理线程来说比较繁琐。但是用它创建多线程非常方便，虽然这样但还是不建议使用。

不过他还是有一些便捷的方法值得关注：

```objc
(NSThread *)currentThread; //获得当前线程对象

(void)sleepForTimeInterval:(NSTimeInterval)ti; //线程休眠

(NSThread *)mainThread; //获取主线程对象

(BOOL)isMainThread; + (BOOL)isMainThread; //当前线程是否主线程

(BOOL)isExecuting; //线程是否正在运行

(BOOL)isFinished; //线程是否已结束
```

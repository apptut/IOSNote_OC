## NSOperation的使用

`NSOperation`实质是使用`GCD`实现的一套`Objective-C` API，方便程序员使用。是面向对象的线程技术，提供了一些在GCD中不容易实现的操作，例如：限制最大并发数量，操作之间的依赖关系等。

苹果官网推荐使用`NSOperation`和`NSOperationQueue`来操作多线程，而不是直接使用`GCD`，因为直接使用`GCD`还是有可能对线程不当操作导致线程不安全。而`且NSOperation`可以设置最大线程并发数,这样就能避免`GCD`开启N多线程对CPU的无线消耗，让程序员对资源可控：

```objc
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
// 设置最多开启2个子线程
[queue setMaxConcurrentOperationCount:2];

```

`NSOperationQueue`提供两种队列：

1. 主队列
2. 自定义队列

主队列表示在主线程中执行，自定义队列表示在后台执行。处理队列任务需要继承`NSOperation`类，苹果提供两个类来处理任务：

1. NSInvocationOperation
2. NSBlockOperation

```objc
// 子线程中执行
NSOperationQueue *queue = [[NSOperationQueue alloc] init];

// 自定义队列，在子线程中执行
[queue addOperationWithBlock:^{
	// 子线程执行
}];

// 主线程中执行任务
[[NSOperationQueue mainQueue] addOperationWithBlock:^{
    // some code
}];

```

需要注意的是，`NSOperationQueue` 执行多线程是并发执行的，也就意味着队列里的任务默认不是按顺序执行的（并行队列），如果要解决这个问题，那么需要指定线程依赖（addDependency:）：

```objc
NSOperationQueue *queue = [[NSOperationQueue alloc] init];

NSBlockOperation *task1 = [NSBlockOperation blockOperationWithBlock:^{
	// 任务一
}];

NSBlockOperation *task2 = [NSBlockOperation blockOperationWithBlock:^{
    // 任务二
}];

// 先执行任务1，再执行任务2
[task2 addDependency:task1];

// 队列里添加两个任务
[queue addOperation:task1];
[queue addOperation:task2];

```


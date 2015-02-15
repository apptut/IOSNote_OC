## GCD的使用

`GCD`全称是：Grand Central Diapatch，如果要翻译为中文，可以说成：大中央调度。

`GCD`是苹果IOS4.0 推出的多线程技术，使用底层C语言接口开发的API,使用Block定义任务，使用方便快捷。并且提供了更多线程操作的函数，以及底层接口。

`GCD`实质就是把要操作的任务放到队列中去执行。队列负责使用多线程来执行队列中的任务。任务都是用`Block`来定义的。

`GCD`中的方法都是以`dispatch_*`开头。

### 1 创建队列

`GCD`中常用的队列方式：

1. 串型队列
2. 并行队列
3. 全局队列

#### 1.1 串行队列

使用串型队列时，队列会新建一个(仅一个)子线程，然后使里面的任务都在这一个线程中按顺序执行，先添加到队列中的任务会先执行。

```objc
// 创建一个串行队列
dispatch_queue_t  queue1 = dispatch_queue_create("queue 1", DISPATCH_QUEUE_SERIAL);

// 执行一个异步线程任务
dispatch_async(queue1, ^{
	// 在同一个子线程中第一步执行的代码
        NSLog(@"当前线程：%@", [NSThread currentThread]);
});

// 再添加一个任务
dispatch_async(queue1, ^{
   // 在同一个子线程中第二部执行的顺序
   NSLog(@"当前线程：%@", [NSThread currentThread]);
});
```
串行队列的好处在于，只会开一个子线程，并且里面的若干任务是有序执行的；而并行任务无法做到这一点。

同样在串行队列里可以执行同步的任务：

```objc
// 创建一个串行队列
dispatch_queue_t  queue1 = dispatch_queue_create("queue 1", DISPATCH_QUEUE_SERIAL);

dispatch_sync(queue1, ^{
	// 同步任务，一般不使用
});
```
串行队列里执行同步任务，该同步任务不会创建新的子线程，而是在当前线程中(可能是主线程)执行，所以很少有人在串行列表中使用同步线程。

#### 1.2 并行队列

使用并行队列时，队列会新建若干个子线程，然后在这些线程中并行执行队列中的任务，并行队列就像是赛跑，其中任务的执行顺序是不可控制的，并且开启多少个子线程也是无法控制的：

```objc
// 创建一个并行队列
dispatch_queue_t  queue2 = dispatch_queue_create("queue 1", DISPATCH_QUEUE_CONCURRENT);

// 执行一个异步线程任务
dispatch_async(queue2, ^{
    // 在同一个子线程中第一步执行的代码
    NSLog(@"当前线程：%@", [NSThread currentThread]);
});

dispatch_async(queue2, ^{
    // 在同一个子线程中第二部执行的顺序
    NSLog(@"当前线程：%@", [NSThread currentThread]);
});
```

`dispatch_queue_create` 参数解释：

```objc
dispatch_queue_create(const char *label, dispatch_queue_attr_t attr);

label   // 当前线程tag，C语言字符串
attr    // 线程类型分为 DISPATCH_QUEUE_SERIAL(串行) 和 DISPATCH_QUEUE_CONCURRENT (并行)

```

#### 1.3 全局队列

苹果为方便多线程设计，自己提供一个全局队列，供所有App共同使用的队列：全局队列。

全局队列不需要创建，直接使用GET方法就可调用；全局队列与并行队列的执行效果是相同的；而且全局队列本身没有名称，因此无法准确确认队列。

```objc
// 获取一个全局队列
dispatch_queue_t  queue3 = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

// 第一个参数：线程执行的优先级
// 第二个参数：flag 苹果官网让传入0，供将来使用(参考官方文档使用手册)
```

#### 1.4 线程操作注意

IOS中所有UI更新都是要在主线程中执行的，获取当前主线程可以通过以下方式：

```objc
// 获取主线程操作
dispatch_queue_t queue2 = dispatch_get_main_queue();

// 主线程中执行一个异步任务
dispatch_async(queue2, ^{
    // 在同一个子线程中第二部执行的顺序
    NSLog(@"当前线程：%@", [NSThread currentThread]);
});
```

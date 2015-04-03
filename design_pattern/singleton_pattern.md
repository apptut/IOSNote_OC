## 单例设计模式

在ios中设计一个单例模式最主要的是如何考虑多线程中线程安全问题，幸运的是ios提供了保证线程安全的方法，因此设计一个单例变得非常容易：

```objc
//  Singleton.h
//  Singleton

#import <Foundation/Foundation.h>

@interface Singleton : NSObject

+ (Singleton *)sharedInstance;

@end
```

```objc
//  Singleton.m
//  Singleton

#import "Singleton.h"

@implementation Singleton

+ (Singleton *)sharedInstance{
    static Singleton *singleton;
    static dispatch_once_t token; // 苹果官方建议使用dispatch_once_t定义为静态全局变量
    dispatch_once(&token,^{
        singleton = [[Singleton alloc]init];
    });
    return singleton;
}

@end
```
`dispatch_once` 保证了内部的代码块只会被执行一次，而且是线程安全的。

使用：

```objc
Singleton *singleton = [Singleton sharedInstance];
```

#### ios中使用单例模式常用类

// todo


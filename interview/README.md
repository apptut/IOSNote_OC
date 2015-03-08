# 面试问题

1、import 跟include 又什么区别，@class呢, ＃import<> 跟 import”"又什么区别？

import是Objective-C导入头文件的关键字，#include是C/C++导入头文件的关键字,使用#import头文件会自动只导入一次，不会重复导入，相当于#include和#pragma once；@class告诉编译器某个类的声明，当执行时，才去查看类的实现文件，可以解决头文件的相互包含；#import<>用来包含系统的头文件，#import””用来包含用户头文件。

2、属性readwrite，readonly，assign，retain，copy，nonatomic 各是什么作用，在那种情况下用？

readwrite 是可读可写特性；需要生成getter方法和setter方法时

readonly 是只读特性  只会生成getter方法 不会生成setter方法 ;不希望属性在类外改变

assign 是赋值特性，setter方法将传入参数赋值给实例变量；仅设置变量时；

retain 表示持有特性，setter方法将传入参数先保留，再赋值，传入参数的retaincount会+1;

copy 表示拷贝特性，setter方法将传入对象复制一份；需要完全一份新的变量时。

nonatomic 非原子操作，决定编译器生成的setter getter是否是原子操作，atomic表示多线程安全，一般使用nonatomic

3、 自定义一个构造函数

4、常见的object-c的数据类型有那些， 和C的基本数据类型有什么区别？如：NSInteger和int

object-c的数据类型有NSString，NSNumber，NSArray，NSMutableArray，NSData等等，这些都是class，创建后便是对象，而C语言的基本数据类型int，只是一定字节的内存空间，用于存放数值；NSInteger是基本数据类型，并不是NSNumber的子类，当然也不是NSObject的子类。NSInteger是基本数据类型Int或者Long的别名（NSInteger的定义typedef long NSInteger），它的区别在于，NSInteger会根据系统是32位还是64位来决定是本身是int还是Long。

5、What are KVO and KVC?

答案：kvc:键 - 值编码是一种间接访问对象的属性使用字符串来标识属性，而不是通过调用存取方法，直接或通过实例变量访问的机制。
很多情况下可以简化程序代码。apple文档其实给了一个很好的例子。
kvo:键值观察机制，他提供了观察某一属性变化的方法，极大的简化了代码。
具体用看到嗯哼用到过的一个地方是对于按钮点击变化状态的的监控。

6、What is purpose of delegates?
代理的作用？
答案：代理的目的是改变或传递控制链。允许一个类在某些特定时刻通知到其他类，而不需要获取到那些类的指针。可以减少框架复杂度。
另外一点，代理可以理解为java中的回调监听机制的一种类似。

7、coredata有哪几种持久化存储机制？
coredata提供以下几种存储机制：XML（iOS系统不支持）,自动存储,SQLite,内存存储。

8、您是否做过异步的网络处理和通讯方面的工作？如果有，能具体介绍一些实现策略么？
9、多线程
10、ios导航控制器切换方式以及区别
11、数据库（使用以及优化原则）
12、coredata使用
13、Core开头的系列的内容。是否使用过CoreAnimation和CoreGraphics。UI框架和CA，CG框架的联系是什么？分别用CA和CG做过些什么动画或者图像上的内容。（有需要的话还可以涉及Quartz的一些内容）
14、是否使用过CoreText或者CoreImage等？如果使用过，请谈谈你使用CoreText或者CoreImage的体验。
15、对于Objective-C，你认为它最大的优点和最大的不足是什么？对于不足之处，现在有没有可用的方法绕过这些不足来实现需求。如果可以的话，你有没有考虑或者实践过重新实现OC的一些功能，如果有，具体会如何做？

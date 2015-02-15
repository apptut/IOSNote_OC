## IOS多线程

IOS中有三种方式使用多线程技术：

1. NSThread
2. NSOperation / NSOperationQueue
3. GCD (Grand Central Dispatch)

在App中对于I/O处理耗时的操作，建议使用多线程技术，例如网络请求，数据库操作，文件读写等。




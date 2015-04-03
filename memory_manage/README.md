## 内存管理

虽然有了ARC，但是如果直接使用一些C库，例如`CoreFandation`、`AddressBook`框架，也需要手动释放内存:

一般的，当一个函数名字中包含了create、retain、copy、new等字眼，那么使用完这些函数需要手动release这些函数


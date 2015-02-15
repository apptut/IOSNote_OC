## id 和 instanceype

XCode内置一些便捷代码生成关键字，帮助我们快速自动生成代码，虽然没有`.idea`那么强大，但是还是有一定作用。

#### 关键字修饰类型差别：
```objc
copy NSString
strong 一般对象（除NString除外）
weak UI控件 || Protocol
assgin 基本数据类型(int float enum struct)
```
#### instancetype 和 id差别

IOS7新增 ```instancetype``` 用来替代 ```id```在函数返回值得类型：

```objc
// 旧的方式
- (id) init{
    // some code
}

// 新的方式
- (instancetype) init{
    // some code
}

```

之所以使用 ```instancetype``` 替代id,是因为id属于万能指针，返回对象能赋值给任何其他对象，但是当一个对象(id类型) 赋值给一个新对象变量，并且这俩对象类型不一样，但是编译器本身不会报错，这样容易造成隐性错误很难被发现，因此使用 ```instancetype``` 来代替，如果发现返回对象与新对象不匹配时，编译器会发出对象类型不匹配的警告来提示程序员。

```objc
NSString *str = [[NSDate alloc] init]; // 对象类型匹配，但是但是编译器不报错

```

但是  ```instancetype```  不能完全替换 ```id``` ,它只能用在返回类型上，函数形参还是只能使用 ```id``` 不能使用  ```instancetype```

```objc
- (instancetype) initWithDog: (id) dog{
    // some code
}

```

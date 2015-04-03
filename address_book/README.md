## 通讯录操作

使用手机通讯录需要用到`AdressBook.framework`,并且需要申请访问权限：

```objc
ABAddressBookRef book = ABAddressBookCreateWithOptions(NULL, NULL);
ABAddressBookRequestAccessWithCompletion(book, ^(bool granted, CFErrorRef error) {
    if (granted == YES) {
        // 授权成功
    }
});

// 释放内存
CFRelease(book);

```

#### 1. 简单获取通讯录用户内容

```objc
// 创建一个通讯录实例
ABAddressBookRef book = ABAddressBookCreateWithOptions(NULL, NULL);

// 获取所有用户数据
CFArrayRef pepple = ABAddressBookCopyArrayOfAllPeople(book);

// 获取通讯录总条目数
CFIndex count = CFArrayGetCount(pepple);
for (CFIndex i =0; i<count; i++) {
    // 获取单条数据

    ABRecordRef item = CFArrayGetValueAtIndex(pepple, i);

    // 所有字段以kAB开头，更多属性见ABAddressBook/Abperson.h文件
    CFStringRef firstName = ABRecordCopyValue(item, kABPersonFirstNameProperty);

    NSLog(@"%@",firstName);

    // 释放内存
    CFRelease(firstName);

}
CFRelease(pepple);
CFRelease(book);

```
纯C语言函数写起来很不爽，可以使用`__bridge`把CF对象转换为OC对象，当然只能转换一部分对象，桥接的对象变为OC对象，操作更方便，而且不需要手动释放内存

```objc
- (void) getBooks{

    if(kABAuthorizationStatusAuthorized == ABAddressBookGetAuthorizationStatus()){
        // 检测授权状态
    }

    // OC不存在此对象，所以不能桥接
    ABAddressBookRef book = ABAddressBookCreateWithOptions(NULL, NULL);

    // 数组桥接为OC数组
    NSArray *pepple = (__bridge NSArray *)(ABAddressBookCopyArrayOfAllPeople(book));

    // 获取通讯录总条目数
    NSInteger count = pepple.count;

    for (int i =0; i<count; i++) {
        // 获取单条数据
        ABRecordRef item = (__bridge ABRecordRef)([pepple objectAtIndex:i]);

        // 所有字段以kAB开头
        NSString *firstName = (__bridge NSString *)(ABRecordCopyValue(item, kABPersonFirstNameProperty));

        NSLog(@"%@",firstName);

    }
    CFRelease(book);


    // 都是C语言函数，需要额外注意的是内存释放问题
}

```

都是C语言函数，需要额外注意的是内存释放问题。

#### 2. 检测通讯录授权状态

ios权限管理，当应用被拒绝或者同意一次后，ios被记住用户操作，之后就不在提示授权弹窗，因此有时候需要检测应用当前是否授权。

```objc
if(kABAuthorizationStatusAuthorized == ABAddressBookGetAuthorizationStatus()){
    // 检测授权状态
}
```

#### 3. 编辑通讯录

```objc
// OC不存在此对象，所以不能桥接
ABAddressBookRef book = ABAddressBookCreateWithOptions(NULL, NULL);

// 数组桥接为OC数组
NSArray *pepple = (__bridge NSArray *)(ABAddressBookCopyArrayOfAllPeople(book));

// 编辑数组中第0条信息
ABRecordRef one = (__bridge ABRecordRef)(pepple[0]);
CFStringRef value = (__bridge CFStringRef)@"找四";

ABRecordSetValue(one, kABPersonLastNameProperty, value, NULL);

// 最终保存
ABAddressBookSave(book, NULL);
```

#### 4. 添加通讯录

```objc
if(kABAuthorizationStatusAuthorized == ABAddressBookGetAuthorizationStatus()){
    // 检测授权状态
}


ABRecordRef newPerson = ABPersonCreate();

// 添加姓名
ABRecordSetValue(newPerson, kABPersonFirstNameProperty, @"张", NULL);
ABRecordSetValue(newPerson, kABPersonLastNameProperty, @"三", NULL);

// 添加电话号码，电话号码是由多个子字符串组成
ABMultiValueRef arr = ABMultiValueCreateMutable(kABMultiStringPropertyType);
ABMultiValueAddValueAndLabel(arr, @"18800118899", kABPersonPhoneMainLabel, NULL);
ABRecordSetValue(newPerson, kABPersonPhoneProperty, arr, NULL);

// 最终保存
ABAddressBookRef book = ABAddressBookCreateWithOptions(NULL, NULL);
ABAddressBookAddRecord(book, newPerson, NULL);

ABAddressBookSave(book, NULL);

CFRelease(arr);
CFRelease(newPerson);
CFRelease(book);

```

#### 5. 删除图片

```objc
// 使用这个函数
ABRecordRemoveValue(<#ABRecordRef record#>, <#ABPropertyID property#>, <#CFErrorRef *error#>)
```

## MKNetwork基本使用

**Get请求：**

```objc
// 1. 初始化engine
MKNetworkEngine *engine = [[MKNetworkEngine alloc] initWithHostName:@"apptut.com"];
// 2. 添加path
MKNetworkOperation *operation = [engine operationWithPath:@"/json.php"];

// 3. 添加请求回调
[operation addCompletionHandler:^(MKNetworkOperation *completedOperation) {
        // JSON转字典
        NSDictionary *dic = [completedOperation responseJSON];
    } errorHandler:^(MKNetworkOperation *completedOperation, NSError *error) {
        NSLog(@"出错啦");
}];

// 4. 添加到请求队列
[engine enqueueOperation:operation];
```

**Post 请求**



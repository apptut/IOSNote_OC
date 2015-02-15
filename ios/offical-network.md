# 官方内置网络

使用IOS内置的NSURLConnection发送请求，大致需要三个步骤：

1. 构造请求Url地址
2. 创建一个request请求
3. 发送请求

###### 1.1 GET请求

```objc
// 1. 初始化
NSURL *url = [NSURL URLWithString:@"http://m.taobao.com"];

// 2. 配置request请求
NSURLRequest *request = [[NSURLRequest alloc] initWithURL:url];

// 3. 发送异步网络请求
[NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue currentQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
 	// 请求结果回调
   	if(!connectionError){
   	// 使用内置的 NSJSONSerialization 序列化字符串，
    id jsonData = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];
    NSDictionary *jsonDict = (NSDictionary *)jsonData;

	// 打印数据
	NSLog(@"%@,%@",[jsonDict objectForKey:@"title"],[jsonDict objectForKey:@"info"]);

	// 使用内置的 NSJSONSerialization 解析json对象为dictionary
	id jsonData = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];
	NSDictionary *jsonDict = (NSDictionary *)jsonData;

	NSLog(@"%@,%@",[jsonDict objectForKey:@"title"],[jsonDict objectForKey:@"info"]);
}];

```
需要注意的是：

1. NSJSONSerialization IOS5.0以后才支持，之前版本无法使用此方法。
2. 网络请求不建议在主线程中使用，尽量使用异步请求，在子线程中获取网络数据，优化用户体验。


###### 1.2 发送POST请求

```objc
// 发送POST请求需要使用 NSMutableURLRequest
NSMutableURLRequest *postReq = [NSMutableURLRequest requestWithURL:url];

// 设置请求方式
[postReq setHTTPMethod:@"POST"];

// 添加请求的数据
[postReq setHTTPBody:[@"hello" dataUsingEncoding:NSUTF8StringEncoding]];

// 设置请求过期时间
[postReq setTimeoutInterval:20.0];

// 初始化一个线程
NSOperationQueue *queue = [[NSOperationQueue alloc]init];

[NSURLConnection sendAsynchronousRequest:postReq queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
        if (!connectionError) {
            // 操作结果
        }
    }];
```

## 文件下载

#### 1. 文件下载关键点

##### 1.1 http协议的`range`参数：

`range`参数用来指定网络数据包的其实范围大小。

```http
bytes=0-100             //表示下载数据包1~100的字节数
bytes=101-              //表示从数据包101~文件末尾的数据

bytes=-500              //下载最后500个字节
bytes=100-200,500-800   //可以指定多个范围，很少用
```

##### 1.2 IOS内置`NSMutableRequest`请求缓存策略：

ios发送网络请求默认的缓存策略是内存缓存，避免同一个url同时出现多次请求，但是这对文件分段下载不利，做文件下载时，如果使用`NSMutableRequest`，注意配置请求的缓存策略为：`NSURLRequestReloadIgnoringCacheData`

```objc
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://a.mp3"]cachePolicy:NSURLRequestReloadIgnoringCacheData timeoutInterval:26];
```
##### 1.3 只获取文件头信息

在开始下载前可能需要获取下载文件的总字节数,`content-length`，这个字段在在`http`的`response`头文件中已定义，因此只需要获取`response`头文件即可，使用如下方法

```objc
// ...
request.HTTPMethod = @"HEAD";
```


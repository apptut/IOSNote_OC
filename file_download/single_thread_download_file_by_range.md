## 文件分段下载原理

原理：主要利用http `range`函数把一个文件裁切为多个数据包段下载，然后再把下载的数据拼接为一整个文件。

实现步骤：

```
1. 获取整个文件大小
2. 利用range计算出文件下载范围
3. 保存分段数据
```

### 1. 获取文件大小

首先我们需要事先一个`getDownloadFileData`函数获取当前文件头文件大小，函数返回类型为`long long`类型，和`response`返回字节数类型一致，避免类型转换。

```objc
- (long long) getDownloadFileData:(NSURL *)url{
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url cachePolicy:0 timeoutInterval:26];

    // 只获取本次请求的头信息
    request.HTTPMethod = @"HEAD";

    NSURLResponse *response = nil;

    // 不需要处理错误
    //NSError *error = nil;

    // 发送同步接口获取当前数据头信息
    [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:NULL];

    if(response == nil){
        return 0;
    }

    return response.expectedContentLength;
}
```
### 2. 利用range计算出文件下载范围




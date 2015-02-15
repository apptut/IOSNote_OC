## UIWebView常用设置
当然在做一些复杂的项目时候并不是之前如此简单的使用UIWebView，可能我们需要对 `UIWebView` 更详细的操作，如，本地缓存数据，对 `JavaScript` 的支持、甚至通过网页调用本地Api接口实现等。

### 5.2.1 设置请求缓存策略

与其说对UIWebView设置缓存，还不如说是通过对 `NSURLRequest` 对象设置缓存策略：

```objc
NSURLRequest *request = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestReloadRevalidatingCacheData timeoutInterval:26.0];
```

`cachePolicy` 属于枚举类型，可以设置当前请求Url的不同缓存策略，源代码如下：

```objc
typedef NS_ENUM(NSUInteger, NSURLRequestCachePolicy)
{
	//NSURLRequest默认的缓存策略，使用Protocol协议定义
    NSURLRequestUseProtocolCachePolicy = 0,

	//忽略缓存直接从原始地址下载
    NSURLRequestReloadIgnoringLocalCacheData = 1,

    //忽略本地缓存以及服务器端缓存
    NSURLRequestReloadIgnoringLocalAndRemoteCacheData = 4, // Unimplemented

    //相当于1，1的等价类型，老版本
    NSURLRequestReloadIgnoringCacheData = NSURLRequestReloadIgnoringLocalCacheData,

	//如果本地有缓存，不管是否缓存过期，都本地缓存中加载数据；如果缓存不存在则从服务器获取
    NSURLRequestReturnCacheDataElseLoad = 2,

    //若果本地有缓存，不换是否缓存过期，都从缓存获取数据；如果本地缓存数据不存在，那么加载失败，此种策略适合做离线应用
    NSURLRequestReturnCacheDataDontLoad = 3,

	//验证本地数据和服务器数据是否相同，不同则重新加载数据
    NSURLRequestReloadRevalidatingCacheData = 5, // Unimplemented
};
```
建议英语好的童鞋直接看IOS官方文档以及源码，注释相当详细。

`timeoutInterval` 用来设置请求的超时时间，刚才的代码可以调整为：

```objc
// 1. 封装URL数据
NSURL *url = [NSURL URLWithString:@"http://m.taobao.com"];

// 2. 包装request对象，本地缓存数据验证策略，26秒超时
NSURLRequest *request = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestReloadRevalidatingCacheData timeoutInterval:26.0];

// 3. 调用webview loadRequest方法加载Url
[self.webView loadRequest:request];

```

### 5.2.2 UIWebView设置Cookie

IOS请求使用Cookie很简单，封装一个cookie然后塞到请求header头即可

```objc
NSDictionary *properties = @{
                                 NSHTTPCookieName : @"tudou",
                                 NSHTTPCookieValue : @"12345",
                                 NSHTTPCookieDomain : @".taobao.com",
                                 NSHTTPCookiePath: @"/",
                                 NSHTTPCookieExpires:[[NSDate date] dateByAddingTimeInterval:(24*3600)]
                                 };
// 生成cookie对象
NSHTTPCookie *cookies = [NSHTTPCookie cookieWithProperties:properties];

// 把生成的cookie存到NSArray中
NSArray *cookieArr = @[cookies];

// 获取组装后cookie字典
NSDictionary *cookieDict = [NSHTTPCookie requestHeaderFieldsWithCookies:cookieArr];

// 1. 封装URL数据
NSURL *url = [NSURL URLWithString:@"http://m.taobao.com"];

// 2. 包装request对象
NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];

// 3. 添加Cookie到头文件
[request setAllHTTPHeaderFields:cookieDict];

// 4. 发起请求
[self.webView loadRequest:request];

```
### 5.2.3 自定义UIWebView 每个请求 header头

在开发中我们可能会遇到给UIWebView每一个url请求定制header头信息或者Cookie(cookie也是添加在header中，这时候我们可以修改UIWebView代理方法)：

```objc
-(BOOL) webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType{

NSString *urlString = [request.URL.absoluteURL absoluteString];
// 注意页面有iframe时候，会有空白页请求
if([urlString isEqualToString:@"about:blank"] || [[request allHTTPHeaderFields] objectForKey:@"xclient"]){
        return YES;
    }
// 开启异步线程重新加载
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0), ^{
   dispatch_async(dispatch_get_main_queue(), ^{

   		NSMutableURLRequest* newRequest = [request mutableCopy];
       [newRequest setValue:@"haha" forHTTPHeaderField:@"xclient"];

       // reload the request
       [webView loadRequest:newRequest];
   });
});

return NO;
}

```objc

### 5.2.4 UIWebView与页面JS交互

使用webview加载网页避免不了需要页面和手机Native之间的交互，例如点击页面按钮打电话、拍照、发短信等等，因为页面本身很难实现这些Native功能，那么需要让页面和本地Native之间进行接口交流，还好IOS也有相应的功能。

#### 5.2.4.1 OC调用Javascript

JavaScript是制作网页的脚本语言，通过OC代码可以让页面主动做一些事情，比如获取当前页面的title显示到native的导航空间上，又或者主动调用页面一个Js函数，让页面去做一些事情，OC提供了如下方法：

```objc
- (NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script
```
比如，当网页记载完毕时获取页面的标题（获取网页标题，需要网页加载完毕）：

```objc
@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIWebView *webView;
@property (weak, nonatomic) IBOutlet UINavigationBar *navigation;
@end

// 省略url加载代码

// UIWebView当前Url加载完毕调用函数
- (void)webViewDidFinishLoad:(UIWebView *)webView{

    NSString *pageTitle = [self.webView stringByEvaluatingJavaScriptFromString:@"document.title"];

    // 动态设置Native控件NavigatoinBar标题
    self.navigation.topItem.title = pageTitle;

}

```
如果想调用页面js函数：

```objc
[self.webView stringByEvaluatingJavaScriptFromString:@"test();"];

```

如果页面并不存在 `test()` 函数，执行上述方法也不会有什么问题的，异常警告都没有。不过此函数官网文档也给出了注意：

1. JS代码占用的内存使用量不能超过10M（JS本身很少有这么，除非有内存溢出，或不当操作）
2. 页面响应时间不能超过10秒，不然也会有问题，因为上述函数是在主线程中执行的，会阻塞线程，因此耗时操作需要考虑放在子线程中异步执行。

#### 5.2.4.2 Javascript调用OC方法

JS

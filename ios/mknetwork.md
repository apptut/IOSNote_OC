## MKNetwork使用

对于IOS来说，优秀的第三网络框架着实不少，`AFNetworking`、`ASIHTTPRequest`、还有就是今天说的`MKNetwork`，自己开发网络请求部分之所以选择这个框架，是因为这个框架集前两个框架之所长，而且`ASIHTTPRequest`当前也停止了更新，并且不支持`ARC`开发模式。

相比之下，我选择`MKNetwork`，也并非是出于框架之间寻根究底的比较谁好谁差，一部分原因也是由于`MKNetwork`的口碑，再加上自己的使用习惯，仅此而已。因为`AFNetworking`和`ASIHTTPRequest`也不错。

`MKNetwork`代码地址：<https://github.com/MugunthKumar/MKNetworkKit>

官方使用文档：<http://blog.mugunthkumar.com/products/ios-framework-introducing-mknetworkkit/>

另外附一张网上疯传的这三个网络框架的功能对比图片：

|网络框架对比   | AFNetworking                          | MKNetworkKit          | ASIHTTPRequest   |
|:-------------:|:-------------------------------------:|:---------------------:|:---------:|
|更新情况       | 维护和使用者相对多                    | 维护和使用者相对少    | 停止更新  |
|支持iOS和OSX   | 是                                    | 是                    | 是        |
|ARC            | 是                                    | 是                    | 否        |
|断点续传       | 否，可通过AFDownloadRequestOperation  | 是                    | 是        |
|同步异步请求   | 只支持异步                            | 否                    | 是        |
|图片缓存到本地 | 图片缓存到本地                        | 否                    | 否        |
|图片缓存到内存 | 是                                    | 是                    | 否        |
|后台下载       | 是                                    | 是                    | 是        |
|下载进度       | 否，可通过AFDownloadRequestOperation  | 是                    | 是        |
|缓存离线请求   | 否，通过SDURLCache或AFCache           | 是                    | 否        |
|JSON、XML      | 是                                    | 是                    | 否        |



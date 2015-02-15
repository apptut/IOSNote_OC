## IOS 文件沙盒

IOS采用沙盒机制来管理每一个App应用，也就是说每个应用除了能访问自己应用内的文件目录外无法访问别的手机别的文件目录，不能实现`Android`应用访问手机`SDCard`功能。

每个App目录下有三个文件目录

```
Hello.app/
         Documents
         Tmp
         Library/
                Caches
                Preferences

```
`Documents`目录只能用来存放应用本身的用户相关的一些信息，并且`ICloud`会同步此目录

`Tmp`应用运行的临时数据目录，`ICloud`不会同步该目录的数据，并且如果当前应用退出，此目录会被回收，此目录可以用来存放App运行时的临时文件数据。

```objc
//缓存目录
NSCachesDirectory
```

`Library`下面包括`Caches`和`Preference`目录，`Caches`目录用来存放应用存放的大数据文件（比如网络下载的图片，视频，数据等），`Preference`用来存放应用常用的设置的文件，并且`Preference`目录也会被`ICloud`同步数据。

---
*注意：*

`Documents`目录下千万不要放应用从网络下载的数据，不然审核肯定通不过的，切记切记！



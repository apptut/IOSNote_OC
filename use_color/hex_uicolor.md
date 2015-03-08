## 16进制颜色换算

因为自己习惯使用`#RRGGBB`十六进制来颜色值，找到一段宏：

```c
#define UIColorFromRGB(rgbValue) [UIColor colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0 green:((float)((rgbValue & 0xFF00) >> 8))/255.0 blue:((float)(rgbValue & 0xFF))/255.0 alpha:1.0]

//RGB color macro with alpha
#define UIColorFromRGBWithAlpha(rgbValue,a) [UIColor colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0 green:((float)((rgbValue & 0xFF00) >> 8))/255.0 blue:((float)(rgbValue & 0xFF))/255.0 alpha:a]
```

使用方式：

```objc
view.backgroundColor = UIColorFromRGB(0xf0f0f0);
view.backgroundColor = UIColorFromRGB(0xf0f0f0,0.5);
```

原文地址：

<http://cocoamatic.blogspot.com/2010/07/uicolor-macro-with-hex-values.html>

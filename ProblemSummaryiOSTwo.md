## 设置一个单个角的弧
```OC
UIBezierPath *maskPath = [UIBezierPath
    bezierPathWithRoundedRect:self.backgroundImageView.bounds byRoundingCorners:(UIRectCornerBottomLeft | UIRectCornerBottomRight)
    cornerRadii:CGSizeMake(20, 20)
];
CAShapeLayer *maskLayer = [CAShapeLayer layer];
maskLayer.frame = self.bounds;
maskLayer.path = maskPath.CGPath;
self.backgroundImageView.layer.mask = maskLayer;
```

##  [ViewController respondsToSelector:]: message sent to deallocated instance

出现这个问题要把ViewController中子控件delegate为ViewController的在dealloc方法中设置为nil。（我是在WKWebView中遇到，然后只有在IOS8才有问题）

```
-(void)dealloc{
  self.delegateView.delegate = nil;
}
```
## 全局设置UIBarButtonItem时排除其他地方的Item

```
[[UIBarButtonItem appearanceWhenContainedIn:[UINavigationController class], nil] setTitleTextAttributes:@{NSFontAttributeName:[UIFont systemFontOfSize:16],NSForegroundColorAttributeName:[UIColor whiteColor]} forState:UIControlStateDisabled];
```
## Keywindow上添加控件和原生AlertView冲突

当Keywindow上还有自定义的控件时不能调用远程AlertView会造成闪退

## UILabel显示问题

如果UILabel的Size不是整数时，设置背景颜色会出现边缘黑线。

## 用XIB创建的View在iPhone5s iOS8上显示错乱

```
    UIXIBView *XIBView = [UIXIBView XIBView];
    XIBView.frame = CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, 216);
    [XIBView layoutIfNeeded];
```


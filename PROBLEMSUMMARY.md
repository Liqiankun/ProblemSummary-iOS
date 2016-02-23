![](https://github.com/Liqiankun/ProblemSummary/raw/master/ProblemSummary.png)<br>
#怎么去掉TableView分割线左边上的空白（必须两个同时实现）
```oc
    //实现tableViewSeparatorLine的长度是屏幕的长度
    if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {
        
        [self.tableView setSeparatorInset:UIEdgeInsetsZero];
        
    }
    if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {
        
        [self.tableView setLayoutMargins:UIEdgeInsetsZero];
        
    }
    
 //实现tableViewSeparatorLine的长度是屏幕的长度
- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath

{
    if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {
        
        [cell setSeparatorInset:UIEdgeInsetsZero];
        
    }
    if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        
        [cell setLayoutMargins:UIEdgeInsetsZero];
    }
    
}
```
```oc
    cell.separatorInset = UIEdgeInsetsZero;
    if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        cell.layoutMargins = UIEdgeInsetsZero;
    }
```
#隐藏StatusBar
```oc
- (BOOL)prefersStatusBarHidden {
    return YES;
}
```
#在有输入框的时候如果不能正常显示可以用
```oc
self.automaticallyAdjustsScrollViewInsets = NO;
```
#UIImage转NSData的方法
```oc
//听说还有exif信息可以先取出然后再使用
	UIImageJPEGRepresentation, UIImagePNGRepresentation
```
#return键在没有文字时不能点击，可以实现当输入框中没有文字时return键为灰色不能点击
```oc
	self.textView.enablesReturnKeyAutomatically = YES;
```
#图片去系统渲染
```oc
  [[UIImage imageNamed:name] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
```
#去掉navigationBar下的黑线
```oc
if ([navigationController.navigationBar respondsToSelector:@selector( setBackgroundImage:forBarMetrics:)]){
        NSArray *list = navigationController.navigationBar.subviews;
        for (id obj in list) {
            if ([obj isKindOfClass:[UIImageView class]]) {
                UIImageView *imageView=(UIImageView *)obj;
                NSArray *list2=imageView.subviews;
                for (id obj2 in list2) {
                    if ([obj2 isKindOfClass:[UIImageView class]]) {
                        UIImageView *imageView2=(UIImageView *)obj2;
                        imageView2.hidden=YES;
                    }
                }
            }
        }
    }
```

```oc
[self.navigationController.navigationBar setShadowImage:[UIImage new]];
```

#UIImageView问题
当有UIImageView有动画时不要添加手势事件

#根据日期得到星期
```oc
-(NSString*)getgetWeekDay:(NSString*)dateString
{
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd hh:mm"];
    dateFormatter.timeZone = [NSTimeZone localTimeZone];
    NSDate *date = [dateFormatter dateFromString:dateString];

    NSArray *weeks=@[[NSNull null],@"星期天",@"星期一",@"星期二",@"星期三",@"星期四",@"星期五",@"星期六"];
    NSCalendar *calendar=[[NSCalendar alloc]initWithCalendarIdentifier:NSGregorianCalendar];
    NSTimeZone *timeZone=[[NSTimeZone alloc]initWithName:@"Asia/Beijing"];
    [calendar setTimeZone:timeZone];
    NSCalendarUnit calendarUnit=NSWeekdayCalendarUnit;
    NSDateComponents *components=[calendar components:calendarUnit fromDate:date];
    
    NSString *finalDate = [NSString stringWithFormat:@"%@ (%@)",dateString,[weeks objectAtIndex:components.weekday]];
        return finalDate;
	
}
```
#添加PCH文件
先创建`pronuciationApp-Prefix.pch`文件，在`BuildSetting`里`Prefix Header`里添加`$(SRCROOT)/pronuciationApp/pronuciationApp-Prefix.pch`

#时间格式
在处理时间时一定要和后端协调好时间格式。

#主色调
在`PCH`里添加App的主色调，方便开发调用。

#项目文件结构重构
如果你在整理项目文件的重构提交时前往不要`git checkout -- projectName.xcodeproj/project.pbxproj`要不就毁了！

#第三方库的使用
在开发中使用第三方库如果库更新很频繁一定要有自己的方法，要不很难搞。比如[AFNetworking](https://github.com/AFNetworking/AFNetworking)最近从2.0升级到了3.0，如果你没各类都用了怎么改，改死你。

#iOS系统适配问题
如果iOS8的API在iOS9上有更新。
```OC
#ifdef __IPHONE_9_0
-(void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info
{
	//如果是IOS9之后的系统执行这个方法
}
#else
-(void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary *)info
{
	//如果IOS9之前的系统执行这个方法
}
#endif
```
#下载模拟器慢怎么办
如果在Xcode下载很慢（肯定很慢），可以在百度云或者CocoaChina下载，然后放在Xcode里。操作为`sudo -s`创建权限，`mkdir -p  /Library/Developer/CoreSimulator/Profiles/Runtimes/`创建目录，`cp -R  {模拟器路径}  /Library/Developer/CoreSimulator/Profiles/Runtimes/{模拟器}`复制。具体参考[下载模拟器](http://blog.csdn.net/zhangao0086/article/details/38491271)

#关于开发中的`NSLog`
在开发中我们经常回会用到`NSLog`，但是`NSLog`是比较耗费性能的，我们要在上线时去掉。但是软件开发过程中我们还需要测试。
```oc
#ifdef DEBUG // 处于开发阶段
#define DLLog(...) NSLog(__VA_ARGS__)
#else // 处于发布阶段
#define DLLog(...)
#endif
```
这样在`DEBUG`时`DLLog`是有效的，在`REALSE`版本中`DLLog`就是无效的。

#关于测试
上线之前一定要测试，完整的测试。包括之前的功能，`登录`和`未登录`状态下测试。

#文本设置行间距
```oc
  NSMutableAttributedString *attributedString = [[NSMutableAttributedString alloc] initWithString: String];
            NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc] init];
            
  [paragraphStyle setLineSpacing:5];//调整行间距
  [attributedString addAttribute:NSParagraphStyleAttributeName value:paragraphStyle range:NSMakeRange(0, String.length)];
```赵成
#版本适配测
如果你的项目支持iOS8和iOS9,测试一定要在Xcode6和Xcode7同时测试你的应用，然后是真机。

#单例的创建
```oc
+(id)manager
{
    static ClassName *class = nil;
    static dispatch_once_t classManager;
    //dispath_once:Executes a block object once and only once for the lifetime of an application
    dispatch_once(&classManager, ^{
        if (!class) {
            class = [[ClassName alloc] init];
        }
    });
    return class;
}
```
#类名之前怎么加默认前缀
在`Target`->右边栏`Class Prefix`,添加自己的前缀即可。

#照片大小处理
在上传图片时速度很慢造成体验很差，一般上传照片都要进行压缩。
```oc
	//处理图片
        NSData *data = UIImageJPEGRepresentation(self.uploadImage, 1.0);
        NSUInteger imageDataLength = [data length] / 1024;
        	if (imageDataLength > 100) {
        	CGFloat compressionQuality = (CGFloat) ((100 * 4)/ imageDataLength);
        	data = UIImageJPEGRepresentation(self.uploadImage, compressionQuality);
        }

```
这样图片的大小就可以减小从而提高用户体验。但是还有一个`[data bytes]`方法有待探索。
#按钮为单个可选时
```oc
- (void)buttonClick:(HMTabBarButton *)button
{
    self.selectedButton.selected = NO;
    button.selected = YES;
    self.selectedButton = button;
}
```
#在真机运行时出现Bitcode错误提示
在`Build Setting`设置`Enable Bitcode`为`NO`。
#字典转模型注意事项
模型中的Key名字要和字典中的Key一模一样。方法为`[model setValuesForKeysWithDictionary:dictionary]`最好的模型里写上`-(void)setValue:(id)value forUndefinedKey:(NSString *)key`。当然可以用第三方框架[MJExtension](https://github.com/CoderMJLee/MJExtension)实现模型和字典互转。
#元数据
程序中有元数据时，最好设计成为单例和只读模式。确保数据安全。
#NSValue转CGRect
```oc
	NSValue *value = [NSValue valueWithCGRect:CGRectZero];
	CGRect rect = [value CGRectValue];
```
#OC中取出string中的数字
```oc
 NSRange rang = [text rangeOfCharacterFromSet:[NSCharacterSet decimalDigitCharacterSet]];
```

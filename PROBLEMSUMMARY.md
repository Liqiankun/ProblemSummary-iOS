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

#UIImageView问题
当有UIImageView有动画时不要添加手势事件

#根据日期得到星期
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
#添加PCH文件
先创建`pronuciationApp-Prefix.pch`文件，在`BuildSetting`里`Prefix Header`里添加`$(SRCROOT)/pronuciationApp/pronuciationApp-Prefix.pch`

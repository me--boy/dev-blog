#分页公式：

```(总数 + 每页的显示的数量 - 1)/每页显示的数量```
#iOS 发送普通的网络请求
	1:指明请求的方式(post/get)
	2:确定超时时间
	3:确定URL路径
	4:设置好Content-Length
	5:设置请求题
	-(NSMutableURLRequest *)createRequestWithData:(NSData *)data{
    	NSMutableURLRequest *request = [[NSMutableURLRequest alloc] init];
    	[request setHTTPMethod:@"POST"];
    	[request setTimeoutInterval:REQUEST_TIMEOUT_INTERVAL];
    	[request setURL:[NSURL URLWithString:SQUARE_REQUEST_URL]];
    	[request setValue:[NSString stringWithFormat:@"%lu",(unsigned long)[data length]] forHTTPHeaderField:@"Content-Length"];
    	[request setHTTPBody:data];
    	return request;
	}
	
#iOS 发送带有表单的的网络请求
	1:指明请求的方式(post/get)
	2:确定超时时间
	3:确定URL路径
	4:设置好Content-Length
	5:设置为表单类型
	6:设置请求题
	-(NSMutableURLRequest *)createRequestWithData:(NSData *)data{
    	NSMutableURLRequest *request = [[NSMutableURLRequest alloc] init];
    	[request setHTTPMethod:@"POST"];
    	[request setTimeoutInterval:REQUEST_TIMEOUT_INTERVAL];
    	[request setURL:[NSURL URLWithString:SQUARE_REQUEST_URL]];
    	[request setValue:[NSString stringWithFormat:@"%lu",(unsigned long)[data length]] forHTTPHeaderField:@"Content-Length"];
    	[request setValue:@"multipart/form-data" forHTTPHeaderField:@"Content-Type"];
    	[request setHTTPBody:data];
    	return request;
	}
#清除tableView多余的分割线
    UIView *diver = [[UIView alloc] init];
    diver.backgroundColor = [UIColor clearColor];
    [self.tableView setTableFooterView:diver];
    [self.tableView setTableHeaderView:diver];

#指向self的弱指针
	__weak typeof(self) weakSelf = self;
#  加载照相机
	-(void)SetupCamera{
    	if ([UIImagePickerController 	isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
        	UIImagePickerController *picker = [[UIImagePickerController alloc] init];//初始化
        	picker.delegate = self;
        	picker.allowsEditing = YES;//设置可编辑
        	picker.sourceType = UIImagePickerControllerSourceTypeCamera;
        	[self presentViewController:picker animated:YES completion:^{}];
    	}
    
	}
#根据UIcolor对象创建UIImage对象
	
	+ (UIImage *)imageWithColor:(UIColor *)color
	{
   	
   		CGFloat imageW = 100;
    	CGFloat imageH = 100;
    
    	//开启基于位图的图形上下文
    	UIGraphicsBeginImageContextWithOptions(CGSizeMake(imageW, imageH), NO, 0.0);
    	//画一个color颜色的矩形框
    	[color set];
    	UIRectFill(CGRectMake(0, 0, imageW, imageH));
    	//拿到图片
    	UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
    
    	//关闭上下文
    	UIGraphicsEndImageContext();
    	return image;
	}
#更新APP徽标的时候，在IOS8中需要授权，否则可能出错。或提示

##Attempting to badge the application icon but haven't received permission from the user to badge the application
	#ifdef SUPPORT_IOS8
    if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 8.0) {
        UIUserNotificationType myTypes = UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeSound;
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:myTypes categories:nil];
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
    }else
	#endif
    {
        UIRemoteNotificationType myTypes = UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeSound;
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes:myTypes];
    }
    
#根据文字计算文字的size
	/**
 	*  根据文字计算文字的size
 	*
 	*  @param size 最大的文字宽度
 	*  @param font 文字的字体大小`
 	*/
	- (CGSize)TextSize:(CGSize)maxSize WithFont:(UIFont *)font
	{

    CGSize textSize = CGSizeZero;
    if (IOS7) {//ios7 以上版本
        //换行模式
        NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc] init];
	//        NSParagraphStyle的换行模式是只读的 所以要用NSMutableParagraphStyle
        paragraphStyle.lineBreakMode = NSLineBreakByWordWrapping;
        //设置字段
        NSDictionary *textAtt = @{NSFontAttributeName: font,NSParagraphStyleAttributeName: paragraphStyle};
        
        textSize = [self boundingRectWithSize:maxSize options:(NSStringDrawingUsesLineFragmentOrigin) attributes:textAtt context:nil].size;
        
    }else{
        //ios6
        textSize = [self sizeWithFont:font constrainedToSize:maxSize lineBreakMode:(NSLineBreakByWordWrapping)];
    }
    
    return textSize;
	}
#RGB 自定义的快速穿件
```16进制```

	#define UIColorFromRGB(rgbValue)        [UIColor colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0f green:((float)((rgbValue & 0xFF00) >> 8))/255.0f blue:((float)(rgbValue & 0xFF))/255.0f alpha:1.0f]
```十进制```

	#define UIColorWithRGBA(r,g,b,a)        [UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:a]
#UIColor对象转变为UIImage对象

	+ (UIImage *)imageWithColor:(UIColor *)color
	{
    CGFloat imageW = 100;
    CGFloat imageH = 100;
    
    //开启基于位图的图形上下文
    UIGraphicsBeginImageContextWithOptions(CGSizeMake(imageW, imageH), NO, 0.0);
    //画一个color颜色的矩形框
    [color set];
    UIRectFill(CGRectMake(0, 0, imageW, imageH));
    //拿到图片
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
    
    //关闭上下文
    UIGraphicsEndImageContext();
    return image;
	}
#给字符串添加下划线
	- (NSAttributedString *)underlineString:(NSString *)string
	{
    NSDictionary* attr = @{NSFontAttributeName:[UIFont systemFontOfSize:14], 	NSForegroundColorAttributeName:RGBCOLOR(51, 153, 255), NSUnderlineStyleAttributeName:	[NSNumber numberWithInt:NSUnderlineStyleSingle]};
    return [[NSAttributedString alloc] initWithString:string attributes:attr];
	}
#iPhone震动
	- (void) vibratePhone
	{
    if ([UIApplication sharedApplication].applicationState == UIApplicationStateActive)
    {
        AudioServicesPlayAlertSound(kSystemSoundID_Vibrate);
    }
	}
	应用有一些文件需要永久的存储在本地使应用支持离线功能。但是这些文件并不包含用户数据，无需备份。如何防止这些文件被备份。

#在iOS上，应用负责确保只有用户数据而不包含应用数据被备份到iCloud和iTunes上。具体的步骤在不同的iOS 版本各有不同。所以对不同的版本进行区分描述。关于具体哪些数据不应该被备份，参见App Backup Best Practices section of the iOS App Programming Guide。
	+ (BOOL)addSkipBackupAttributeToItemAtPath:(NSString *)filePath
	{
    if (![SYFilePath fileExists:filePath])
    {
        return NO;
    }
    NSError *error = nil;
    NSURL* url = [NSURL fileURLWithPath:filePath];
    BOOL success = [url setResourceValue: [NSNumber numberWithBool: YES]
                                  forKey: NSURLIsExcludedFromBackupKey error: &error];
    if(!success){
        NSLog(@"Error excluding %@ from backup %@", [url lastPathComponent], error);
    }
    return success;
	}	
	

```注意：应用应该避免将应用数据和用户数据和在相同的文件中。这样会增加不必要的备份大小并且被认为是违反iOS的数据存储指南。```
#检查目录是否存在
	+ (BOOL) fileExists:(NSString *)filePath
	{
    BOOL isDirectory;
    return [[NSFileManager defaultManager] fileExistsAtPath:filePath isDirectory:&isDirectory] && !isDirectory;
	}
#检测字符是否是表情 (注意这是java代码)
	private static boolean isEmojiCharacter(char codePoint) {          return !((codePoint == 0x0) ||                  (codePoint == 0x9) ||                  (codePoint == 0xA) ||                  (codePoint == 0xD) ||                  ((codePoint >= 0x20) && (codePoint <= 0xD7FF)) ||                  ((codePoint >= 0xE000) && (codePoint <= 0xFFFD)) ||                  ((codePoint >= 0x10000) && (codePoint <= 0x10FFFF)));      }
#iPhone 手机像素
	iphone4 ： 640x960 或者 960x640	phone5    640 x 1136或者1136 x 640 	phone6  750 x 1334 或者1334 x 750	phone6 plus 1242 x 2208 或者  2208x1242
#根据文字计算文字的size

	/**
	 *  根据文字计算文字的size
	 *
 	 *  @param size 最大的文字宽度
 	 *  @param font 文字的字体大小`
 	 */
 	#define IOS7         [[[UIDevice currentDevice] systemVersion] doubleValue] >= 7.0
 	
	- (CGSize)TextSize:(CGSize)maxSize WithFont:(UIFont *)font
	{
	
    CGSize textSize = CGSizeZero;
    if (IOS7) {//ios7 以上版本
        //换行模式
        NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc] init];
	//        NSParagraphStyle的换行模式是只读的 所以要用NSMutableParagraphStyle
        paragraphStyle.lineBreakMode = NSLineBreakByWordWrapping;
        //设置字段
        NSDictionary *textAtt = @{NSFontAttributeName: font,NSParagraphStyleAttributeName: paragraphStyle};
        
        textSize = [self boundingRectWithSize:maxSize options:(NSStringDrawingUsesLineFragmentOrigin) attributes:textAtt context:nil].size;
        
    }else{
        //ios6
        textSize = [self sizeWithFont:font constrainedToSize:maxSize lineBreakMode:(NSLineBreakByWordWrapping)];
    }
    
    return textSize;
	}
#__weak typeof(self) weakSelf = self;
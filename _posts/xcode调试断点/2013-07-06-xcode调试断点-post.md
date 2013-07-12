---
layout: post
title: "xcode调试断点"
description: ""
category: iphone
tags: [iphone, debug]
---
{% include JB/setup %}

**参考**  
[1] [http://developer.apple.com/library/ios/#recipes/xcode_help-breakpoint_navigator/articles/about_breakpoint_navigator.html#//apple_ref/doc/uid/TP40010433-CH6-SW1](http://developer.apple.com/library/ios/#recipes/xcode_help-breakpoint_navigator/articles/about_breakpoint_navigator.html#//apple_ref/doc/uid/TP40010433-CH6-SW1)  
调试程序时，断点是必不可少的工具，下面介绍一下断点具体使用 

###1.普通断点
如下图所示，在xcode编辑框左侧点击 就出现一个断点，右击出现 “编辑断点”等选项。如下图1代码所示，添加断点后，每次for循环都会在NSLog 40行停住。  
![图1](/blog/{{page.title}}/1.png)

![图2](/blog/{{page.title}}/2.png)

如果想在某一条件下 断点才生效，则添加条件选项，右击断点，选择编辑断点，如下图所示，只有当item 为“three”时断点才生效，中断程序运行。
(BOOL)[item isEqualToString:@"three"] 前面的（BOOL）是必须的。否则console会提示类型不符号，导致条件不能生效。
从下图中可以看出，还可以设置忽略断点次数，以及当断点条件满足时 要做的事情，图中所示是  
po item 输出 item变量的值  
bt 表示输出 方法调用堆栈信息
haha 表示用系统的PPS 语音读出“haha”发音提示。
更多的gdb 或lldb命令，网上搜下，或者直接在命令行输入help

![图3](/blog/{{page.title}}/3.png)

当然条件断点一般可以用以下方法替换
![图4](/blog/{{page.title}}/4.png)

###2.异常断点（Exception breakpoint)
如下代码所示，

	- (void)testExceptionBreakPoint
	{
		NSString* value = nil;
		NSMutableDictionary* mutDict = [NSMutableDictionary dictionaryWithCapacity:0];
		[mutDict setObject:value forKey:@"key"];
	}
其中`[mutDict setObject:value forKey:@"key"];` 会导致如下错误信息，并且系统中断在mian函数上，你虽然从错误信息看出是 `setObjectForKey` 导致异常，但在一个工程中，用到setObjectForKey 可能有很多地方，你还是不能确定哪一行代码导致的，这时就需要添加异常断点。

	*** Terminating app due to uncaught exception 'NSInvalidArgumentException', 	reason: '*** setObjectForKey: object cannot be nil (key: key)'
	*** First throw call stack:
	(0x1c8d012 0x10cae7e 0x1d100de 0x2ede 0x2fc8 0xf3817 0xf3882 0x42a25 	0x42dbf 0x42f55 0x4bf67 0x296b 0xf7b7 0xfda7 0x10fab 0x22315 0x2324b 	0x14cf8 0x1be8df9 0x1be8ad0 0x1c02bf5 0x1c02962 0x1c33bb6 0x1c32f44 0x1c32e1b 	0x107da 0x1265c 0x2672 0x25a5)
	libc++abi.dylib: terminate called throwing an exception


添加异常断点如下 

![图5](/blog/{{page.title}}/5.png)  
 
![图6](/blog/{{page.title}}/6.png)

异常断点建议是全局的，即在User下面，保存在Xcode的设置信息中，以后创建任意工程时 该断点都时有效的。创建断点后，右击选择·`“move breakpoint to ”` 选择 `“User”`即可。
再次运行程序后，可以很简单的找到 `[mutDict setObject:value forKey:@"key"];`  之前的异常是这一行导致的。

![图7](/blog/{{page.title}}/7.png)

###3.符号断点（symbolic breakpoint）
符号断点 可以针对某一个方法设置断点，不仅可以的是自己创建的类的方法，更重要的是系统SDK中的函数或方法也可以设置断点。
比如为了在异常时设置断点，我们知道系统会调用`objc_exception_throw` 函数，则只要创建 objc_exception_throw 的符号断点，如下述所示。

![图8](/blog/{{page.title}}/8.png)

![图9](/blog/{{page.title}}/9.png)


运行后，如下图所示，也能轻松的找到 [mutDict setObject:value forKey:@"key"];   这一行代码导致异常，如使用异常断点时一致。

![图10](/blog/{{page.title}}/10.png)

如果想为自己的类方法 设置符号断点也可以，如下图所示，为类BI_TestClass 类的实例方法  - (void)method:(id)obj  创建了 符号断点，减号（-）表示实例方法，加号（+）表示类方法。那么在每次调用该方法时都会中断。当然你也可以直接在该方法中设置一个普通断点达到相同效果。
特别注意：`-[BI_TestClass method:]`  不需要 在引号内，虽然它Example 的格式是需要包含在双引号内的，坑爹的Example 导致我刚开始一直设没有效果。

![图11](/blog/{{page.title}}/11.png)


其他常用的符号断点 -[NSObject doesNotRecognizeSelector:]

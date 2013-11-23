--- 
layout: post
title: "SparkInspector使用说明"
description: "" 
category:   
tags: [其他]
---
{% include JB/setup %}

###1.什么是SparkInspector###  
提供了一种智能方式来窥探app的整体结构，以及调试。     

* 提供了一种你从来没想到过的方式查看app，能够三维方式查看app的view层级关系，并且支持实时修改view属性查看效果。
* notification 监视，记录完整的调用堆栈，包括接收者，触发方法等内容。

###2.SparkInspector 特点###    
* 实时监控，app运行时同时能在SparkInspector 看到界面效果，及格view层级关系
* 实时测试，修改view参数，实时看到效果。
* 支持模拟器或者真机，SparkInspector 通过无线连接测试的app
* 实时查看通知notification，有些app基于notification驱动，发送通知后，界面也发生变化，这样不方便查看中间的逻辑。通过SparkInspector不需要设置断点，能查看实时的通知，并且支持重发通知。

###3.SparkInspector Get start###
**参考:**<http://www.sparkinspector.com/>   

1. Run Spark, Open the Integration Assistant (`⌘+2`);也可以通过菜单Window-》Framwork setup assitant 启动。   
**参见:** <http://www.sparkinspector.com/framework_setup.html>

	**使用pods：** 则在Podfile添加 `pod 'SparkInspector'`,然后调用`pod install`更新即可	  
	**不使用pods：**则按如下步骤操作，或者简单点直接选择工程文件即可。
	1. Select your project in the Xcode sidebar, click 'Build Phases' and open the 'Link Binary with Libraries' section. Tap the '+' icon and add the following frameworks to your project:

			 - QuartzCore.framework
			 - libz.dylib
	2. Click 'Build Settings.' Click the 'Other Linker Flags' row and  add two new flags for the Debug build configuration: `-ObjC` and `-framework SparkInspector`. Be sure that you have added these to the Debug build configuration only.
	3. Open the Spark Inspector app and select Spark Inspector > Reveal Framework in Finder from the menu bar. Drag and drop SparkInspector.framework into the sidebar of Xcode under 'Frameworks'.

			 Sidenote: We chose to keep the framework inside the application's bundle so it's automatically updated when you update the app. If you're working with a team and everyone has the app in their Applications folder, it should work fine. You might prefer using Cocoapods, or copying the framework into your project's directory and pushing it to source control.
	4. Check that the framework was NOT added to the "Link Binary with Libraries" phase in your target's build settings. If it is, it will be linked into your binary when you do release builds and may cause your app to be rejected from the App Store. If it's listed there, remove it now.
 
2. Select your Xcode project file
3. Watch as configuration changes are made 
4. Build and run! Spark will connect automatically.

###4.碰到问题
**问题1**  
![图1](/images/{{page.title}}/1.png)  

**解决方法**  
链接libz.dylib即可。

**问题2**   
编译通过，但在sparkInspector 未能检测到app

**解决方法**  
other link flags 中添加`-ObjC` and `-framework SparkInspector`，关键是添加`-ObjC`，`-framework SparkInspector` 和在`Build Phases` 下的`Link Binary with Libraries` 中设置SparkInspector.framework 是一致的。

**问题3**  
![图2](/images/{{page.title}}/2.png)  

**解决方法**  
删除`Build Phases` 下的`Link Binary with Libraries` 中的SparkInspector。虽然能编译通过，但是无效果，对应问题2，然后添加other link flags 中添加`-ObjC` and `-framework SparkInspector` 会出现同样的问题。

![图3](/images/{{page.title}}/3.png)  


添加sparkInspector.framework后会在`framework search path`中设置对应的绝对路径，虽然你后面选`recursive`也不行。

***终级方法，但不知道为什么***  
如果你在已经有的绝对路径后面加`../..` 也可以，或者使用`$(SRCROOT)/../ ` 也可以，当然别忘记设置为`recursive`


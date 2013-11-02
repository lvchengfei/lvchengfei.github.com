--- 
layout: post
title: "SparkInspector使用说明"
description: "" 
category:  
tags: [工具] 
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


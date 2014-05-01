---
title: 易聊
description: 5分钟为你的APP加入完整的微信聊天功能
category: emchat
layout: docs
---

# 快速入门（五分钟运行易聊demo) 


## 1.下载并运行易聊demo (Android) 

###  1.1 什么是易聊demo

易聊demo展示了怎样使用易聊SDK快速创建一个完整的类微信聊天APP。展示的功能包括：注册新用户，用户登录，添加好友，单聊，群聊，发送文字，表情。

易聊demo源代码已在github上开源供开发者下载，以帮助开发者更好的学习了解易聊SDK。

### 1.2 下载并运行易聊demo 

    

1. 易聊demo下载：[下载连接](#{site.base_url}/docs/downloads/downloads.html)
    
2. 解压缩下载下来的androidsdk.zip


3. 在手机上安装chatdemo-nonui.apk。
   


1.  注册：在登录页面选择“注册”，进入到注册页面。 可使用任意英文字母及数字组合作为用户名或手机号码作为用户名进行注册。

    

1. 登录：输入已注册的用户名及密码登录

    

1. 添加好友

    

1. 聊天页面

    

1. 会话页面


## 从源代码级别深入了解易聊demo (Android)

 
### 下载易聊demo源代码及易聊SDK 

    [http://www.easemob.com/downloads/sdkexamples-android.rar](http://www.easemob.com/downloads/sdkexamples-android.rar)

 

易工厂提供了一系列demo以帮助开发者更好的学习了解易聊SDK。所有demo均已开源并放到github上供开发者使用。你可以clone这些项目来学习了解易聊SDK，也可以在这些demo基础上快速创建你自己的真正项目。易聊SDK在github的下载地址是：

    [https://github.com/easemob/sdkexamples-android](https://github.com/easemob/sdkexamples-android)

### 在Eclipse/IDEA中创建易聊SDK project 



1. 解压已下载的sdkexamples-android.rar。



1. IDEA IDE： 打开菜单“File - New Project”，选择“Create project from existing sources”, 在“Project files location”选项，选择解压后的sdkexamples-android下的emclient-android路径, 点击数次 "Next"直到“Finish”。


 	Eclipse IDE： 打开菜单“ File - New - Project“，选择”Android Project from Existing Code”， 选择解压后的sdkexamples-android下的emclient-android路径,点击“Finish”。

### 在Eclipse/IDEA中创建易聊demo project 



1. 解压已下载的sdkexamples-android.rar。



1. IDEA IDE： 打开菜单“File - New Project”，选择“Create project from existing sources”, 在“Project files location”选项，选择解压后的sdkexamples-android下chat路径, 点击数次 "Next"直到“Finish”。

	项目创建成功后，导入易工厂SDK project： 鼠标右键点击易聊demo project， 选择“Properties",在弹出菜单选择”Android - > Library -> Add" ,添加已创建的易工厂SDK project。

	Eclipse IDE： 打开菜单“ File - New - Project“，选择”Android Project from Existing Code”， 选择解压后的sdkexamples-android下chat路径,点击“Finish”。

	项目创建成功后，导入易工厂SDK project： 鼠标右键点击易聊demo project， 选择“Properties",在弹出菜单选择”Android - > Library -> Add" ,添加已创建的易工厂SDK project。


### 2.5. 更改项目的orgkey/appkey(可跳过) ###

为方便测试，易聊demo内置使用一个公用的测试账户。你可以使用这个公用的测试账户来测试使用易聊demo。

当你创建自己的项目时，你需要更改项目的orgpkey/appkey。修改AndroidManifest.xml以下值即可：

        <meta-data
            android:name="EASEMOB_ORGKEY"
            android:value="easemob" />
        <meta-data
            android:name="EASEMOB_APPKEY"
            android:value="chatdemo" />

### 2.6. 深入理解demo背后的代码 ###

注册新用户：见com.easemob.demo.Register.java

 

登录：com.easemob.demo.Login.java

 

添加好友：com.easemob.demo.AddContact.java

 

聊天：com.easemob.demo.ChatActivity.java

 
# 3. Bug报告跟踪 #

请使用以下地址来报告跟踪bug：

https://github.com/easemob/sdkexamples-android/issues



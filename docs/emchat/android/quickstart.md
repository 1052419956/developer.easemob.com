---
title: Easemob Chat 快速入门
description: Easemob Chat 快速入门
category: push
layout: docs
---
# 快速入门（五分钟运行易聊demo) #


## 1. 下载并运行易聊demo (Android)  ##
### 1.1. 什么是易聊demo ###

易聊demo展示了怎样快速创建一个完整的微信聊天APP。展示的功能包括：用户登录，注册新用户，添加好友，单聊，群聊，发送文字，语音，地理位置，图片，视频等。
### 1.2. 下载并运行易聊demo app ###

    易聊demo apk下载：http://www.easemob.com/downloads/chatdemo.apk
    安装chatdemo.apk。
    注册：在登录页面选择“注册”，进入到注册页面。 可使用任意英文字母及数字组合作为用户名或手机号码作为用户名进行注册。

    登录：输入已注册的用户名及密码登录

    添加好友

    聊天页面

    会话页面


## 2. 从源代码级别深入了解易聊demo (Android) ##

 
### 2.1. 下载易聊demo源代码及易聊SDK ###

    易聊demo 源代码下载: http://www.easemob.com/downloads/sdkexamples-android.rar

 

易工厂提供了一系列demo以帮助开发者更好的学习了解易工厂SDK。所有demo均已开源并放到github上供开发者使用。你可以clone这些项目来学习了解工厂SDK，也可以在这些demo基础上快速创建你自己的真正项目。易工厂SDK在github的下载地址是：https://github.com/easemob/sdkexamples-android

### 2.2. 在Eclipse/IDEA中创建易聊SDK project ###

解压已下载的sdkexamples-android.rar。

IDEA IDE： 打开菜单“File - New Project”，选择“Create project from existing sources”, 在“Project files location”选项，选择解压后的sdkexamples-android下的emclient-android路径, 点击数次 "Next"直到“Finish”。

Eclipse IDE： 打开菜单“ File - New - Project“，选择”Android Project from Existing Code”， 选择解压后的sdkexamples-android下的emclient-android路径,点击“Finish”。

### 2.3. 在Eclipse/IDEA中创建易聊demo project ###

解压已下载的sdkexamples-android.rar。

IDEA IDE： 打开菜单“File - New Project”，选择“Create project from existing sources”, 在“Project files location”选项，选择解压后的sdkexamples-android下chat路径, 点击数次 "Next"直到“Finish”。

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

 

聊天：

 
# 3. Bug报告跟踪 #

请使用以下地址来报告跟踪bug：

https://github.com/easemob/sdkexamples-android/issues



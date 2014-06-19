---
title: 环信
description: 5分钟为你的APP加入完整的微信聊天功能
category: emchat
layout: docs
---

## 自主开发集成聊天配置指导(Android)

### 添加环信的类库

在自行开发的应用中，集成环信聊天需要把libs文件夹下*easemobchat_2.0.0.jar*和3rdpartylibs文件夹下*httpmime-4.2.jar*拷贝到你的项目的libs文件夹底下


![alt text](demo_dirs.jpg "demo") 

 ![](http://i.imgur.com/NrMwsez.jpg)

### 添加环信的配置信息

在清单文件AndroidManifest.xml里加入以下权限，以及写上你注册的appkey

	<uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_MOCK_LOCATION" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>  
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    
		
		<!--  设置环信SDK的appkey -->
	<meta-data android:name="EASEMOB_APPKEY"  android:value="你申请的appkey" />
	<service android:name="com.easemob.chat.EMChatService" />

关于EASEMOB_APPKEY，请登录或注册[环信开发者后台](https://console.easemob.com),申请APPKEY后，进行相关配置。（测试demo中 APPKEY为*easemob-demo#chatdemo*）
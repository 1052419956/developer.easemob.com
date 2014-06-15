---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

# EaseMob集成示例操作流程

## 有聊天页面的Demo

### Demo说明

#### 提供测试的AppKey(easemob-demo#chatdemoui)

#### 需要至少2个账号，互加好友，互发消息

### Demo演示流程

#### 下载环信Demo及SDK

请到[这里下载](http://www.easemob.com/downloads.php)环信Demo及SDK


  ![alt text](example_layout.png "Demo")
  
#### 运行程序

账号不支持中文

 ![alt text](chatUIDemoLogin.png "Demo")
 
#### 登录成功进入首页

会话：聊天的会话列表

通讯录：申请通知，群组，好友列表

设置：退出登录

 ![alt text](chatUIDemoHome.png "Demo")
 
#### 添加好友

运行程序并登录账号2。点击“通讯录”页面的“+”

 ![alt text](chatUIDemoOther.png "Demo")
 
输入好友用户名（账号1），进行搜索添加
 
 ![alt text](chatUIDemoAddFriend.png "Demo")
 
在账号1接收账号2的好友申请
 
 ![alt text](chatUIDemoApplyList.png "Demo")
 
#### 账号1和账号2互发消息

 ![alt text](chatUIDemoChatList.png "Demo") 
 
### 其他说明
#### 监测网络状态

在MainViewController类中有体现，监测以下方法

	#pragma mark - IChatManagerDelegate 登录状态变化

	- (void)didConnectionStateChanged:(EMConnectionState)connectionState
	{
    	
	}
	
![alt text](chatUIDemoNetwork.png "Demo") 

#### 账号在其它设备登录

账号在其它设备登录时, 当前设备会自动断开连接(收到该回调时, 当前客户端已不能收发消息了, 当前客户端必须处理该回调, 退出到登录页面, )

	- (void)didLoginFromOtherDevice
	{
	    //退出到登录页面代码
	}


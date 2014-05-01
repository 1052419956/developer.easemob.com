---
title: 易聊
description: 5分钟为你的APP加入完整的微信聊天功能
category: emchat
layout: docs
---

# 快速入门（五分钟运行易聊demo) 


## 1.下载易聊demo (iOS) 

###  1.1 什么是易聊demo

易聊demo展示了怎样使用易聊SDK快速创建一个完整的类微信聊天APP。展示的功能包括：易聊SDK初始化，登录，登出，注册消息接收listener, 发送消息

易聊demo源代码已在github上开源供开发者下载，以帮助开发者更好的学习了解易聊SDK。

### 1.2 下载易聊demo 

    

1. 下载易聊demo：[下载链接](#{site.base_url}/docs/downloads/downloads.html)

2. 解压缩iOSsdk.zip后会得到以下目录结构：
 
 ![alt text](example_layout.png "Title")


## 2.运行易聊demo (iOS) 

1. 在手机上安装chatdemo-nonui.ipa
    
 
2. 运行chatdemo-nonui: 点击“发送文本消息”，会发送消息给测试机器人（其账号为"bot"）。该测试机器人接收到消息后会把接收的消息原封不动的自动发送回来

 ![alt text](demo.png "demo")

## 3.快速集成

#### 1.下载EaseMobSDK:
下载EaseMobSDK
[下载链接](http://www.easemob.com/downloads/iOSSDK.zip)

#### 2.将EaseMobSDK拖入到项目中
 ![alt text](import.png "Title")
 
#### 3.加入依赖库
 ![alt text](addLib.png "Lib")
 
#### 4.设置Linker
![alt text](link.png "link")

*	向Other Linker Flags 中添加 -Objc。(如果已有，则不需要再添加)

#### 5.设置Architectures
![alt text](Active.png "Active")

#### 5.在Info中配置服务器信息
![alt text](info.png "info")
 
 *	EASEMIB_APPKEY 申请的公司名
 *	EASEMOB_USERSERVICE_FACTORY_CLASS 用户体系工厂类 （默认UGUserServiceFactory）
  
 

## 4. 从源代码级别深入了解易聊demo (iOS)


### 4.1. 深入理解易聊demo背后的代码 ###

#### 1.注册listener,以接收聊天消息:RootViewController.m

    [[EaseMob sharedInstance].chatManager addDelegate:self
                                        delegateQueue:nil];

#### 2. 登录：见RootViewController+Login.m ####

    [[EaseMob sharedInstance].userManager asyncLoginWithUsername:@"test"
                                                        password:@"123456"
                                                      completion:
     ^(NSDictionary *loginInfo,EMError *error){
         if (error) {
             [self errorMsgShowAlert:error];
         }else {
             NSLog(@"登录成功");
         }
     } onQueue:nil];


#### 3. 退出登录：见RootViewController+Login.m ####

	[[EaseMob sharedInstance].userManager asyncLogoff];

#### 4. 发送消息：见RootViewController+sendChat.m ####

    EMChatText *text = [[EMChatText alloc] initWithText:message];
    EMMessageBody *body = [[EMTextMessageBody alloc] initWithMessage:text];
    NSString *myUsername = [[[EaseMob sharedInstance].userManager loginInfo]
                            objectForKey:kUserLoginInfoUsername];
    EMMessage *msg = [[EMMessage alloc] initWithSender:myUsername
                                              receiver:receiverUsername
                                                bodies:[NSArray arrayWithObject:body]];
    
    [[EaseMob sharedInstance].chatManager sendMessage:msg
                                             progress:nil
                                                error:nil];


#### 5. 接收聊天消息并显示：见RootViewController.m ####

	-(void)didReceiveMessage:(EMMessage *)message
          inConversation:(EMConversation *)conversation{
          
    	EMMessageBody *body = message.messageBodies.lastObject;
		if (body.messageType == eMessageType_Text) {
      	  if (!_textView.text || _textView.text.length == 0) {
       	     _textView.text = ((EMTextMessageBody *)body).text.text;
      	  }else{
      	      _textView.text = [NSString stringWithFormat:@"%@\n%@",_textView.text,								((EMTextMessageBody *)body).text.text];
      	  }
    	}
	}




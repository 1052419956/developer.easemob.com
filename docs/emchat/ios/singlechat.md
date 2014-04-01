---
title: 易聊
description: 5分钟为你的APP加入完整的微信聊天功能
category: emchat
layout: docs
---
## 单聊:

### 1.登录
     [[EaseMob sharedInstance].userManager loginWithUsername:username
                                                    password:password
                                                  andCompany:kCompanyName
                                                       error:&error];

### 2.退出登录
	[[EaseMob sharedInstance].userManager logoffWithError:&error];

### 3.发送消息

#### 3.1发送文本消息及表情 
	EMChatText *text = [[EMChatText alloc] initWithText:str];
    EMMessageBody *body = [[EaseMob sharedInstance].chatManager createTextMessageBody:text];
    EMMessage *msg = [[EMMessage alloc] initWithSender:sender
                                              receiver:username
                                                bodies:[NSArray arrayWithObject:body]];
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];
#### 3.2发送语音消息
#### 3.3发送图片消息
    EMChatImage *chatImage = [[EMChatImage alloc] initWithImage:image displayName:@"image"];
    EMChatImage *chatThumbnailImage = [[EMChatImage alloc] initWithImage:image displayName:@"image"];
    EMMessageBody *body = [[EaseMob sharedInstance].chatManager createImageMessageBody:chatImage
                                                               thumbnailImage:chatThumbnailImage
                                                               optimized:YES];

#### 3.4发送地理位置消息

### 4.接收消息
	注册一个delegate
	[[EaseMob sharedInstance].chatManager addDelegate:self 	delegateQueue:nil];
	
	#pragma mark - IChatManagerDelegate
	-(void)didReceiveMessage:(EMMessage *)message{
	}
### 5.获取聊天记录
	    EMConversation *conversation = [[EaseMob sharedInstance].chatManager
                                    conversationForChatter:username];
### 6.删除聊天记录
	[[EaseMob sharedInstance].chatManager removeMessage:message];
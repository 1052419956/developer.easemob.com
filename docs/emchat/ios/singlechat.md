---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---
## 单聊:

### 1.登录 ###

如果使用自己的用户体系，需要先登录您的用户体系，成功后登录IM部分;

	[[EaseMob sharedInstance].chatManager asyncLoginWithUsername:username 
	password:@"123456" 
	completion:
	^(NSDictionary *loginInfo, EMError *error) {
		[self hideHud];
		if (error) {
			NSLog(@"登录失败");
		}else {
			NSLog(@"登录成功");
		}
	} onQueue:nil];
     
loginInfo包含账号，密码等信息;

### 2.退出登录 ###

	[[EaseMob sharedInstance].chatManager asyncLogoff];

### 3.发送消息 ###

#### 3.1发送文本消息及表情 ####

	EMChatText *text = [[EMChatText alloc] initWithText:str];
	EMMessageBody *body = [[EaseMob sharedInstance].chatManager 	createTextMessageBody:text];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username
	bodies:@[body]];
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];


#### 3.2发送语音消息 ####

##### 3.2.1 录音 #####

	EMError *error = nil;
	[[EaseMob sharedInstance].chatManager startRecordingAudioWithError:&error];
	if (error) {
		NSLog(@"开始录音失败");
	}

##### 3.2.2 停止录音 #####

	[[EaseMob sharedInstance].chatManager asyncStopRecordingAudioWithCompletion:
	^(EMChatVoice *voice, EMError *error){
	} onQueue:nil];
     
##### 3.2.3 取消录音 #####

	[[EaseMob sharedInstance].chatManager asyncCancelRecordingAudioWithCompletion:
	^(EMChatVoice *voice, EMError *error){
	} onQueue:nil];


##### 3.2.4 发送录音 #####

录音结束时，会得到一个EMChatVoice对象，之后用对象生成messageBody即可发送;

	EMMessageBody *body = [[EMVoiceMessageBody alloc]initWithMessage:voice];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username
	bodies:@[body]];
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];

	
#### 3.3发送图片消息 ####

	EMChatImage *chatImage = [[EMChatImage alloc] initWithImage:image											displayName:@"image"];
	EMMessageBody *body = [[EaseMob sharedInstance].chatManager									createImageMessageBody:chatImage
									thumbnailImage:nil
									optimized:YES];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username
											bodies:@[body]];
	
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];

	
chatImage：大图

chatThumbnailImage：缩略图（可不传）

#### 3.4发送地理位置消息 ####

在得到经纬度和位置信息后，可以生成对应的LocationType的Message，之后发送即可;
	
	EMChatLocation *chatLocation = [[EMChatLocation alloc]
	initWithLatitude:latitude
	longitude:longitude
	address:address];
	EMMessageBody *body = [[EMLocationMessageBody alloc]
	initWithMessage:chatLocation];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username
	bodies:@[body]];
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];



### 4.接收消息 ###

#### 4.1 实现委托 ####

在需要接受消息的页面，应该首先实现一个delegate:IChatManagerDelegate;
	
	@interface RootViewController : UIViewController<IChatManagerDelegate>


#### 4.2 注册接收消息 ####

	// 注册一个delegate
	[[EaseMob sharedInstance].chatManager addDelegate:self 	delegateQueue:nil];
	
	// 实现接收消息的委托
	#pragma mark - IChatManagerDelegate
	-(void)didReceiveMessage:(EMMessage *)message{
	}
	
收到消息后，会调用 -(void)didReceiveMessage:(EMMessage *)message; 

根据message.messageType 区分是哪种消息，之后做对应的解析;
	
### 5.获取聊天记录 ####

根据username可以得到一个conversation;

	EMConversation *conversation = [[EaseMob sharedInstance].chatManager
	conversationForChatter:username];
                                    
#### 5.1 根据messageID得到一条聊天记录 ####

	EMMEssage *message = [conversation loadMessage:message.messageID];

#### 5.2 根据messageID数组，得到一组聊天记录 ####

	NSArray *messages = [conversation loadMessages:messageIDs];
	
#### 5.3 得到所有messages ####

	NSArray *messages = [conversation loadAllMessages];
	
#### 5.4 根据时间得到要求条数的messages ####

	NSArray *messages = [conversation loadNumbersOfMessages:10
	before:[[NSDate new] timeIntervalSince1970]];
                                 	
### 6.删除聊天记录 ###
根据username可以得到一个conversation;

	EMConversation *conversation = [[EaseMob sharedInstance].chatManager
	conversationForChatter:username];
                                    
#### 6.1 删除一个EMMessage ####

	- (BOOL)removeMessage:(EMMessage *)message;

#### 6.2 删除一组EMMessages ####

	- (NSUInteger)removeMessages:(NSArray *)messages;

#### 6.3 删除该EMconversation下得所有EMMessages ####

	- (NSUInteger)removeAllMessages;

### 7. 设置消息状态 ###

#### 7.1 获取消息未读数量 ####
EMConversation中，提供了unreadMessagesCount属性;

	/**
	@method
	@abstract 获取此对话中所有未读消息的条数
	@discussion
	@result 此对话中所有未读消息的条数
	*/
	- (NSUInteger)unreadMessagesCount;


#### 7.2 设置一条消息的已读状态 ####
EMConversation中，提供了设置某一条message的状态的方法;
		
	/**
	@method
	@abstract 把本条消息标记为已读/未读
	@discussion
	@param isRead 已读或未读
	@result 是否成功标记此条消息
	*/
	- (BOOL)markMessage:(EMMessage *)message asRead:(BOOL)isRead;
	
	
#### 7.3 设置EMConversation下所有message为已读 ####
EMConversation中，提供了设置该EMConversation对象中所有message状态的方法;

	/**
	@method
	@abstract 把本对话里的所有消息标记为已读/未读
	@discussion
	@param isRead 已读或未读
	@result 成功标记的消息条数
	*/
	- (NSUInteger)markMessagesAsRead:(BOOL)isRead;
	
 

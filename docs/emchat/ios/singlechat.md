---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---
## 单聊:

### 1. 初始化 ###
在Appdelegate生命中期中，加入对应的初始化，以便SDK能正常工作;

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary 	*)launchOptions
	{
    	self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    	// Override point for customization after application launch.
    	self.window.backgroundColor = [UIColor whiteColor];

    	[self.window makeKeyAndVisible];
    
    	// 让SDK得到App目前的各种状态，以便让SDK做出对应当前场景的操作
		BOOL ret = [[EaseMob sharedInstance] application:application didFinishLaunchingWithOptions:launchOptions];
		return ret;
	}

	- (void)applicationWillResignActive:(UIApplication *)application
	{
		// 让SDK得到App目前的各种状态，以便让SDK做出对应当前场景的操作
		[[EaseMob sharedInstance] applicationWillResignActive:application];
	}

	- (void)applicationDidEnterBackground:(UIApplication *)application
	{
		// 让SDK得到App目前的各种状态，以便让SDK做出对应当前场景的操作
		[[EaseMob sharedInstance] applicationDidEnterBackground:application];
	}

	- (void)applicationWillEnterForeground:(UIApplication *)application
	{
		// 让SDK得到App目前的各种状态，以便让SDK做出对应当前场景的操作
		[[EaseMob sharedInstance] applicationWillEnterForeground:application];
	}

	- (void)applicationDidBecomeActive:(UIApplication *)application
	{
		// 让SDK得到App目前的各种状态，以便让SDK做出对应当前场景的操作
		[[EaseMob sharedInstance] applicationDidBecomeActive:application];
	}

	- (void)applicationWillTerminate:(UIApplication *)application
	{
		// 让SDK得到App目前的各种状态，以便让SDK做出对应当前场景的操作
		[[EaseMob sharedInstance] applicationWillTerminate:application];
	}
	
### 2. 登录 ###

如果使用自己的用户体系，需要先**登录您的用户体系**，成功后登录IM部分;

	[[EaseMob sharedInstance].chatManager asyncLoginWithUsername:username 
	password:@"123456" 
	completion:
	^(NSDictionary *loginInfo, EMError *error) {
		if (error) {
			NSLog(@"登录失败");
		}else {
			NSLog(@"登录成功");
		}
	} onQueue:nil];
     
loginInfo包含账号，密码等信息;

### 3. 退出登录 ###

**退出登录时, 需要先退出自己的用户系统, 然后调用下面的方法退出EaseMob服务器**

	[[EaseMob sharedInstance].chatManager asyncLogoff];

### 4. 发送消息 ###

#### 4.1 发送文本消息及表情 ####

	EMChatText *text = [[EMChatText alloc] initWithText:str];
	EMMessageBody *body = [[EaseMob sharedInstance].chatManager createTextMessageBody:text];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username
	bodies:@[body]];
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];


#### 4.2 发送语音消息 ####

##### 4.2.1 录音 #####

	EMError *error = nil;
	[[EaseMob sharedInstance].chatManager startRecordingAudioWithError:&error];
	if (error) {
		NSLog(@"开始录音失败");
	}

##### 4.2.2 停止录音 #####

	[[EaseMob sharedInstance].chatManager asyncStopRecordingAudioWithCompletion:
	^(EMChatVoice *voice, EMError *error){
	} onQueue:nil];
     
##### 4.2.3 取消录音 #####

	[[EaseMob sharedInstance].chatManager asyncCancelRecordingAudioWithCompletion:
	^(EMChatVoice *voice, EMError *error){
	} onQueue:nil];


##### 4.2.4 发送录音 #####

录音结束时，会得到一个EMChatVoice对象，之后用对象生成messageBody即可发送;

	EMMessageBody *body = [[EMVoiceMessageBody alloc]initWithMessage:voice];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username
	bodies:@[body]];
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];

	
#### 4.3 发送图片消息 ####

	EMChatImage *chatImage = [[EMChatImage alloc] initWithImage:image displayName:@"image"];
	EMMessageBody *body = [[EaseMob sharedInstance].chatManager createImageMessageBody:chatImage thumbnailImage:nil optimized:YES];
	EMMessage *msg = [[EMMessage alloc] initWithReceiver:username bodies:@[body]];
	
	[[EaseMob sharedInstance].chatManager sendMessage:msg progress:nil error:nil];

	
chatImage：大图

chatThumbnailImage：缩略图（可不传）

#### 4.4 发送地理位置消息 ####

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



### 5. 接收消息 ###

#### 5.1 实现委托 ####

在需要接受消息的页面，应该首先实现一个delegate:IChatManagerDelegate;
	
	@interface RootViewController : UIViewController<IChatManagerDelegate>


#### 5.2 注册接收消息 ####

	// 注册一个delegate
	[[EaseMob sharedInstance].chatManager addDelegate:self 	delegateQueue:nil];
	
	// 实现接收消息的委托
	#pragma mark - IChatManagerDelegate
	-(void)didReceiveMessage:(EMMessage *)message{
	}
	
收到消息后，会调用 -(void)didReceiveMessage:(EMMessage *)message; 

根据message.messageType 区分是哪种消息，之后做对应的解析;
	
### 6.获取聊天记录 ####

根据username可以得到一个conversation;

	EMConversation *conversation = [[EaseMob sharedInstance].chatManager
	conversationForChatter:username];
                                    
#### 6.1 根据messageID得到一条聊天记录 ####

	EMMEssage *message = [conversation loadMessage:message.messageID];

#### 6.2 根据messageID数组，得到一组聊天记录 ####

	NSArray *messages = [conversation loadMessages:messageIDs];
	
#### 6.3 得到所有messages ####

	NSArray *messages = [conversation loadAllMessages];
	
#### 6.4 根据时间得到要求条数的messages ####

	NSArray *messages = [conversation loadNumbersOfMessages:10
	before:[[NSDate new] timeIntervalSince1970]];
                                 	
### 7.删除聊天记录 ###
根据username可以得到一个conversation;

	EMConversation *conversation = [[EaseMob sharedInstance].chatManager
	conversationForChatter:username];
                                    
#### 7.1 删除一个EMMessage ####

	- (BOOL)removeMessage:(EMMessage *)message;

#### 7.2 删除一组EMMessages ####

	- (NSUInteger)removeMessages:(NSArray *)messages;

#### 7.3 删除该EMconversation下得所有EMMessages ####

	- (NSUInteger)removeAllMessages;

### 8. 设置消息状态 ###

#### 8.1 获取消息未读数量 ####
EMConversation中，提供了unreadMessagesCount属性;

	/**
	@method
	@abstract 获取此对话中所有未读消息的条数
	@discussion
	@result 此对话中所有未读消息的条数
	*/
	- (NSUInteger)unreadMessagesCount;


#### 8.2 设置一条消息的已读状态 ####
EMConversation中，提供了设置某一条message的状态的方法;
		
	/**
	@method
	@abstract 把本条消息标记为已读/未读
	@discussion
	@param isRead 已读或未读
	@result 是否成功标记此条消息
	*/
	- (BOOL)markMessage:(EMMessage *)message asRead:(BOOL)isRead;
	
	
#### 8.3 设置EMConversation下所有message为已读 ####
EMConversation中，提供了设置该EMConversation对象中所有message状态的方法;

	/**
	@method
	@abstract 把本对话里的所有消息标记为已读/未读
	@discussion
	@param isRead 已读或未读
	@result 成功标记的消息条数
	*/
	- (NSUInteger)markMessagesAsRead:(BOOL)isRead;
	
 

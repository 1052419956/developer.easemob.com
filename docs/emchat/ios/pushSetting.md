---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##消息推送设置:

###1.获取消息推送设置

登陆SDK成功之后，会从服务器返回消息推送的设置，读取方式

	EMPushNotificationOptions *options = [[EaseMob sharedInstance].chatManager pushNotificationOptions];
	
###2.修改消息推送设置
	
接口   将配置信息同步到服务器

	/*!
	 @method
	 @brief 更新消息推送相关属性配置（同步方法）
	 @param options    属性
	 @param pError     更新错误信息
	 @result    最新的属性配置
	 */
	- (EMPushNotificationOptions *)updatePushOptions:(EMPushNotificationOptions *)options
	                                           error:(EMError **)pError;
	
	/*!
	 @method
	 @brief 更新消息推送相关属性配置（异步方法）
	 @param options    属性
	 @param pError     更新错误信息
	 @discussion
	    方法执行完之后，调用[didUpdatePushOptions:error:];
	 */
	- (void)asyncUpdatePushOptions:(EMPushNotificationOptions *)options
	                         error:(EMError **)pError;
	
	/*!
	 @method
	 @brief 更新消息推送相关属性配置(异步方法)
	 @param options    属性
	 @param completion 回调
	 @param aQueue     回调时的线程
	 @result
	 */
	- (void)asyncUpdatePushOptions:(EMPushNotificationOptions *)options
	                    completion:(void (^)(EMPushNotificationOptions *options, EMError *error))completion
	                       onQueue:(dispatch_queue_t)aQueue;
	                       
调用示例

	EMPushNotificationOptions *options = [EMPushNotificationOptions alloc] init];
	options.displayStyle = ePushNotificationDisplayStyle_simpleBanner;
	[[EaseMob sharedInstance].chatManager asyncUpdatePushOptions:options error:nil];


---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##群聊:

###1.收发消息及聊天记录相关等
这部分与单聊是一样的，详情见单聊[http://developer.easemob.com/docs/emchat/ios/singlechat.html](http://developer.easemob.com/docs/emchat/ios/singlechat.html)

###2.新建群聊

接口

	/*!
	 @method
	 @brief 创建一个私有群组
	 @param subject        主题
	 @param description    说明信息
	 @param invitees       初始群组成员的用户名列表
	 @param welcomeMessage 对初始群组成员的邀请信息
	 @param pError         错误信息
	 @result 创建的群组对象, 失败返回nil
	 */
	- (EMGroup *)createPrivateGroupWithSubject:(NSString *)subject
	                               description:(NSString *)description
	                                  invitees:(NSArray *)invitees
	                     initialWelcomeMessage:(NSString *)welcomeMessage
	                                     error:(EMError **)pError;

	/*!
	 @method
	 @brief 异步方法, 创建一个私有群组
	 @param subject        主题
	 @param description    说明信息
	 @param invitees       初始群组成员的用户名列表
	 @param welcomeMessage 对初始群组成员的邀请信息
	 @discussion
	 函数执行完, 回调group:didJoinWithError:会被触发
	 */
	- (void)asyncCreatePrivateGroupWithSubject:(NSString *)subject
	                               description:(NSString *)description
	                                  invitees:(NSArray *)invitees
	                     initialWelcomeMessage:(NSString *)welcomeMessage;

	/*!
	 @method
	 @brief 异步方法, 创建一个私有群组
	 @param subject        主题
	 @param description    说明信息
	 @param invitees       初始群组成员的用户名列表
	 @param welcomeMessage 对初始群组成员的邀请信息
	 @param completion     消息完成后的回调
	 @param aQueue         回调block时的线程
	 */
	- (void)asyncCreatePrivateGroupWithSubject:(NSString *)subject
	                               description:(NSString *)description
	                                  invitees:(NSArray *)invitees
	                     initialWelcomeMessage:(NSString *)welcomeMessage
	                                completion:(void (^)(EMGroup *group,
	                                                     EMError *error))completion
	                                   onQueue:(dispatch_queue_t)aQueue;
	
调用示例

	[[EaseMob sharedInstance].chatManager asyncCreatePrivateGroupWithSubject:(群组标题) description:(群组描述) invitees:(群组初始化成员，不需要包括创建者) initialWelcomeMessage:(邀请信息) completion:^(EMGroup *group, EMError *error) {
	        //code
	    } onQueue:nil];

###3.群聊加人

接口

	/*!
	 @method
	 @brief 邀请用户加入群组
	 @param occupants      被邀请的用户名列表
	 @param groupId        群组ID
	 @param welcomeMessage 欢迎信息
	 @param pError         错误信息
	 @result 返回群组对象, 失败返回空
	 */
	- (EMGroup *)addOccupants:(NSArray *)occupants
	                  toGroup:(NSString *)groupId
	           welcomeMessage:(NSString *)welcomeMessage
	                    error:(EMError **)pError;

	/*!
	 @method
	 @brief 异步方法, 邀请用户加入群组
	 @param occupants      被邀请的用户名列表
	 @param groupId        群组ID
	 @param welcomeMessage 欢迎信息
	 @discussion
	        函数执行完, 回调group:didAddOccupants:welcomeMessage:error:会被触发
	 */
	- (void)asyncAddOccupants:(NSArray *)occupants
	                  toGroup:(NSString *)groupId
	           welcomeMessage:(NSString *)welcomeMessage;

	/*!
	 @method
	 @brief 异步方法, 邀请用户加入群组
	 @param occupants      被邀请的用户名列表
	 @param groupId        群组ID
	 @param welcomeMessage 欢迎信息
	 @param completion     消息完成后的回调
	 @param aQueue         回调block时的线程
	 */
	- (void)asyncAddOccupants:(NSArray *)occupants
	                  toGroup:(NSString *)groupId
	           welcomeMessage:(NSString *)welcomeMessage
	               completion:(void (^)(NSArray *occupants,
	                                    EMGroup *group,
	                                    NSString *welcomeMessage,
	                                    EMError *error))completion
	                  onQueue:(dispatch_queue_t)aQueue;
	
调用示例

	EMError *error;
	EMGroup *chatGroup = [[EaseMob sharedInstance].chatManager addOccupants:(被邀请的用户名列表) toGroup:(groupId) welcomeMessage:@"欢迎加入" error:&error];

###4.群聊减人

接口

	/*!
	 @method
	 @brief 将某人请出群组
	 @param occupant 要请出群组的人的用户名
	 @param groupId  群组ID
	 @param pError   错误信息
	 @result 返回群组对象
	 @discussion
	        此操作需要admin/owner权限
	 */
	- (EMGroup *)removeOccupant:(NSString *)occupant
	                  fromGroup:(NSString *)groupId
	                      error:(EMError **)pError;

	/*!
	 @method
	 @brief 异步方法, 将某人请出群组
	 @param occupant 要请出群组的人的用户名
	 @param groupId  群组ID
	 @discussion
	        此操作需要admin/owner权限.
	        函数执行完, group:didRemoveOccupant:error:回调会被触发
	 */
	- (void)asyncRemoveOccupant:(NSString *)occupant
	                  fromGroup:(NSString *)groupId;

	/*!
	 @method
	 @brief 异步方法, 将某人请出群组
	 @param occupant   要请出群组的人的用户名
	 @param groupId    群组ID
	 @param completion 消息完成后的回调
	 @param aQueue     回调block时的线程
	 @discussion
	        此操作需要admin/owner权限
	 */
	- (void)asyncRemoveOccupant:(NSString *)occupant
	                  fromGroup:(NSString *)groupId
	                 completion:(void (^)(EMGroup *group,
	                                      EMError *error))completion
	                    onQueue:(dispatch_queue_t)aQueue;

调用示例

	[[EaseMob sharedInstance].chatManager asyncRemoveOccupant:(要请出群组的人的用户名) fromGroup:(groupId) completion:^(EMGroup *group, EMError *error) {
	                        //code
	                    } onQueue:nil];

###5.退出群聊

接口

	/*!
	 @method
	 @brief 退出群组
	 @param groupId  群组ID
	 @param pError   错误信息
	 @result 退出的群组对象, 失败返回nil
	 */
	- (EMGroup *)leaveGroup:(NSString *)groupId
	                  error:(EMError **)pError;

	/*!
	 @method
	 @brief 异步方法, 退出群组
	 @param groupId  群组ID
	 @discussion
	 函数执行完, 回调group:didLeave:error:会被触发
	 */
	- (void)asyncLeaveGroup:(NSString *)groupId;

	/*!
	 @method
	 @brief 异步方法, 退出群组
	 @param groupId  群组ID
	 @param completion 消息完成后的回调
	 @param aQueue     回调block时的线程
	 */
	- (void)asyncLeaveGroup:(NSString *)groupId
	             completion:(void (^)(EMGroup *group,
	                                  EMGroupLeaveReason reason,
	                                  EMError *error))completion
	                onQueue:(dispatch_queue_t)aQueue;
	
调用示例

	[[EaseMob sharedInstance].chatManager asyncLeaveGroup:(groupId) completion:^(EMGroup *group, EMGroupLeaveReason reason, EMError *error) {
	        //code
	    } onQueue:nil];

###6.解散群聊

**需要创建者权限**

接口及用法同<5、退出群聊>，SDK中会判断调用者是否是创建者，如果是则解散群组。

###7.获取群聊列表

接口

	/*!
	 @method
	 @brief 获取所有与自己相关的群组列表
	 @param pError 错误信息
	 @result 群组对象列表
	 */
	- (NSArray *)fetchAllGroupsWithError:(EMError **)pError;

	/*!
	 @method
	 @brief 异步方法, 获取所有与自己相关的群组列表
	 @discussion
	        执行后, 回调difFetchAllGroups:error:会被触发
	 */
	- (void)asyncFetchAllGroups;

	/*!
	 @method
	 @brief 异步方法, 获取所有与自己相关的群组列表
	 @param completion 消息完成后的回调
	 @param aQueue     回调block时的线程
	 */
	- (void)asyncFetchAllGroupsWithCompletion:(void (^)(NSArray *groups,
	                                                    EMError *error))completion
	                                  onQueue:(dispatch_queue_t)aQueue;
	
调用示例

	[[EaseMob sharedInstance].chatManager asyncFetchAllGroupsWithCompletion:^(NSArray *groups, EMError *error) {
	        //code
	    } onQueue:nil];

###8.群聊事件监听

**前提条件**

注册为 [EaseMob sharedInstance].chatManager 的代理

接口

	/*!
	 @method
	 @brief 创建一个群组后的回调
	 @param group 所创建的群组对象
	 @param error 错误信息
	 @discussion
	*/
	- (void)group:(EMGroup *)group didCreateWithError:(EMError *)error;
	
	/*!
	 @method
	 @brief 离开一个群组后的回调
	 @param group  所要离开的群组对象
	 @param reason 离开的原因
	 @param error  错误信息
	 @discussion
	        离开的原因包含主动退出, 被别人请出, 和销毁群组三种情况
	 */
	- (void)group:(EMGroup *)group didLeave:(EMGroupLeaveReason)reason error:(EMError *)error;

	/*!
	 @method
	 @brief 群组信息更新后的回调
	 @param group 发生更新的群组
	 @param error 错误信息
	 @discussion
	        当添加/移除/更改角色/更改主题/更改群组信息之后,都会触发此回调
	 */
	- (void)groupDidUpdateInfo:(EMGroup *)group error:(EMError *)error;

	/*!
	 @method
	 @brief 把人加入到群组后的回调
	 @param group          加入的群组
	 @param occupants      被加的用户名列表
	 @param welcomeMessage 欢迎消息
	 @param error          错误信息
	 */
	- (void)group:(EMGroup *)group didAddOccupants:(NSArray *)occupants welcomeMessage:(NSString *)welcomeMessage error:(EMError *)error;

	/*!
	 @method
	 @brief 收到了其它群组的加入邀请
	 @param groupId  群组ID
	 @param username 邀请人名称
	 @param message  邀请信息
	 @discussion
	 */
	- (void)didReceiveGroupInvitationFrom:(NSString *)groupId
	                              inviter:(NSString *)username
	                              message:(NSString *)message;

	/*!
	 @method
	 @brief 接受群组邀请后的回调
	 @param group 所接受的群组
	 @param error 错误信息
	 */
	- (void)didAcceptInvitationFromGroup:(EMGroup *)group
	                               error:(EMError *)error;

	/*!
	 @method
	 @brief 邀请别人加入群组, 但被别人拒绝后的回调
	 @param groupId  群组ID
	 @param username 拒绝的人的用户名
	 @param reason   拒绝理由
	 */
	- (void)didReceiveGroupRejectFrom:(NSString *)groupId
	                          invitee:(NSString *)username
	                           reason:(NSString *)reason;

	/*!
	 @method
	 @brief 群组列表变化后的回调
	 @param groupList 新的群组列表
	 */
	- (void)didUpdateGroupList:(NSArray *)groupList;

	/*!
	 @method
	 @brief 获取群组信息后的回调
	 @param group 群组对象
	 @param error 错误信息
	 */
	- (void)didFetchGroupInfo:(EMGroup *)group
	                    error:(EMError *)error;

	/*!
	 @method
	 @brief 获取所有与自己相关的群组列表后的回调
	 @param groups 获得的与自己相关的群组列表(创建和加入的)
	 @param error  错误信息
	 */
	- (void)didFetchAllGroups:(NSArray *)groups
	                    error:(EMError *)error;
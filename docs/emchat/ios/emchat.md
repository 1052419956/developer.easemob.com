---
title: Easemob Chat 快速入门
description: Easemob Chat 快速入门
category: push
layout: docs
---
#易聊 API
#功能
#API使用详解
##1.初始化
初始化及启动EaseMobService以便接收消息。
>
EMClientSDK *sdk = [EMClientSDK sharedInstance];
>
[sdk startEngine];

##2.接收消息 
>
\#import "XMPPDelegateManager.h"  
@interface ViewController () <XMPPChatProtocol>  
@end
<br/>
<br/>
>
@implementation ViewController  
-(void)viewDidload{  
[super viewDidload];  
//将self注册到Delegate中，以便收到消息时回调 -(void)didReceiveMessage:(ChatData *)message 函数。  
[[XMPPDelegateManager sharedInstance] addChatDelegate:self delegateQueue:dispatch_get_main_queue()];  
}
<br/>
<br/> 
>\#pragma mark - chat delegate  
-(void)didReceiveMessage:(ChatData *)message {  
// message.type 是一个枚举，里面包含类型如下： 根据不同的值，可以知道当前的message的类型，从而得到对应数据。  
// eMessageType_Unknown = -1,   未知类型  
// eMessageType_Text = 1,       文本及表情  
// eMessageType_Image,          图片  
// eMessageType_Video,          视频  
// eMessageType_Location,       位置  
// eMessageType_Audio,          语音  
// eMessageType_File = 11       文件  
NSString *body = message.body;  
NSString *from = message.from;  
NSString *to = message.to;  
NSString *title = message.title;  
NSDate *date = [NSDate 
        dateWithTimeIntervalInMilliSecondSince1970:
        (NSTimeInterval)message.time];  
        //你自己的代码处理收到的消息  
        }
        

##3.发送消息
###3.1 发送文本及消息表情
>  
/**  
 发送文本消息的函数  
  @param text 发送的文本或表情  
  @param receiverJID 接收方jid  
  @returns the chatdata 将要发送的chatData  
  @comments a callback block 发送完成后返回的chatData  
  */  
  ChatData * chatdata = [[XMPPManager sharedInstance] asyncSendTextMessage:text  
  toReceiver:receiverJID  
  completion:^(ChatData *chatdata){  
  }onQueue:dispatch_get_main_queue()];
  
###3.2 发送语音消息
>/**  
send audio message asyncronizely  
@param filename 音频文件所在路径.  
@param duration 音频文件时长.  
@param receiverJID 接收方jid.  
@param progress 发送进度.  
@returns 要发送的ChatData  
@comments a callback block 发送完成后返回的chatData  
 */  
 ChatData *chatdata = [[XMPPManager sharedInstance] asyncSendAudioMessageWithAudioFile:filename  
  duration:duration  
  toReceiver:receiverJID  
  progress:nil   
  completion:^(ChatData *chatdata){  
  } onQueue:nil];
  
###3.3 发送图片消息
>  
/**  
发送图片  
@param image 发送的图片  
@param imagename 图片名称  
@param receiverJID 接收方jid  
@param progress 发送进度.  
@returns 要发送的ChatData  
@comments a callback block 发送完成后返回的chatData  
*/  
ChatData *chatdata =[[XMPPManager sharedInstance] asyncSendImageMessageWithImage:image  
imageName:imagename  
toReceiver:receiverJID  
progress:nil  
 completion:^(ChatData *chatdata){  
 } onQueue:nil];
 
###3.4 发送地理位置
>/**    
发送位置信息  
@param latitude 纬度  
@param longitude 经度  
@param address the 经纬度对应位置  
@param receiverJID 接收方jid  
@returns the chatdata 将要发送的chatData  
@comments a callback block 发送完成后返回的chatData  
*/  
ChatData *chatData = [[XMPPManager sharedInstance] asyncSendLocationMessageWithLatitude:latitude  
andLongitude:longitude  
andAddress:address  
toReceiver:receiverJID  
completion:^(ChatData *chatdata){  
} onQueue:dispatch_get_main_queue()];

##4.获取好友列表，详细资料，及头像
###4.1 获取用户列表
>/**   
从服务器获取最新用户列表    
@returns none.   
 @comments a callback block:返回得到的用户列表，以及是否获取成功    
 */  
 [[ContactManager sharedInstance]  asyncUpdateContactListWithCompletion:\^(NSArray *contactAry, BOOL success){  
  } onQueue:dispatch_get_main_queue()];

###4.2 修改用户信息
>/**  
 修改用户信息 （昵称，邮箱）  
 @param username 用户名  
 @param attributes  对应属性的值  
 @param keys 对应attributes的key  
 @return none.  
 @comments a callback block 返回user和是否修改成功  
 */  
 [[ContactManager sharedInstance]  
 asyncUpdateContact:username  
 byAttributes:@[@"我"，@"www.XXXX@XX.com"]  
  andKeys:@[nick,email]  
  completion:\^(EMUser *user, BOOL success){  
  } onQueue:dispatch_get_main_queue()];

---
title: 易聊
description: 5分钟为你的APP加入完整的微信聊天功能
category: emchat
layout: docs
---

##单聊:

### 1.初始化易聊SDK
	EaseMobChat.getInstance().init(getApplicationContext());

### 2.登陆
    EMUserManager.getInstance().login(userName, password, callback);

### 3.退出登陆
	EMUserManager.getInstance().logout();

### 4.发送消息
#### 4.1发送文本消息及表情 ####

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
 	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.TXT);
    TextMessageBody txtBody = new TextMessageBody(content);
    message.addBody(txtBody);
	message.setReceipt(username);
	conversation.addMessage(message);


#### 4.2 发送语音消息 ####

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.VOICE);
	VoiceMessageBody body = new VoiceMessageBody(new File(filePath), len);
    message.addBody(body);
	message.setReceipt(username);
	conversation.addMessage(message);


#### 4.3 发送图片消息 ####

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.IMAGE);
	ImageMessageBody body = new ImageMessageBody(new File(filePath));
    message.addBody(body);
	message.setReceipt(username);
	conversation.addMessage(message);


#### 4.4 发送地理位置消息 ####

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.LOCATION);
    LocationMessageBody locBody = new LocationMessageBody(locationAddress, latitude, longitude);
    message.addBody(locBody);
	message.setReceipt(username);
    conversation.addMessage(message);

### 5.接收消息 ###
	注册一个相应broadcast，用来接收消息
	NewMessageBroadcastReceiver msgReceiver = new NewMessageBroadcastReceiver();
	IntentFilter intentFilter = new IntentFilter(EMChatManager.getInstance().getNewMessageBroadcastAction());
    intentFilter.setPriority(3);
    registerReceiver(msgReceiver, intentFilter);
	
	private class NewMessageBroadcastReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            Log.d("TAG", "new message received");
                    
            }
    }

### 6.获取聊天记录 ###
	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	List<EMMessage> messages = conversation.getMessages();

### 7.删除聊天记录 ###
   //删除和某个user的整个的聊天记录
   EMChatManager.getInstance().deleteConversation(username);
   //删除当前会话的某条聊天记录
   conversation.removeMessage(deleteMsg.msgId);
	 


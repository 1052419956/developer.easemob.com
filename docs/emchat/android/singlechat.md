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

### 4.发送消息 ###
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
            //消息id
			String msgId = intent.getStringExtra("msgid");
			//发消息的人的username(userid)
			String msgFrom = intent.getStringExtra("from");
			//消息类型，文本，图片，语音消息等,这里返回的值为msg.type.ordinal()。
			//所以消息type实际为是enum类型
			int msgType = intent.getIntExtra("type", 0);
			//消息body，为一个json字符串
			String msgBody = intent.getStringExtra("body");
			Log.d("main", "new message id:" + msgId + " from:" + msgFrom + " type:" + msgType + " body:" + msgBody);
			
			//更方便的方法是通过msgId直接获取整个message
            EMMessage message = EMChatManager.getInstance().getMessage(msgId);
                    
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
	 


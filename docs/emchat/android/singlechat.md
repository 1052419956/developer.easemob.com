---
title: 易聊
description: 易聊SDK,5分钟为你的APP加入完整的微信聊天功能
category: chat
layout: docs
---

##API:

###1.初始化chat sdk
	EaseMobChat.getInstance().init(getApplicationContext());

###2.登陆及退出登陆
	 EMUserManager.getInstance().login(userName, password, callback);

###3.发送消息
3.1发送文本消息及表情

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
 	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.TXT);
    TextMessageBody txtBody = new TextMessageBody(content);
    message.addBody(txtBody);
	message.setReceipt(username);
	conversation.addMessage(message);


3.2 发送语音消息

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.VOICE);
	VoiceMessageBody body = new VoiceMessageBody(new File(filePath), len);
    message.addBody(body);
	message.setReceipt(username);
	conversation.addMessage(message);


3.3 发送图片消息

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.IMAGE);
	ImageMessageBody body = new ImageMessageBody(new File(filePath));
    message.addBody(body);
	message.setReceipt(username);
	conversation.addMessage(message);


3.4 发送地理位置消息

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.LOCATION);
    LocationMessageBody locBody = new LocationMessageBody(locationAddress, latitude, longitude);
    message.addBody(locBody);
	message.setReceipt(username);
    conversation.addMessage(message);


####4.接收消息
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


####5.获取聊天记录
	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	List<EMMessage> messages = conversation.getMessages()


####6.删除聊天记录
	 EMChatDB.getInstance().deleteConversions(username);
     conversation.clear();


####7.退出登陆
	EMUserManager.getInstance().logout();
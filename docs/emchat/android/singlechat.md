title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##单聊:

### 初始化环信聊天SDK

建议在application中初始化

	EMChat.getInstance().init(getApplicationContext());

### 登陆聊天服务器

    EMChatManager.getInstance().login(userName,password,
    	new EMCallBack() {//回调
    		@Override
    		public void onSuccess() {
    			runOnUiThread(new Runnable() {
    				public void run() {
    					Log.d("main", "登陆聊天服务器成功！");
    				}
    			});
    		}

    		@Override
    		public void onProgress(int progress, String status) {

    		}

    		@Override
    		public void onError(int code, String message) {
    			Log.d("main", "登陆聊天服务器失败！");
    		}
    	});

### 退出聊天登陆

	EMChatManager.getInstance().logout();

### 发送消息(单聊/群聊)

#### 发送文本消息及表情

	//获取到与聊天人的会话对象。参数username为聊天人的userid或者groupid，后文中的username皆是如此
	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	//创建一条文本消息
    EMMessage message = EMMessage.createSendMessage(EMMessage.Type.TXT);
	//如果是群聊，设置chattype,默认是单聊
	message.setChatType(ChatType.GroupChat);
	//设置消息body
    TextMessageBody txtBody = new TextMessageBody(content);
    message.addBody(txtBody);
	//设置接收人
	message.setReceipt(username);
	//把消息加入到此会话对象中
	conversation.addMessage(message);
	//发送消息
	EMChatManager.getInstance().sendMessage(message, new EMCallBack());


#### 发送语音消息

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.VOICE);
	//如果是群聊，设置chattype,默认是单聊
	message.setChatType(ChatType.GroupChat);
	VoiceMessageBody body = new VoiceMessageBody(new File(filePath), len);
    message.addBody(body);
	message.setReceipt(username);
	conversation.addMessage(message);
	EMChatManager.getInstance().sendMessage(message, new EMCallBack());


#### 发送图片消息

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.IMAGE);
	//如果是群聊，设置chattype,默认是单聊
	message.setChatType(ChatType.GroupChat);
	ImageMessageBody body = new ImageMessageBody(new File(filePath));
    message.addBody(body);
	message.setReceipt(username);
	conversation.addMessage(message);
	EMChatManager.getInstance().sendMessage(message, new EMCallBack());


#### 发送地理位置消息

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	EMMessage message = EMMessage.createSendMessage(EMMessage.Type.LOCATION);
	//如果是群聊，设置chattype,默认是单聊
	message.setChatType(ChatType.GroupChat);
    LocationMessageBody locBody = new LocationMessageBody(locationAddress, latitude, longitude);
    message.addBody(locBody);
	message.setReceipt(username);
    conversation.addMessage(message);
	EMChatManager.getInstance().sendMessage(message, new EMCallBack());

### 接收消息

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

### 获取聊天记录

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	//获取此会话的所有消息
	List<EMMessage> messages = conversation.getAllMessages();
	//sdk初始化加载的聊天记录为20条，到顶时需要去db里获取更多
	//获取startMsgId之前的pagesize条消息，此方法获取的messages sdk会自动存入到此会话中，app中无需再次把获取到的messages添加到会话中
	List<EMMessage> messages = conversation.loadMoreMsgFromDB(startMsgId, pagesize);
	//如果是群聊，调用下面此方法
	List<EMMessage> messages = conversation.loadMoreGroupMsgFromDB(startMsgId, pagesize);
	

### 获取未读消息数量

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	conversation.getUnreadMsgCount();

### 未读消息数清零(指定会话消息未读数清零) 

	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
	conversation.resetUnsetMsgCount();

### 清空会话聊天记录

	//清空和某个user的聊天记录，不删除整个会话
	EMChatManager.getInstance().clearConversation(username);


### 删除聊天记录

    //删除和某个user的整个的聊天记录
    EMChatManager.getInstance().deleteConversation(username);
    //删除当前会话的某条聊天记录
	EMConversation conversation = EMChatManager.getInstance().getConversation(username);
    conversation.removeMessage(deleteMsg.msgId);

### 新消息提示
SDK中提供了方便的新消息提醒功能。可以在收到消息时调用，提醒用户有新消息

首先获取EMChatOptions  
    
    chatOptions = EMChatManager.getInstance().getChatOptions();

设置是否启用新消息提醒 

    chatOptions.setNotificationEnable(true|false); //默认为true 开启新消息提醒
    
设置是否启用新消息声音提醒 

    chatOptions.setNoticeBySound(true|false); //默认为true 开启声音提醒
    
设置是否启用新消息震动提醒 
    
    chatOptions.setNoticedByVibrate(true|false); //默认为true 开启震动提醒
    
设置语音消息播放是否设置为扬声器播放

	chatOptions.setUseSpeaker(true|false); //默认为true 开启扬声器播放

附：

    chatOptions.setAcceptInvitationAlways(false); //默认添加好友时为true，是不需要验证的，改成需要验证为false)
	

	

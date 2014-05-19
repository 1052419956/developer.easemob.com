---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##好友列表管理

### 1.获取好友列表
	//获取好友的usernam list，开发者需要根据username去自己服务器获取好友的详情
	List<String> usernames = EMChatManager.getInstance().getContactUserNames();

### 2.添加好友
	//参数为要添加的好友的username和添加理由
	EMContactManager.getInstance().addContact(toAddUsername, reason);
	
### 3.删除好友
	EMContactManager.getInstance().deleteContact(username);

### 4.同意好友请求
	//同意username的好友请求
	EMChatManager.getInstance().acceptInvitation(username);

### 5.监听好友请求，同意好友请求等事件
	//注册一个好友请求等的BroadcastReceiver
	IntentFilter inviteIntentFilter = new IntentFilter(EMChatManager.getInstance().getContactInviteEventBroadcastAction());
	registerReceiver(contactInviteReceiver, inviteIntentFilter);
	
	private BroadcastReceiver contactInviteReceiver = new BroadcastReceiver(){

		@Override
		public void onReceive(Context context, Intent intent) {
			//请求理由
			final String reason = intent.getStringExtra("reason");
			final boolean isResponse = intent.getBooleanExtra("isResponse", false);
			//消息发送方username
			final String from = intent.getStringExtra("username");
		}
	}

### 6.监听好友列表变化
	EMContactManager.getInstance().setContactListener(new EMContactListener() {
			
		@Override
		public void onContactDeleted(List<String> usernameList) {
			
			
		}
		
		@Override
		public void onContactAdded(List<String> usernameList) {
			
			
		}
	});
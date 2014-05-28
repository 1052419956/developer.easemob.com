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
			//sdk暂时只提供同意好友请求方法，不同意选项可以参考微信增加一个忽略按钮。
			if(!isResponse){
				Log.d(TAG, from + "请求加你为好友,reason: " + reason);
			}else{
				Log.d(TAG, from + "同意了你的好友请求");
			}
			//具体ui上的处理参考chatuidemo。
		}
	}

### 6.监听好友列表变化
	EMContactManager.getInstance().setContactListener(new EMContactListener() {
			
		@Override
		public void onContactDeleted(List<String> usernameList) {
			//好友删除的回调
			
		}
		
		@Override
		public void onContactAdded(List<String> usernameList) {
			//好友增加的回调
			//返回的usernameList可能在原来的好友列表中已经有了，
			//使用的时候务必判断原来的列表中是否已经包含回调中的好友。
		}
	});
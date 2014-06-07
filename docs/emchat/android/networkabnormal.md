---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##网络异常监听:
在聊天过程中难免会遇到网络问题，在此SDK为您提供了网络监听接口，实时监听


	//注册一个监听连接状态的listener
	EMChatManager.getInstance().addConnectionListener(new MyConnectionListener());

	//实现ConnectionListener接口
	private class MyConnectionListener implements ConnectionListener{

		@Override
		public void onConnected() {
			
		}

		@Override
		public void onDisConnected(String errorString) {
			
		}

		@Override
		public void onReConnected() {
			
		}

		@Override
		public void onReConnecting() {
		}

		@Override
		public void onConnecting(String progress) {

		}
		
		
	}




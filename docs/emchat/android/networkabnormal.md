---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##网络异常监听:
1.在聊天过程中难免会遇到网络问题，在此SDK为您提供了网络监听接口，实时监听

2.对于同一个账号在多处登录，则根据本监听事件中的onDisConnected方法传递的String类型参数errorString来进行判断是否同一个账号在其它地方进行了登录，若服务器返回的参数值为errorString!=null&&errorString.contains("conflict")，则认为是有同一个账号异地登录


	//注册一个监听连接状态的listener
	EMChatManager.getInstance().addConnectionListener(new MyConnectionListener());

	//实现ConnectionListener接口
	private class MyConnectionListener implements ConnectionListener{

		@Override
		public void onConnected() {
			
		}

		@Override
		public void onDisConnected(String errorString) {
			if(errorString!=null&&errorString.contains("conflict"))
			{
				//收到帐号在其他手机登录
				// TODO 
			}else{
				
				//"连接不到聊天服务器"
			}
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




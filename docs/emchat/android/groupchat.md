---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##群聊:

###1.收发消息及聊天记录相关等
这部分与单聊是一样的，详情见单聊[http://developer.easemob.com/docs/emchat/android/singlechat.html](http://developer.easemob.com/docs/emchat/android/singlechat.html)

###2.新建群聊
	//groupName：要创建的群聊的名称
	//desc：群聊简介
	//members：群聊成员,为空时这个创建的群组只包含自己
	EMGroupManager.getInstance().createGroup(groupName, desc, members);

###3.群聊加人
	EMGroupManager.getInstance().addUsersToGroup(groupId, newmembers);

###4.群聊减人
	//把username从群聊里删除
	EMGroupManager.getInstance().removeUserFromGroup(groupId, username);

###5.退出群聊
	EMGroupManager.getInstance().exitFromGroup(groupId);

###6.解散群聊
	EMGroupManager.getInstance().exitAndDeleteGroup(groupId);

###7.获取群聊列表
	//从服务器获取自己加入的和创建的群聊列表
	EMGroupManager.getInstance().getGroupsFromServer();

###8.群聊事件监听
	EMGroupManager.getInstance().addGroupChangeListener(new GroupChangeListener() {
				
				@Override
				public void onUserRemoved(String groupId, String groupName) {
					//当前用户被管理员移除出群聊
				}
				
				@Override
				public void onInvitationReceived(String groupId, String groupName, String inviter, String reason) {
					//收到加入群聊的邀请
				}
				
				@Override
				public void onInvitationDeclined(String groupId, String invitee, String reason) {
					//群聊邀请被拒绝
				}
				
				@Override
				public void onInvitationAccpted(String groupId, String inviter, String reason) {
					//群聊邀请被接受
				}
				
				@Override
				public void onGroupDestroy(String groupId, String groupName) {
					//群聊被创建者解散
				}
			});
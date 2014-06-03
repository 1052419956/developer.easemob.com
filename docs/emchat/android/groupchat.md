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

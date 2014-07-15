---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

##群聊:

### 收发消息及聊天记录相关等

这部分与单聊是一样的，详情见[单聊](http://developer.easemob.com/docs/emchat/android/singlechat.html)

### 新建群聊
####创建私有群
私有群，不能被搜索到，只能通过群主加人进群，或者设置了allowInvite为true，即允许群成员邀请，那么群成员也可以邀请群外面的人进入群聊，此种群群成员可以邀请，不能踢人，类似微信群。
	//groupName：要创建的群聊的名称
	//desc：群聊简介
	//members：群聊成员,为空时这个创建的群组只包含自己
	//allowInvite:是否允许群成员邀请人进群
	EMGroupManager.getInstance().createPrivateGroup(groupName, desc, members,allowInvite);

####创建公开群 
公开群可以被用户搜索到，并且可以直接加入或者申请加入

	//前面三个参数和创建私有群一致
	//needApprovalRequired:如果创建的公开群用需要户自由加入，就传false。否则需要申请，等群主批准后才能加入，传true
	EMGroupManager.getInstance().createPublicGroup(groupName, desc, members, needApprovalRequired);
	

### 群聊加人
	//群主加人调用此方法
	EMGroupManager.getInstance().addUsersToGroup(groupId, newmembers);
	//私有群里，如果开放了群成员邀请，群成员邀请调用下面方法
	EMGroupManager.getInstance().inviteUser(groupId, newmembers, null);

### 群聊减人

	//把username从群聊里删除
	EMGroupManager.getInstance().removeUserFromGroup(groupId, username);

### 加入某个群聊
只能用于加入公开群

	//如果群开群是自由加入的，即group.isMembersOnly()为false，直接join
	EMGroupManager.getInstance().joinGroup(groupid);
	//需要申请和验证才能加入的，即group.isMembersOnly()为true，调用下面方法
	EMGroupManager.getInstance().applyJoinToGroup(groupid, "求加入");
### 退出群聊

	EMGroupManager.getInstance().exitFromGroup(groupId);

### 解散群聊
	EMGroupManager.getInstance().exitAndDeleteGroup(groupId);

### 获取群聊列表

	//从服务器获取自己加入的和创建的群聊列表
	List<EMGroup> grouplist = EMGroupManager.getInstance().getGroupsFromServer();

	//从本地加载群聊列表，节省了每次从服务器加载数据的时间
	List<EMGroup> grouplist = EMGroupManager.getInstance().getAllGroups();

	//获取所有公开群列表
	List<EMGroupInfo> groupsList = EMGroupManager.getInstanc().getAllPublicGroupsFromServer();


### 群聊事件监听

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
		@Override
		public void onApplicationReceived(String groupId, String groupName, String applyer, String reason) {
			//收到加群申请
			
		}

		@Override
		public void onApplicationAccept(String groupId, String groupName, String accepter) {
			//加群申请被同意
			
		}

		@Override
		public void onApplicationDeclined(String groupId, String groupName, String decliner, String reason) {
			// 加群申请被拒绝
			
		}
	});

### 获取群组信息
	//根据群组ID从本地获取群组信息
	EMGroup group = EMGroupManager.getInstance().getGroup(groupId);
	//根据群组ID从服务器获取群组信息
	EMGroup group =EMGroupManager.getInstance().getGroupFromServer(groupId);

	group.getMembers();//获取群成员
	group.getOwner();//获取群主
    ...
	其它方法详见环信接口文档


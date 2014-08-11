---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

## 黑名单

### 获取黑民单列表

	//获取黑名单用户的usernames
	EMContactManager.getInstance().getBlackListUsernames();


### 把用户加入到黑民单
	
	//第二个参数如果为true，则把用户加入到黑名单后双方发消息时对方都收不到；false,则
	//我能给黑名单的中用户发消息，但是对方发给我时我是收不到的
    EMContactManager.getInstance().addUserToBlackList(username,true);

### 把用户从黑名单中移除
	
	EMContactManager.getInstance().deleteUserFromBlackList(username);
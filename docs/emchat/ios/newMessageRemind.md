---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

## 新消息提示 
SDK中提供了方便的新消息提醒功能。可以在收到消息时调用，提醒用户有新消息。


调用播放音频

	// 播放音频
    [[EaseMob sharedInstance].deviceManager asyncPlayNewMessageSound];


调用手机震动

	// 震动
    [[EaseMob sharedInstance].deviceManager asyncPlayVibration];
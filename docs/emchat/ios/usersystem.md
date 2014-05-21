---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

# EaseMob集成已有用户体系流程
**SDK使用[易用户SDK](#{site.base_url}/docs/emuser/whatisemuser.html)作为缺省用户体系，也支持使用自建用户体系或其他第三方用户体系**

**本文档主要用于描述怎样集成已有的用户体系，包括登录用户体系、退出用户体系、添加好友、接受好友请求、移除好友等操作**

## 登录已有的用户体系
**SDK不提供登录已有用户体系的接口，使用SDK的工程需在代码中添加登录已有用户体系的方法，成功后再进行SDK的登录**

## 退出集成的用户体系
**SDK不提供退出已有用户体系的接口，使用SDK的工程需在代码中添加退出已有用户体系的方法。先退出SDK，成功后调用用户体系的退出方法**

## 操作好友列表

**[获取好友列表，监听好友列表](#{site.base_url}/docs/emchat/ios/buddylist.html)**
	
## 操作好友

**[根据账号 （查找，添加，删除，黑名单）](#{site.base_url}/docs/emchat/ios/buddymanager.html)**


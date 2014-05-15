---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

# 用户管理REST API

# 1. 用户的json数据结构说明
	{
		username: "jliu",  //username是用户的primarykey。需要在appkey的范围内唯一。
		password:"123456", 密码
	}

# 2. 用户管理REST API
## 2.1 创建用户 ##

###POST /${orgName}/${appName}/users

- 描述：在指定的org和app中创建一个新的用户
- 权限： 创建用户是不需要授权的。所以不需要传入token。
- url参数： 无
- request body： 要创建的用户，json格式。见用户的数据结构。
- response： 创建成功的用户，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X POST -i "http://a1.easemob.com/easemob-demo/chatdemo/users" -d '{"username":"jliu","password":"123456"}'

#### response返回:
		
		{
		"action" : "post",
		"application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
		"params" : { },
		"path" : "/users",
		"uri" : "http://a1.easemob.com/easemob-demo/chatdemo/users",
		"entities" : [ {
			"uuid" : "7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
			"type" : "user",
			"name" : "jliu0003",
			"created" : 1368377620796,
			"modified" : 1368377620796,
			"username" : "jliu0003",
			"activated" : true,			
			"metadata" : {
				"path" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
				"sets" : {
					"rolenames" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/rolenames",
					"permissions" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/permissions"
				},
				"collections" : {
					"activities" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/activities",
					"devices" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/devices",
					"feed" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/feed",
					"groups" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/groups",
					"roles" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/roles",
					"following" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/following",
    				"followers" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/followers"
      			}
    		}
      	} ],
      	"timestamp" : 1368377620793,
      	"duration" : 125,
      	"organization" : "easemob-demo",
      	"applicationName" : "chatdemo"
		}
  


## 2.2 获取指定用户详情 ##

###GET /${orgName}/${appName}/users/${username}

- 描述：获取app的指定用户详情
- 权限：app用户级别登录
- url参数：无
- request body： 无
- response： 指定的用户详情，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://a1.easemob.com/easemob-demo/chatdemo/users/jliu1"

## 2.3 重置用户密码 ##

### PUT /${orgName}/${appName}/users/${username}/password

- 描述： 重置用户密码
- 权限：app用户级别登录
- url参数：无
- request body： 新密码
- response： 更新后的用户信息，json格式。见用户的数据结构。
- 错误代码：
 
#### curl示例：
		
	curl -X PUT -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "http://a1.easemob.com/easemob-demo/chatdemo/users/jliu/password" -d '{"newpassword" : "newpwd"}'



## 2.4 删除用户 ##

### DELETE /${orgName}/${appName}/users/${username}
- 描述：删除app的指定用户
- 权限：app用户级别登录
- url参数：无
- request body： 无
- response： 无
- 错误代码：

#### curl示例：
		
	curl -X DELETE -i -H "Authorization: Bearer YWMtP_8IisA-EeK-a5cNq4Jt3QAAAT7fI10IbPuKdRxUTjA9CNiZMnQIgk0LEUE" "http://a1.easemob.com/easemob-demo/chatdemo/users/jliu"
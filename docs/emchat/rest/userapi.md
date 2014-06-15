---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

# 用户管理REST API

## 用户的json数据结构说明
	{
	  username: "jliu",  //username是用户的primarykey。需要在appkey的范围内唯一。
	  password:"123456"//登录密码
	}

## 名词解释
    org_name: 是开发者在环信开发者后台注册账号时填写的公司Id。
    app_name: 是开发者在环信开发者后台创建app时填写的app名。
	公司管理员：是开发者在环信开发者后台注册的账号。公司管理员账号可以在自己账号下创建多个app。公司管理员拥有对该公司账号下所有app的操作权限。
	app管理员：每个app都可以创建app管理员（可选操作）。app管理员拥有app级别的操作权限。

## 用户管理REST API

以下所有API均需要公司管理员或app管理员权限才能访问。

强烈建议保护好公司管理员及app管理员的用户名和密码。尽量只在APP的服务器后台对环信用户做增删改查的管理，包括新用户注册。一定不要将公司管理员或app管理员的用户名和密码写死在手机客户端中。

### app管理员登录并获取授权token

#### POST /${org_name}/${app_name}/token

- 描述： 登录并授权，获得一个token。
- 权限：
- url参数： 无
- request body： 授权数据（json格式）。

		{
		  "grant_type":"password",
	  	  "username":"jliu1",
	  	  "password":"jliu1"
		}

- response： 授权结果(json),其中access_token为授权后的token
- 错误代码：

#### curl示例：

>> 注：请将URL中的easemob-demo/chatdemo替换成你自己的org_name和app_name。并将"username","password"分别替换成你自己的用户名和密码
		
	curl -X POST "http://a1.easemob.com/easemob-demo/chatdemo/token" -d '{"grant_type":"password","username":"jliu1","password":"jliu1"}'
	
				
如果这个用户之前已经注册了, 并且这里提供的密码正确的话, response会返回
		
	{	  	"access_token":"YWMtNda4DFzyEeOrOy_LuVzHjAAAAULiG1IrN8opggpytUDFmJkiocawbINICYk",
		"expires_in":604800,
		"user":
        {
            "uuid":"1f3c832a-5cf1-11e3-b3c3-53fbf4b08789",
            "type":"user",
            "created":1386167643218,
            "modified":1386167643218,
            "username":"test1",
            "activated":true
        }
	}

从这个返回值中, 可以得到两部分信息:

1. token

     这个是服务器用来标识这个用户已经登陆的, 用户登陆后的所有操作(这里的操作指的是app访问服务器的request), 都需要把这个token加到header当中, 在本文档后面所有描述到得request都会有这个header   

		
        -H "Authorization: Bearer YWMtNda4DFzyEeOrOy_LuVzHjAAAAULiG1IrN8opggpytUDFmJkiocawbINICYk"

2. 用户的具体信息。见用户的数据结构


### 创建用户 分两种模式：自由注册 和 仅管理员可注册
	简要说明：创建一个用户需要注册到两个服务进行注册，一个是app开发者自己的服务器（完整数据），另一个是环信服务器（部分数据）。出于安全因素，环信服务器端进行操
	作时是需要身份认证的，为了方便开发者，这里使用两种用户创建模式：其中“自由注册”模式下，一个app向环信服务器端注册用户时不用携带管理员身份认证信息；“仅管理员
	可注册”模式下，一个app向环信服务器注册用户必须携带管理员身份认证信息。

## 自由注册 
### POST /${org_name}/${app_name}/users

- 描述：在指定的org和app中创建一个新的用户
- 权限： 无
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
			"name" : "jliu0003",
			"created" : 1368377620796,
			"modified" : 1368377620796,
			"username" : "jliu0003",
    		}
      	} ],
      	"timestamp" : 1368377620793,
      	"duration" : 125,
      	"organization" : "easemob-demo",
      	"applicationName" : "chatdemo"
		}
 
## 仅管理员可注册
### 先获取app管理员token ； 
详见3.1

### 创建用户
### POST /${org_name}/${app_name}/users

- 描述：在指定的org和app中创建一个新的用户
- 权限： app管理员或org管理员
- url参数： 无
- request body： 要创建的用户，json格式。见用户的数据结构。
- response： 创建成功的用户，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X POST -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://a1.easemob.com/easemob-demo/chatdemo/users" -d '{"username":"jliu","password":"123456"}'

#### response返回:
		
		{
		"action" : "post",
		"application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
		"params" : { },
		"path" : "/users",
		"uri" : "http://a1.easemob.com/easemob-demo/chatdemo/users",
		"entities" : [ {
			"uuid" : "7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
			"name" : "jliu0003",
			"created" : 1368377620796,
			"modified" : 1368377620796,
			"username" : "jliu0003",
    		}
      	} ],
      	"timestamp" : 1368377620793,
      	"duration" : 125,
      	"organization" : "easemob-demo",
      	"applicationName" : "chatdemo"
		}
  
注：返回的response json数据中会包含除上述属性之外的一些其他信息，均可以忽略。

### 获取指定用户详情

### GET /${org_name}/${app_name}/users/${username}

- 描述：获取app的指定用户详情
- 权限：app管理员或org管理员
- url参数：无
- request body： 无
- response： 指定的用户详情，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://a1.easemob.com/easemob-demo/chatdemo/users/jliu1"

### 重置用户密码

### PUT /${org_name}/${app_name}/users/${username}/password

- 描述： 重置用户密码
- 权限：app管理员或org管理员
- url参数：无
- request body： 新密码

		'{"newpassword" : "newpwd"}'
- response： 更新后的用户信息，json格式。见用户的数据结构。
- 错误代码：
 
#### curl示例：
		
	curl -X PUT -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "http://a1.easemob.com/easemob-demo/chatdemo/users/jliu/password" -d '{"newpassword" : "newpwd"}'



### 删除用户

### DELETE /${org_name}/${app_name}/users/${username}
- 描述：删除app的指定用户
- 权限：app管理员或org管理员
- url参数：无
- request body： 无
- response： 无
- 错误代码：

#### curl示例：
		
	curl -X DELETE -i -H "Authorization: Bearer YWMtP_8IisA-EeK-a5cNq4Jt3QAAAT7fI10IbPuKdRxUTjA9CNiZMnQIgk0LEUE" "http://a1.easemob.com/easemob-demo/chatdemo/users/jliu"
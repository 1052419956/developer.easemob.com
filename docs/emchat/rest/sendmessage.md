---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---



## 发送消息
环信支持通过rest接口发送消息



### 查看用户在线状态

#### GET /{org}/{app}/users/{username}/status
- 描述： 查看一个用户的在线状态
- 权限：app管理员或org管理员
- url参数： 无
- request body：	无		
- response： 

		{
    		....
    		"data": [
		        {
		            "target_name": "zw123", //username
		            "result_code": 200, 
		            "result_content": {"result":"offline"} //在线状态。offline:离线，online:在线
		        }
   			],
    		....
		}	

#### curl示例：
	
	curl -X get -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "http://a1.easemob.com/easemob-demo/chatdemoui/users/zw123/status"

### 给某个用户发送消息

#### POST /{org}/{app}/users/{username}/messages
- 描述：给某个用户发送消息，username为对方的username
- 权限：app管理员或org管理员
- url参数： 无
- request body：{"msg":{//消息内容，参考[聊天记录](http://developer.easemob.com/docs/emchat/rest/chatmessage.html)里的bodies内容}},譬如：

		{
			"msg":{
				"type":"text",
				"msg":"你好"
			}
		}

- response：
 
		{
			 ....
			 "data": [
		        {
		            "result_code": 200, //200为成功
		            "result_content": {
		                "result": "ok"
		            }
		        }
		    ],
			....
		}

#### curl示例
		
	curl -X post -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "http://a1.easemob.com/easemob-demo/chatdemoui/users/zw123/messages" -d '{"msg":{"type":"text","msg":"hello singlechat"}}'


### 给群组发送消息

#### POST /{org}/{app}/groups/{group_name}/messages
- 描述：给某个群组发送消息，group_name为要发送的群的id
- 权限：app管理员或org管理员
- url参数： 无
- request body：	和上面的单聊一样
- response： 

		{
			 ....
			 "data": [
		        {
		            "result_code": 200, //200为成功
		            "result_content": {
		                "result": "ok"
		            }
		        }
		    ],
			....
		}

#### curl示例
	curl -X post -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "http://a1.easemob.com/easemob-demo/chatdemoui/users/zw123/messages" -d '{"msg":{"type":"text","msg":"hello groupchat"}}'
---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---
## 聊天相关API

### 查看用户在线状态

#### GET /{org}/{app}/users/{username}/status

* 描述： 查看一个用户的在线状态
* 权限：app管理员或org管理员
* url参数： 无
* request body：	无		
* response： 

        {
            "action": "get",
            "application": "4d7e4ba0-dc4a-11e3-90d5-e1ffbaacdaf5",
            "params": {},
            "uri": "https://a1.easemob.com/easemob-demo/chatdemoui",
            "entities": [],
            "data": {
                "stliu": "online"  //注意, 这里返回的是用户名和在线状态的键值对, 值为 online 或者 offline
            },
            "timestamp": 1404932199220,
            "duration": 743,
            "organization": "easemob-demo",
            "applicationName": "chatdemoui"
        }
#### curl示例：
	
	curl -X GET -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "https://a1.easemob.com/easemob-demo/chatdemoui/users/zw123/status"

### 发送消息

此API可以一次给一个或者多个用户, 或者一个或者多个群组发送消息, 并且通过可选的_from_字段, 可以让接收方看到发送方是不同的人,
例如, 实现服务器替某个用户发消息的功能.

同时, 支持扩展字段, 通过_ext_属性, app可以发送自己专属的消息结构.

#### POST /{org}/{app}/messages

* 描述：给一个或者多个用户或者群组发送消息
* 权限：app管理员或org管理员
* url参数： 无
* request body：


        {
            "target_type" : "users", //or chatgroups
            "target" : ["u1", "u2", "u3"], //注意这里需要用数组, 即使只有一个用户, 也要用数组 ['u1']
            "msg" : {
                "type" : "txt",
                "msg" : "hello from rest" //消息内容，参考[聊天记录](http://developer.easemob.com/docs/emchat/rest/chatmessage.html)里的bodies内容
                },
            "from" : "jma2", //表示这个消息是谁发出来的, 可以没有这个属性, 那么就会显示是admin, 如果有的话, 则会显示是这个用户发出的    
            "ext" : { //扩展属性, 由app自己定义
                "attr1" : "v1",
                "attr2" : "v2"
            }    
    
        }


* response：
 
        {
            "action": "post",
            "application": "4d7e4ba0-dc4a-11e3-90d5-e1ffbaacdaf5",
            "params": {},
            "uri": "https://a1.easemob.com/easemob-demo/chatdemoui",
            "entities": [],
            "data": {
                "stliu1": "success",
                "jma3": "success",
                "stliu": "success",
                "jma4": "success"
            },
            "timestamp": 1404932446668,
            "duration": 110,
            "organization": "easemob-demo",
            "applicationName": "chatdemoui"
        }

## 群组相关API



## 获取app中所有的群组

    GET /{org}/{app}/chatgroups
    
返回值

        {
            "action": "get",
            "application": "4d7e4ba0-dc4a-11e3-90d5-e1ffbaacdaf5",
            "params": {},
            "uri": "https://a1.easemob.com/easemob-demo/chatdemoui",
            "entities": [],
            "data": [
                {
                    "groupid": "1404705934830"
                },
                {
                    "groupid": "1404114807341"
                },
                {
                    "groupid": "1404268812888"
                }    ],
            "timestamp": 1404932824158,
            "duration": 3551,
            "organization": "easemob-demo",
            "applicationName": "chatdemoui",
            "count": 0
        }
                

## 创建一个群组

    POST /chatgroups {}

参数

    {
        "groupname":"testrestgrp12", 
        "desc":"server create group", 
        "public":true, 
        "owner":"jma1", 
        "members":["jma2","jma3"]
    }
返回值 

    {"groupid":"1404882944671671"}

## 删除群组

    DELETE /chatgroups/{group_id}


返回值:

    {
        "action": "delete",
        "application": "4d7e4ba0-dc4a-11e3-90d5-e1ffbaacdaf5",
        "params": {},
        "uri": "http://localhost:8080/easemob-demo/chatdemoui",
        "entities": [],
        "data": {
            "success": true,
            "groupid": "1404928094479135"
        },
        "timestamp": 1404928958752,
        "duration": 11311,
        "organization": "easemob-demo",
        "applicationName": "chatdemoui"
    }

## 获取群组中的所有成员

    GET /chatgroups/{group_id}/users

## 在群组中添加一个人

    POST /chatgroups/{group_id}/users/{username}



## 在群组中减少一个人

    DELETE /chatgroups/{group_id}/users/{username}

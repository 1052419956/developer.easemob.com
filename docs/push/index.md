---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---



## 注册设备

为了让移动设备能够接收到推送的消息, app必须在启动的时候, 先到EMPush中注册

REST API:

    POST http://{push_server_host:port}/subscribers
    
这个API支持以下的参数:

* proto 

    当前设备的类型, 有两个可选值, 分别为
    
    * apns -- iOS 设备
    * xmpp -- android 设备
    
* token

    当前设备的device token
    
* jiduser

    为了支持离线聊天消息的推送, app可以选择同时提交当前登陆用户的jid, 这样, 在用户离线的情况下, 还能够通过push消息来得到聊天的离线消息
    
同时, 在调用此API的时候, 还需要带有一个HTTP Header, key为*appkey*    
    
这些参数需要组织成一个json数据来post到此API, 例如下面的示例    

	$ curl -d proto=apns \
	       -d token=FE66489F304DC75B8D6E8200DFF8A456E8DAEACEC428B427E9518741C92C6660 \
	       --header 'appkey: 1234567890' \
	       http://223.202.120.59:7874/subscribers
   
系统会返回一个json

	{
	  "proto": "apns",
	  "token": "7ee2400ca162be71eebafdb46056ff0afc973c8fab272d2b6627df4f86bac457",
	  "lang": "zh",
	  "badge": 0,
	  "updated": 1379343042,
	  "created": 1379343042,
	  "id": "UeJvEkzXLqA"
	}	       

需要把返回值中的_id_即为EMPush中此设备的_push id_保存下来

## 删除设备

<!-- 当一个app注册之后, 每次这个app启动的时候, 都需要ping push server, 来确保server知道这个设备还存在着, 不然, server有可能会把这个设备给移除掉 (目前没有做)

    curl -d lang=zh -d badge=0 http://223.202.120.59:7874/subscriber/UeJvEkzXLqA -->

把一个设备从EMPush中移出 (例如, app中的设置关闭了推送通知), 那么可以通过来完成

	DELETE http://{push_server_host:port}/subscribers/{push_id}


## Subscriptions

对于每个app, 它可以选择订阅到多个topic中, 从而接受这些topic中的push消息, 例如, 对于一个新闻类型的app, 它可以让用户来选择感兴趣的领域来接收推送消息

	curl -X POST http://223.202.120.59:7874/subscriber/UeJvEkzXLqA/subscriptions/sport		

也可以通过 **DELETE** 来取消订阅某一个topic的消息 (**TODO xmpp is not support this yet**)

或者, 也可以通过一次调用来完成订阅多个topic的操作 (**TODO** xmpp is not supported yet)

	curl -H 'Content-Type: application/json' -d '{"sport":{}, "music":{}}' http://223.202.120.59:7874/subscriber/UeJvEkzXLqA/subscriptions

这样, 对于每个app user, 可以自动的把他们注册到某些topic, 从而发送全局消息, 例如先获取用户的地理位置, 然后把他们注册到对应的城市中, 或者国家中等等

查看一个app都订阅了哪些topics

	GET /subscriber/UeJvEkzXLqA/subscriptions

## Event message

现在, 我们就可以给设备发送推送消息了, 注意, 消息是发送到topic的, 这里我们(推送消息的发送方)不需要知道topic中有哪些设备, 如果一个设备都没有的话, push server会直接忽略的

	curl -d title=Test%20message http://223.202.120.59:7874/event/sport	

上面的例子只使用一个 _title_, 这个的值会被发送到ios的通知栏显示出来

标准的消息格式目前支持:


    {
        title: "this is a title",
        msg: "the internal message",
        sound: "the sound app should play when got this notifer",
        data: {
            //here can be anyting, the value of 'data' attribute, either a sub-json, or a string
        }
    }	

只有title是必须的, data属性的值可以是任何合法的json值, 这部分数据只有在用户相应了通知之后, 打开app, app才会得到



## 单点推送

通过这个功能, 我们可以根据push id来推送给单独的一个人

	POST /send/:push_id {}

这里POST的数据遵循上面event message部分的规则

当前这个单点推送只支持ios	
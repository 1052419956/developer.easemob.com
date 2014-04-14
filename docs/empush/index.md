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
    
例如(注意, 在调用这个API的时候, 同样需要在HTTP Header当中附带_appkey_信息)

    curl --header "appkey: 1234567890" http://223.202.120.59:7874/subscriber/UeJvEkzXLqA

## 创建Topic

topic无需创建，当订阅topic时，如果该topic不存在，则系统自动创建该topic.

<!-- 
## 删除Topic

## 查询已有Topic
-->

## 订阅Topic

对于每个在EMPush中注册的设备, 都可以选择订阅到多个topic中, 从而接收这些topic中的push消息, 例如, 对于一个新闻类型的app, 可能就可以同时订阅到"sport", "news"等topics当中.

REST API

    POST http://{push_server_host:port}/subscribers/{push_id}/subscriptions/{topic_name}

例如    

	curl --header "appkey: 1234567890" -X POST http://223.202.120.59:7874/subscriber/UeJvEkzXLqA/subscriptions/sport		


## 批量订阅Topic

可以通过一次调用来完成订阅多个topic的操作 (**TODO** xmpp is not supported yet)

	curl -H 'Content-Type: application/json' -d '{"sport":{}, "music":{}}' http://223.202.120.59:7874/subscriber/UeJvEkzXLqA/subscriptions

这样, 对于每个app user, 可以自动的把他们注册到某些topic, 从而发送全局消息, 例如先获取用户的地理位置, 然后把他们注册到对应的城市中, 或者国家中等等

## 取消订阅Topic

可以通过 **DELETE** 来取消订阅某一个topic的消息 (**TODO xmpp is not support this yet**)

REST API

    DELETE http://{push_server_host:port}/subscribers/{push_id}/subscriptions/{topic_name}

## 查看已订阅的Topics

REST API

	GET http://{push_server_host:port}/subscribers/{push_id}/subscriptions

## 发送推送消息

现在, 我们就可以给设备发送推送消息了, 注意, 消息是发送到topic的, 这里我们(推送消息的发送方)不需要知道topic中有哪些设备, 如果一个设备都没有的话, push server会直接忽略的

REST API

    POST http://{push_server_host:port}/event/{topic_name}  {推送的消息}    

<!--### 其他推送参数

如何发送其他推送参数？
time_to_live
从消息推送时起，保存离线的时长。秒为单位。最多支持10天（864000秒）。

 0 表示该消息不保存离线。即：用户在线马上发出，当前不在线用户将不会收到此消息。

此参数不设置则表示默认，默认为保存1天的离线消息（86400秒）


verification_code

验证串，用于校验发送的合法性。

由 sendno, receiver_type, receiver_value, master_secret 4个值拼接起来（直接拼接字符串）后，进行一次MD5生成。

-->

### 推送消息内容格式
推送消息要求为JSON格式

    {
        "title": "通知栏弹出的消息",
        "data": {
            //具体的消息体,可以是任何合法的json值
        },
    }

只有title是必须的, data属性的值可以是任何合法的json值, 这部分数据只有在用户响应了通知之后, 打开app, app才会得到

### iOS APNs 特别说明 ###

通过 EMPush API 推送 iOS APNs 消息时，部分内容需要与 APNs 适配。

关于 APNs 的详细定义，请参考官方文档：[Apple Push Notification Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW12)。

EMPush 字段	APNs 字段

    {
        "title": "通知栏弹出的消息", //对应APNs的alert
        "badge":88	//对应APNs的badge
        "sound":""	//对应APNs的sound
    }


title 字段必须存在，但可以为空字符串。这时，如果 extras 里带了 badge 字段，则收到的通知会显示应用图标右上角数字，而没有通知栏内容。


指定了 iOS 特定参数的完整的通知 msg_content JSON串示例：

    {
        "title": "通知栏弹出的消息", 
        "data": {
            //具体的消息体
        },
        "badge":88,	
        "sound":"default"	
    }

 
### 通知长度限制说明 ###

EMPush 同时支持 Andorid 与 iOS 平台的通知推送。

由于 APNs 限制是 255 个字节，所以 EMPush 推送通知也依据 APNs 来统一限制。
	

注意：长度指字节。由于使用 UTF-8 编码，所以一个中文字符占 3 个字节。


<!-- 
## 单点推送

通过这个功能, 我们可以根据push id来推送给单独的一个人

    POST /send/:push_id {}

这里POST的数据遵循上面event message部分的规则

当前这个单点推送只支持ios     -->
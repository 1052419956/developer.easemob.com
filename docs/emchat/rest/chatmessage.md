---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---

# 聊天消息导出

环信支持app把聊天记录通过REST接口导出

## 聊天记录数据结构

	{
		"type": "chatmessage",
	    "from": "zw123", //发送人username
        "msg_id": "5I02W-16-8278a", //消息id
        "payload": {
            "bodies": [ //消息bodies
                {
                    "msg": "hhhhhh", //消息内容
                    "type": "txt" //消息类型。txt:文本消息, img:图片, loc：位置, audio：语音
					"chat_type": "chat" //用来判断单聊还是群聊。chat:单聊，groupchat:群聊
					"length": 3, //语音时长，单位为秒，这个属性只有语音消息有
                    "url": "", //图片语音等文件的网络url，图片和语音消息有这个属性
                    "filename": "22.png", //文件名字，图片和语音消息有这个属性
                    "secret": "pCY80PdfEeO4Jh9URCOfMQWU9QYsJytynu4n-yhtvAhmt1g9", //获取文件的secret，图片和语音消息有这个属性
					"lat": 39.983805, //发送的位置的纬度，只有位置消息有这个属性
                    "lng": 116.307417, //位置经度，只有位置消息有这个属性
                    "addr": "北京市海淀区北四环西路66号" //位置消息详细地址，只有位置消息有这个属性
                }
            ]
        },
        "timestamp": 1403099033211, //消息发送时间
        "to": "1402541206787" //接收人的username或者接收group的id
	}

## 聊天消息REST API

以下所有API均需要公司管理员或app管理员权限才能访问。

### app管理员登录并获取授权token

获取token请参考[用户管理REST API](http://developer.easemob.com/docs/emchat/rest/userapi.html)


#### GET /{org_name}/{app_name}/chatmessages
- 描述： 获取聊天记录
- 权限：app管理员或org管理员
- url参数： 
- request body：	无		
- response： 聊天记录(json),默认返回10条记录

		{
    		....
    		count : 10, //返回的数量
    		cursor : "asdsdfaee" //游标，用于分页查询
    		entities : [
    			{
    			聊天记录entity
    			}
    			...
    		]
    		....
		}	

#### 使用示例1：获取最新的20条记录

在url后面加上参数`ql=order by timestamp desc，limit=20`,实际使用时需要对"="后边的内容进行utf8 encode转义
		
curl示例

	curl -X get -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "https://a1.easemob.com/easemob-demo/chatdemo/chatmessages?ql=order+by+timestamp+desc&limit=20"


#### 使用示例2：获取某个时间段内的消息

在url后面加上参数`ql=select * where timestamp<1403164734226 and timestamp>1403166586000 order by timestamp desc`, 同上"="后的参数需要转义

curl示例

	curl -X get -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "https://a1.easemob.com/easemob-demo/chatdemoui/chatmessages?ql=select+*+where+timestamp%3C1403164734226+and+timestamp%3E1403163586000+order+by+timestamp+desc"

#### 使用示例3：分页获取数据

使用limit参数获取数据完毕后，如果后边还有数据，会返回一个不为空的cursor回来，使用这个cursor就能进行分页获取了，具体可参考[文档](http://developer.easemob.com/docs/emchat/rest/pagingquery.html)。

分页示例：根据之前获取数据返回的cursor继续获取后面的20条数据。在url后面加上参数

    ql=order by timestamp desc
    limit=20
    cursor=MTYxOTcyOTYyNDpnR2tBQVFNQWdHa0FCZ0ZHczBKN0F3Q0FkUUFRYUdpdkt2ZU1FZU9vNU4zVllyT2pqUUNBZFFBUWFHaXZJUGVNRWVPMjdMRWo5b0w4dEFB

同上参数需要转义

curl示例

	curl -X get -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "https://a1.easemob.com/easemob-demo/chatdemoui/chatmessages?ql=select+*+order+by+timestamp+desc&limit=10&cursor=MTYxOTcyOTYyNDpnR2tBQVFNQWdHa0FCZ0ZHczFuSG93Q0FkUUFROW94S0lQZVBFZU9mTEQxQWVMdHEyQUNBZFFBUTlvd2pFUGVQRWVPaHFWa1l0ZjA2dEFB"


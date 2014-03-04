---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---


### 使用limit和cursor

假设当前数据库中, cats的集合中有100个entity

然后执行查询

	GET /cats?limit=9
	
这时候会返回一个json

	{
	....
	count : 9,
	cursor : asdfsdfdsfdsfsd
	entities : [
		{
		ENTITY PROPERTIES
		}
	]
	....
	}	
	
	
上面的response中的json里面有三个属性需要注意:

* count : 告诉你的是这个response中包含了多少个entities
* cursor : 告诉你的是当前的游标是多少
* entities : 包含了返回的count个entities

把 limit 和 cursor结合起来使用可以达到分页的效果

例如上面的例子中, 那个查询只使用了 limit, 而没有使用cursor, 则表达的是:

从cats集合中的开头获取9个cat, 返回给我

然后返回的response的cursor表达的是, 这里有个指针, 指向第十个cat

所以, 想要再获取9个cats的话 就可以执行查询


	GET /cats?limit=9&cursor=asdfsdfdsfdsfsd
	
这样, 会返回从第十条到第17条数据 (这次又会返回一个新的cursor的值, 指向第18个元素), 依次类推

这样, 当我们执行了这个查询11次( 取了99个entities了), 因为一共有100个, 那么还剩下一个, 我们还可以继续使用第11次返回的cursor, 假设是111111

	GET /cats?limit=9&cursor=111111
	
注意, 这时候, 111111这个指针指向的是第100行, 并且表中没有更多的数据了

所以, 这次只会返回一个 (也就是第一百个), 并且, 返回值中没有 "cursor"这个属性!!!

所以, 我们只需要检查返回值中是否有"cursor", 就可以判断是否已经把数据都取完了

-------------------------------------

上面是一次同步的过程

也就是, 通过使用limit和cursor, 把当前表中的数据都遍历完, 那么, 假设过了一段时间, 我们的app又需要同步, 这时候, 我们想要的是, 从我们之前完成的地方(第101条) 开始, 到当前数据库中的新内容 (假设这段时间, 数据库又被更新了100条), 所以, 现在数据库中一共有200条数据, 而我们这次想要遍历的是 101 -- 200

而上面我们说过了, 我们上次同步结束的时候, 返回值是没有cursor的, 而再上一次, 是有一个指向第99条数据的cursor的

并且, 我们也知道, 上次同步的时候, 最后一次获取到的entities个数是1个 (通过count属性可知)

所以, 我们只需要使用 指向第99行的cursor, 来继续执行

	GET /cats?limit=9&&cursor=111111
	
这一个查询, 实际上是获取的 100 - 109行

而第100行实际上我们已经处理过了, 所以这次我们需要从返回值中跳过第100行, 这个判断可以从上次同步的时候, 最后一次请求获取到的count来得知 (上面说了, 最后一次返回值, 没有cursor, 但是有count)

	



# /management

### GET /management/token
em这里获取的是Org的管理token


查询参数:

* grant_type

    可选值为"password" 或者 "client_credentials"

* username
* password

    当 _grant_type_ 为"password"的时候, 这两个是必须的

* client_id
* client_secret

    当 _grant_type_ 为"client_credentials"的时候, 这两个是必须的, 不过也可以不把这两个参数放在query中, 而是放在Header中
    
    把id和secret使用":"连接起来然后使用base64编码, 放在header的 _Authorization_ 属性中.
    
    header.put('Authorization', 'BASIC base64(client_id:client_secret)')
    
* ttl

    token的有效期

返回值:

    {
      "access_token": "YWMto8iOILGGEeKXq-kjNogX0AAAAT5-r_QCD4iN5STo9JJGZ8bUwaE0oO68aTA",
      "expires_in": 3600,
      "organization": {
        "users": {
          "test": {
            "applicationId": "00000000-0000-0000-0000-000000000001",
            "username": "test",
            "name": "Test User",
            "email": "test@usergrid.com",
            "activated": true,
            "disabled": false,
            "properties": {},
            "uuid": "2ba68b7a-ae5e-11e2-b072-afb881fa9e35",
            "adminUser": true,
            "displayEmailAddress": "Test User <test@usergrid.com>",
            "htmldisplayEmailAddress": "Test User &lt;<a href=\"mailto:test@usergrid.com\">test@usergrid.com</a>&gt;"
          }
        },
        "name": "test-organization",
        "applications": {
          "test-organization/test-app": "2d2dc7b0-ae5e-11e2-b659-419316fe4dac",
          "test-organization/test1-app": "09621090-affb-11e2-ac08-773f1211b6de",
          "test-organization/test2-app": "0ce4f5c0-affb-11e2-a39c-d3084a41edc8"
        },
        "uuid": "2c98176a-ae5e-11e2-9ab7-4d3f1d94d74f"
      }
    }


**TODO** 貌似这个返回可以简化一下, 只返回基本信息, 从而减少流量消耗?

    { access_token, expire_in, organization_name }
    
### POST /management/token


和GET的方法一样, 只不过这里需要把参数拼成json的格式然后放在POST 的data中.

## /management/orgs

POST /management/orgs

创建一个新的org, 并且创建这个新的org的管理员

所需参数:

    {
		organization: 'test22-org',
		username: 'test22',
		name: 'test22-name',
		email: 'test22@usergrid.com',
		password: 'test22'
	}
	
权限: 无 (这个可能有问题, 至少得需要个system access的权限)	
返回值:

    {
      "action": "new organization",
      "status": "ok",
      "data": {
        "owner": {
          "applicationId": "00000000-0000-0000-0000-000000000001",
          "username": "test22",
          "name": "test22-name",
          "email": "test22@usergrid.com",
          "activated": true,
          "disabled": false,
          "properties": {},
          "uuid": "32c7025a-b196-11e2-b7b3-bfc2e5ed324a",
          "adminUser": true,
          "displayEmailAddress": "test22-name <test22@usergrid.com>",
          "htmldisplayEmailAddress": "test22-name &lt;<a href=\"mailto:test22@usergrid.com\">test22@usergrid.com</a>&gt;"
        },
        "organization": {
          "name": "test22-org",
          "uuid": "33fb8aba-b196-11e2-af1d-11932069958b"
        }
      },
      "timestamp": 1367326942957,
      "duration": 2477
    }

### /management/orgs/:organization_uuid
### /management/orgs/:organization_name

权限: organization access

class: org.usergrid.rest.management.organizations.OrganizationResource

## /management/users

class: org.usergrid.rest.management.users.UsersResource

    
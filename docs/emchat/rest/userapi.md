---
title: 环信
description: 5分钟为你的APP加入聊天功能
category: emchat
layout: docs
---
# 环信用户管理介绍



# 用户管理REST API

## 用户的json数据结构说明

	{
	  username: "jliu",  //username是用户的primarykey。需要在appkey的范围内唯一。
	  password:"123456"  //登录密码 123456
	}

## 用户名规则

当App和环信集成的时候， 需要把App系统内的已有用户和新注册的用户和环信继承， 为每个已有用户创建一个环信的账号， 并且App有新用户注册的时候， 需要同步的在环信中注册。

在注册环信账户的时候， 需要注意环信的**用户名规则**：

* 用户名需要使用英文字母和（或）数字的组合
* 用户名不能使用中文
* 用户名不能使用email地址
* 用户名中间不能有空格或者井号（#）等特殊字符

因为一个用户的环信用户名和他的在App中的用户名并不需要一致， 只需要有一个明确的对应关系， 例如， 用户名是 _stliu@apache.org_, 当这个用户登陆到App的时候， 可以登陆成功之后， 再登陆环信的服务器， 所以这时候， 只需要能够从 _stliu@apache.org_ 推导出这个用户的环信用户名即可

我们推荐可以使用 _md5_ 函数， 对用户名进行加密， 因为很多app的用户名选择使用的是 _email_ 或者 _手机号_ 等注册的， 这时候， 用户名实际上也是用户的隐私信息， 如果直接使用这种用户名到环信注册， 相当于app把用户的隐私信息暴露给了环信的， 从环信的角度， 我们也不希望知道App用户的任何信息的

而使用 _md5_ 函数对App用户名进行转换， 则很好的解决了这一问题， 即保证了， 无论App中的用户名规则是什么样子的， 中文， email等等的值， 经过md5函数之后， 都可以得到一个环信中的合法的用户名， 同时， 也保证了App的*真实*用户名信息并没有暴露给环信。

## 名词解释

* org_name
    
    是开发者在环信开发者后台注册账号时填写的公司Id。
    
* app_name

    是开发者在环信开发者后台创建app时填写的app名。

* org_admin
    是开发者在环信开发者后台注册的账号。org管理员账号可以在自己账号下创建多个app。org管理员拥有对该公司账号下所有app的操作权限。
    
* app_admin
    
    每个app都可以创建app管理员（可选操作）。app管理员拥有app级别的操作权限。

## 用户管理REST API

以下所有API均需要org管理员或app管理员权限才能访问。

强烈建议保护好org管理员及app管理员的用户名和密码, 尽量只在APP的服务器后台对环信用户做增删改查的管理，包括新用户注册。

为了您的信息安全, 请一定不要将org管理员或app管理员的用户名和密码写死在手机客户端中, 因为手机app很容易被反编译, 从而导致别人获取到您的管理员账号和密码, 导致数据泄露.

### 创建app管理员

<!-- #### 方式一  进入环信开发者后台创建 -->

以管理员身份登录环信开发者后台 -> 进入到每个app应用概况页 -> 点击"添加应用管理员"即可添加app管理员。创建结束后会跳转到用户列表页，app管理员会出现在"用户管理"下的用户列表中。可以像操作IM用户一样操作app管理员用户（包括重置密码、删除用户） 



### 使用app的client_id 和 client_secret登陆并获取授权token

client_id 和 client_secret可以在[环信管理后台](https://console.easemob.com)的app详情页面看到

#### POST /{org_name}/{app_name}/token

- 描述： 登录并授权，获得一个token。
- 权限：
- url参数： 无
- request body： 授权数据（json格式）。

		{
		  "grant_type": "client_credentials",
	  	  "client_id": "jliu1",
	  	  "client_secret": "jliu1"
		}

- response： 授权结果(json),其中access_token为授权后的token


这个token都具有操作整个app内部所有api的权限, 我们建议在做服务器端继承的时候, 在程序中使用 client_id和 client_secret的方式, 在登陆管理后台的时候使用账号密码

#### curl示例：

>> 注：请将URL中的easemob-demo/chatdemo替换成你自己的org_name和app_name。并将"client_id","client_secret"分别替换成你自己App中的值
		
	curl -X POST "https://a1.easemob.com/easemob-demo/chatdemo/token" -d '{"grant_type":"client_credentials","client_id":"jliu1","client_secret":"jliu1"}'
	
				
Response 的返回结果如下：
		
    {
        "access_token": "YWMtowpVAP9yEeOihk3Cy3TsXwAAAUcLE-Bkfko63p9yHqqF6MsOLaOJPzSHDB8",
        "expires_in": 604800,
        "application": "4d7e4ba0-dc4a-11e3-90d5-e1ffbaacdaf5"
    }

从这个返回值中, 可以得到两部分信息:

1. access_token

     这个是服务器用来做权限验证的，后续的所有操作(这里的操作指的是app访问服务器的request), 都需要把这个token加到header当中, 在本文档后面所有描述到得request都会有这个header   

         -H "Authorization: Bearer YWMtNda4DFzyEeOrOy_LuVzHjAAAAULiG1IrN8opggpytUDFmJkiocawbINICYk"

2. expires_in

    access token的有效期， 单位为秒， 现在默认的有效期是七天（60*60*24 ＝ 604800）， 所以在七天内是不需要重复获取的
        
## 创建环信账号 分两种模式：开放注册 和 授权注册


- "开放注册"模式：注册环信账号时不用携带管理员身份认证信息；


- "授权注册"模式：注册环信账号必须携带管理员身份认证信息。推荐使用"授权注册"，这样可以防止某些已经获取了注册url和知晓注册流程的人恶意向服务器大量注册垃圾用户。

### 开放注册模式示例

#### POST /{org_name}/{app_name}/users

- 描述：在指定的org和app中创建一个新的用户
- 权限： 无
- url参数： 无
- request body： 要创建的用户，json格式。见用户的数据结构。
- response： 创建成功的用户，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X POST -i "https://a1.easemob.com/easemob-demo/chatdemo/users" -d '{"username":"jliu","password":"123456"}'

#### response返回:
		
	{
		"action" : "post",
		"application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
		"params" : { },
		"path" : "/users",
		"uri" : "https://a1.easemob.com/easemob-demo/chatdemo/users",
		"entities" : [ {
			"uuid" : "7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
			"name" : "jliu0003",
			"created" : 1368377620796,
			"modified" : 1368377620796,
			"username" : "jliu",
    		}
      	} ],
      	"timestamp" : 1368377620793,
      	"duration" : 125,
      	"organization" : "easemob-demo",
      	"applicationName" : "chatdemo"
	}
 注：返回的response json数据中会包含除上述属性之外的一些其他信息，均可以忽略。

### 授权注册模式

#### 先获取app管理员token

详见_“使用app的client_id 和 client_secret登陆并获取授权token”_

#### 创建用户

### POST /{org_name}/{app_name}/users

- 描述：在指定的org和app中创建一个新的用户
- 权限： app管理员或org管理员
- url参数： 无
- request body： 要创建的用户，json格式。见用户的数据结构。
- response： 创建成功的用户，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X POST -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "https://a1.easemob.com/easemob-demo/chatdemo/users" -d '{"username":"jliu","password":"123456"}'

注：可以看到，授权注册模式创建用户时，请求多带了一个名为"Authorization"的header，这个header的值就是管理员的token，例如: -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k"

#### response返回:
		
	{
		"action" : "post",
		"application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
		"params" : { },
		"path" : "/users",
		"uri" : "https://a1.easemob.com/easemob-demo/chatdemo/users",
		"entities" : [ {
			"uuid" : "7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
			"name" : "jliu0003",
			"created" : 1368377620796,
			"modified" : 1368377620796,
			"username" : "jliu",
    		}
      	} ],
      	"timestamp" : 1368377620793,
      	"duration" : 125,
      	"organization" : "easemob-demo",
      	"applicationName" : "chatdemo"
	}
  
注：返回的response json数据中会包含除上述属性之外的一些其他信息，均可以忽略。

## 批量创建用户

### POST /{org_name}/{app_name}/users [{"username":"u1", "password":"p1"}, {"username":"u2", "password":"p2"}]

- 描述：批量注册IM用户  建议批量不要过多, 在20-60之间
- 权限：app管理员或org管理员
- url参数：无
- request body： 要创建的用户的json数组。每个用户是json格式，见用户的数据结构。
- response： 无
- 错误代码：

#### curl示例：
		
	curl -X POST -i -H "Authorization: Bearer YWMtP_8IisA-EeK-a5cNq4Jt3QAAAT7fI10IbPuKdRxUTjA9CNiZMnQIgk0LEUE" "https://a1.easemob.com/easemob-demo/chatdemo/users" -d '[{"username":"u1", "password":"p1"}, {"username":"u2", "password":"p2"}]'


## 获取指定用户详情

### GET /{org_name}/{app_name}/users/{username}

- 描述：获取app的指定用户详情
- 权限：app管理员或org管理员
- url参数：无
- request body： 无
- response： 指定的用户详情，json格式。见用户的数据结构。
- 错误代码：

#### curl示例：
		
	curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "https://a1.easemob.com/easemob-demo/chatdemo/users/jliu1"

## 重置用户密码

### PUT /{org_name}/{app_name}/users/{username}/password

- 描述： 重置用户密码
- 权限：app管理员或org管理员
- url参数：无
- request body： 新密码

		'{"newpassword" : "newpwd"}'
- response： 更新后的用户信息，json格式。见用户的数据结构。
- 错误代码：
 
#### curl示例：
		
	curl -X PUT -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "https://a1.easemob.com/easemob-demo/chatdemo/users/jliu/password" -d '{"newpassword" : "newpwd"}'

## 删除用户

### DELETE /{org_name}/{app_name}/users/{username}
- 描述：删除app的指定用户
- 权限：app管理员或org管理员
- url参数：无
- request body： 无
- response： 无
- 错误代码：

#### curl示例：
		
	curl -X DELETE -i -H "Authorization: Bearer YWMtP_8IisA-EeK-a5cNq4Jt3QAAAT7fI10IbPuKdRxUTjA9CNiZMnQIgk0LEUE" "https://a1.easemob.com/easemob-demo/chatdemo/users/jliu"

## 批量删除用户

### DELETE /{org_name}/{app_name}/users?limit=300
- 描述：删除某个app下指定数量的环信账号。上述url可一次删除300个用户,这个300用户是指按创建时间逆序排列的300用户，即从当前时间起往前数,数值可以修改 建议这个数值在100-500之间，不要过大
- 权限：app管理员或org管理员
- url参数：无
- request body： 无
- response： 无
- 错误代码：

#### curl示例：
		
	curl -X DELETE -i -H "Authorization: Bearer YWMtP_8IisA-EeK-a5cNq4Jt3QAAAT7fI10IbPuKdRxUTjA9CNiZMnQIgk0LEUE" "https://a1.easemob.com/easemob-demo/chatdemo/users?limit=300"

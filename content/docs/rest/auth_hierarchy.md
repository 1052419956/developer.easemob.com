---
title: REST API 开发指南
secondnavrest: true
sidebar: restsidebar
---


# 权限 角色 授权 认证

## 权限 permission
在环信REST服务体系，权限代表对某个资源的操作能力，包括GET / PUT / POST / DELETE等。

权限的表示：

        get:/*    表示可以获取根路径开始往下的所有资源
    	get:/users/*    表示可以获取/users开始往下的所有的资源
    	get,put,post,delete:/**  多个权限表示的简写表示
  
## 角色 role
>首先需要明确的是, 一个用户可以有多个roles, 每个role可以有多个permissions, 每个permission定义了一个匹配规则, 来决定一个request是否能够被访问

默认的系统有三个roles, 分别是
  -  admin
  -  default
  -  guest

所有的未登陆用户都拥有guest权限, 所有的登陆用户都有default权限,也就是说一个登陆用户对所有非系统API都有所有的访问权限."非系统API"是说的是一些系统定义的拥有默认权限控制的API, 例如删除用户, 就只允许admin和用户自己来删除, 这时候不管当前用户有用什么权限

从default的permission可以看到, 一个登陆用户能够访问到所有的数据

所以, 为了做到访问控制, 首先第一步就是删除这个权限

delete /{org}/{app}/roles/default

然后创建一个新的同名的role

post /{org}/{app}/roles {"name":"default", "rolename":"default", "title":"Default"}

这个时候, 因为default role没有任何的permission, 所以, 所有的登陆用户都没有办法做任何事情

现在, 假设, 我们想让 一个用户拥有"自己"的数据的完整的访问权限

我们可以假定所有已 当前登陆用户的用户名开头的collection都是用户自己的数据

例如, 当前登陆用户为 stliu, 那么, 系统中所有以 "stliu-" 开头的collection都是stliu自己的数据, 只能够被stliu访问 具体来说, 就是 stliu 对 /{org}/{app}/stliu-cats 以及下面的url有用所有的访问权限, 而用户jliu想要访问 /{org}/{app}/stliu-cats 的资源的时候, 就会报错, 说没有访问权限

为了达到这个目的, 我们可以给default添加下面的permission
post /{org}/{app}/roles/default {"permission": "get,put,post,delete:/${user}-*/**" }

可以看到, 在这个permission的定义中, 我们使用了 ${user} 的变量, 这个变量会在校验权限的时候, 使用当前登陆用户的用户名来代替, 从而达到我们的目的
但是, 现在一个登陆用户, 除了操作自己的数据之外, 就什么也干不了了, 例如, 一个用户名为stliu的登陆用户只能执行
post /{org}/{app}/stliu-books


## 授权
授权即是给某个用户赋予某个角色，从而使其拥有某些资源的操作权限。



## 认证
在环信的认证体系中，是通过校验token来做权限认证的。具体说就是用户请求操作某个资源的时候在http头信息中按规则发送token到服务器端，服务器对此token进行校验并返回给客户端校验结果。

强烈建议保护好org管理员及app管理员的用户名和密码, 尽量只在APP的服务端对环信用户做增删改查的管理。

##获取token
获取token有两种方式：1. 通过用户名和密码获取 2. 通过client_id和client_secret获取
#### 获取client_id和client_secret
#### 使用app的client_id 和 client_secret登陆并获取授权token

client_id 和 client_secret可以在[环信管理后台](https://console.easemob.com)的app详情页面看到

#### POST /{org_name}/{app_name}/token

- 描述： 登录并授权，获得一个token。
- 权限：
- url参数： 无
- request body： 授权数据（json格式）。

		{
		  "grant_type": "client_credentials",
	  	  "client_id": "YXA6wDs-MARqEeSO0VcBzaqg5A",
	  	  "client_secret": "YXA6JOMWlLap_YbI_ucz77j-4-mI0JA"
		}

- response： 授权结果(json),其中access_token为授权后的token


这个token都具有操作整个app内部所有api的权限, 我们建议在做服务器端继承的时候, 在程序中使用 client_id和 client_secret的方式, 在登陆管理后台的时候使用账号密码

#### curl示例：

>> 注：请将URL中的easemob-demo/chatdemo替换成你自己的org_name和app_name。并将"client_id","client_secret"分别替换成你自己App中的值
		
	curl -X POST "https://a1.easemob.com/easemob-demo/chatdemo/token" -d '{"grant_type":"client_credentials","client_id":"YXA6wDs-MARqEeSO0VcBzaqg5A","client_secret":"YXA6JOMWlLap_YbI_ucz77j-4-mI0JA"}'
	
				
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

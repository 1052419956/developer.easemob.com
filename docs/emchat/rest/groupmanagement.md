---
title: Easemob Chat 快速入门
description: Easemob Chat 快速入门
category: push
layout: docs
---
# 群聊群组管理

此文档用于群组聊天页面的**创建群组**、**管理员删除群组**、**管理员添加群成员**、**管理员删除群成员**、**群成员手动退出群组**等接口描述。

	BaseUrl: http://cloudcode.easemob.com
	
## 创建一个群组

### POST /management/rest/organizations/{AppKey}/chatrooms

* 描述: 创建一个聊天群组
* 权限: app用户登录
* Url参数: 无
* Request body: 
  
        {
		    "name": "weiquan1792863_131203130921393917196331", //由客户端生成, 需要保证唯一性, 最好是使用TimeStamp来生成
		    "members": ["13370102365", "13811691807", "13120313092"], //需要添加到群组的用户Username数组, 需要将自己也添加在里面
		    "groupname": "群组的名称", //群组的名称, 可以随便取名, 用于显示群组的名称
		    "admin": "13120313092",  //创建群组的用户名(管理员)
		    "desc": "这是一个群组"   //群组的描述信息
		}

* 错误代码：
  
    401 Unauthorized： 认证失败。返回的response body为json数据：
    
        {"error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }

* curl示例：

		curl -X POST -i -H "Authorization: Bearer YWMtht-JGHzrEeO-PP32wx0elgAAAUOzppyII4imX-1OwFD3VrVI9J9ua383uug" -H "Content-Type: application/json" "http://cloudcode.easemob.com/management/rest/organizations/weiquan1792863/chatrooms" -d '{"name":"weiquan1792863_131203130921393917196331","members":["13370102365","13811691807","13120313092"],"groupname":"群组","admin":"13120313092","desc":"这是一个群组"}'


* 发送请求后, 服务器状态码返回204, 表示创建成功, 其它状态码, 都表示创建失败, 创建成功后, 可以直接使用客户端创建的群组name (示例里是weiquan1792863_131203130921393917196331)来进行通信
	
## 管理员删除一个群组

### DELETE /management/rest/organizations/{AppKey}/chatrooms/{chatroom name}

* 描述: 管理员删除一个群组
* 权限: app用户登录
* Url参数: 无	
* Request body: 无
* 错误代码：
  
    401 Unauthorized： 认证失败。返回的response body为json数据：
    
        {"error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }

* curl示例：

		curl -X DELETE -i -H "Authorization: Bearer YWMtht-JGHzrEeO-PP32wx0elgAAAUOzppyII4imX-1OwFD3VrVI9J9ua383uug" "http://cloudcode.easemob.com/management/rest/organizations/weiquan1792863/chatrooms/weiquan1792863_131203130921393917195431"

* 发送请求后, 服务器状态码返回204, 表示删除成功, 其它状态码, 都表示删除失败, 删除成功后, 可以客户端继续做其它操作(退出当前页面)

## 管理员添加群成员

### POST /management/rest/organizations/{AppKey}/chatrooms/{chatroom name}/members

* 描述: 管理员添加群成员
* 权限: app用户登录
* Url参数: 无	
* Request body: 

		{
		    "members": ["18610245183"]  //需要添加的用户名数组
		}
	
* 错误代码：
  
    401 Unauthorized： 认证失败。返回的response body为json数据：
    
        {"error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }

* curl示例：

		curl -X GET -i -H "Authorization: Bearer YWMtht-JGHzrEeO-PP32wx0elgAAAUOzppyII4imX-1OwFD3VrVI9J9ua383uug" -H "Content-Type: application/json" "http://cloudcode.easemob.com/management/rest/organizations/weiquan1792863/chatrooms/weiquan1792863_131203130921393917196331/members" -d '{"members":["18610245183"]}'

* 发送请求后, 服务器状态码返回204, 表示添加成功, 其它状态码, 都表示添加失败, 添加成功后, 可以客户端继续做其它操作(刷新客户端群成员列表等)

## 管理员删除群成员

### POST /management/rest/organizations/{AppKey}/chatrooms/{chatroom name}/members/delete

* 描述: 管理员删除群成员
* 权限: app用户登录
* Url参数: 无
* Request body: 
  
		{
		    "members": ["18610245183"]  //需要删除的用户名数组
		}

* 错误代码：
  
    401 Unauthorized： 认证失败。返回的response body为json数据：
    
        {"error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }

* curl示例：

		curl -X GET -i -H "Authorization: Bearer YWMtht-JGHzrEeO-PP32wx0elgAAAUOzppyII4imX-1OwFD3VrVI9J9ua383uug" -H "Content-Type: application/json" "http://cloudcode.easemob.com/management/rest/organizations/weiquan1792863/chatrooms/weiquan1792863_131203130921393917196331/members/delete" -d '{"members":["18610245183"]}'

* 发送请求后, 服务器状态码返回204, 表示删除成功, 其它状态码, 都表示删除失败, 删除成功后, 可以客户端继续做其它操作(刷新客户端群成员列表等)
	

## 群成员退出群组

### POST /management/rest/organizations/{AppKey}/chatrooms/{chatroom name}/exit

* 描述: 群成员退出群组
* 权限: app用户登录
* Url参数: 
		
		{
		    "members": ["13120313092"] //需要退出群组的Username
		}
		
* Request body: 无
* 错误代码：
  
    401 Unauthorized： 认证失败。返回的response body为json数据：
    
        {"error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }

* curl示例：

		curl -X GET -i -H "Authorization: Bearer YWMtht-JGHzrEeO-PP32wx0elgAAAUOzppyII4imX-1OwFD3VrVI9J9ua383uug" -H "Content-Type: application/json" "http://cloudcode.easemob.com/management/rest/organizations/weiquan1792863/chatrooms/weiquan1792863_1861024508320131118163235/exit" -d '{"members":["18610245183"]}'

* 发送请求后, 服务器状态码返回204, 表示退出成功, 其它状态码, 都表示退出失败, 退出成功后, 可以客户端继续做其它操作(退出当前页面, 并刷新群组列表)


---
title: 易用户
description: EM用户体系
category: emuser
layout: docs
---

# 1. 概述 #

EaseMob 用户体系的所有功能均以REST接口方式暴露出来供客户端调用。客户端可以使用任何语言调用用户体系API接口，比如Android, iOS, php, Java, 浏览器上支持的各种语言等。

## 1.1 多租户系统 ##

EaseMob 用户体系在设计上支持多租户，即可以有多个互相隔离的组织机构同时使用同一套系统。

## 1.2 名称解释 ##

organization：组织机构。组织机构可以是一个公司账号，也可以是一个个人账号。以下简称Org。

application：App应用。每个组织机构可以在自己的账号下创建多个app应用。以下简称App。

user：App的用户。每个App可以有自己的用户。每个app的用户都是和其他app的用户隔离的。

## 1.3 Usergrid返回的response的数据结构。

请注意，在这个reponse的json数据中，只有"entities"才是真正的数据体，并且总是一个数组类型。

    {
        "action" : "post",                                     //请求类型，可以为get|post|put|delete
        "application" : "3bec9de0-6abe-11e3-8606-41e5b3c17b39",//请求的app的UUID.在usergrid中每个app即可通过他的appkey唯一标识，也可以通过他的uuid唯一标识
        "params" : { },                                        //请求的参数。
        "path" : "/users",                                     //请求的请求路径。
        "uri" : "http://210.76.97.29:8080/hoho/hoho/users",    //请求URI。
        "entities" : [ {                                       //response的数据体。总是一个数组。
          "uuid" : "98b746e0-6ac0-11e3-914b-0b6cc7bc5592",     //返回的数据entity的uuid
          "type" : "user",                                     //返回的数据entity的类型
          "created" : 1387686117965,                           //返回的数据entity的创建时间
          "modified" : 1387686117965,                          //返回的数据entity的最后更新时间
          "username" : "admin",                                //返回的数据entity的是user类型。username是user类型entity的主键。
          "activated" : true                                   //返回的数据entity的是否被激活
        } ],
        "timestamp" : 1387686117951,                           //请求的时间戳。
        "duration" : 207,                                      //请求所用时常
        "organization" : "hoho",                               //请求的app所属的org
        "applicationName" : "hoho"                             //请求的app的名字
    }

 
# 2. 组织机构（Org）管理 #
## 2.1 创建一个组织机构（org）及该组织机构的管理员账号 ##

##POST /management/organizations
- 描述: 创建一个新org，同时在该org下创建一个新用户
- 权限：超级管理员
- Url参数:无
- Request返回: 如果创建成功，返回200
- 错误代码：
- 示例：创建一个名为:"weiquan"的org, 并同时为这个org创建一个管理员。管理员的用户名为"weiquan"， 其注册邮件地址是admin@vokeji.com (用于找回密码)，密码为"weiquan123456"
    
    	curl -X POST "http://api.easemob.com/management/organizations" -d '{"organization":"weiquan","username":"weiquan","email":"admin@vokeji.com","password":"weiquan123456"}'


response:

    {
      "action" : "new organization",
      "status" : "ok",
      "data" : {
    "owner" : {
      "applicationId" : "00000000-0000-0000-0000-000000000001",
      "username" : "jliu1",
      "name" : "jliu1",
      "email" : "jopir@yahoo.com",
      "activated" : true,
      "disabled" : false,
      "properties" : { },
      "uuid" : "7f9ca48a-b2f5-11e2-a2da-5b5746e42e13",
      "adminUser" : true,
      "displayEmailAddress" : "jliu1 <jopir@yahoo.com>",
      "htmldisplayEmailAddress" : "jliu1 &lt;<a href=\"mailto:jopir@yahoo.c
    >jopir@yahoo.com</a>&gt;"
    },
    "organization" : {
      "name" : "org1",
      "uuid" : "812f2b6a-b2f5-11e2-805f-a9d048dc6b73"
    }
      },
      "timestamp" : 1367477825212,
      "duration" : 3559
    }

## 2.2 org admin管理员登陆并获取授权token  ##

     
- POST /management/token
- 描述: 登录并授权，获得一个token。
- 参数:
- 返回: 授权结果(json),其中access_token为授权后的token
- 示例：


		curl -X POST "http://api.easemob.com/management/token" -d '{"grant_type":"password","username":"jervisliu@gmail.com","password":"yan7312"}'
    


response:

    {"access_token":"YWMt4IYuoKpyEeKAVDvUzId7bAAAAT5QTmKK7SK9-DA3eqvCX9ISX7xN2rJHsoQ","expires_in":604800,"user":{"username":"jliu","email":"jervisliu@gmail.com","organizations":{"easemob":{"users":{"jliu":{"applicationId":"00000000-0000-0000-0000-000000000001","username":"jliu","name":"jliu","email":"jervisliu@gmail.com","activated":true,"disabled":false,"properties":{},"adminUser":true,"displayEmailAddress":"jliu <jervisliu@gmail.com>","htmldisplayEmailAddress":"jliu &lt;<a href=\"mailto:jervisliu@gmail.com\">jervisliu@gmail.com</a>&gt;","uuid":"635ef90a-a7f9-11e2-ad38-59f55259f326"}},"name":"easemob","applications":{"easemob/test1":"92a86160-a7f9-11e2-9b9f-05f910c95d9e","easemob/sandbox":"63b15ec0-a7f9-11e2-891d-4b59fb74c9dd"},"uuid":"637c1dfa-a7f9-11e2-a1f9-8b35fa40759c"}},"adminUser":true,"activated":true,"name":"jliu","applicationId":"00000000-0000-0000-0000-000000000001","uuid":"635ef90a-a7f9-11e2-ad38-59f55259f326","properties":{},"htmldisplayEmailAddress":"jliu &lt;<a href=\"mailto:jervisliu@gmail.com\">jervisliu@gmail.com</a>&gt;","displayEmailAddress":"jliu <jervisliu@gmail.com>","disabled":false}}

### 2.3 在Org下创建App ###
- 
- POST /management/organizations/${orgName}/applications
- 描述:
- 参数:
- 返回:
- 示例：

    curl -X POST -i -H "Authorization: Bearer YWMt4IYuoKpyEeKAVDvUzId7bAAAAT5QTmKK7SK9-DA3eqvCX9ISX7xN2rJHsoQ" "http://api.easemob.com/management/organizations/easemob/applications" -d '{"name":"qixin"}'

response

    {
    "action" : "new application for organization",
    "uri" : "http://163.177.200.107:8080/null/null",
    "entities" : [ {
    "uuid" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
    "type" : "application",
    "name" : "easemob/qixin",
    "created" : 1366614166517,
    "modified" : 1366614166517,
    "accesstokenttl" : null,
    "applicationName" : "qixin",
    "organizationName" : "easemob",
    "metadata" : {
      "collections" : {
        "assets" : {
          "title" : "Assets",
          "count" : 0,
          "name" : "assets",
          "type" : "asset"
        },
        "users" : {
          "title" : "Users",
          "count" : 0,
          "name" : "users",
          "type" : "user"
        },
        "events" : {
          "title" : "Events",
          "count" : 0,
          "name" : "events",
          "type" : "event"
        },
        "roles" : {
          "title" : "Roles",
          "count" : 0,
          "name" : "roles",
          "type" : "role"
        },
        "folders" : {
          "title" : "Folders",
          "count" : 0,
          "name" : "folders",
          "type" : "folder"
        },
        "activities" : {
          "title" : "Activities",
          "count" : 0,
          "name" : "activities",
          "type" : "activity"
        },
        "devices" : {
          "title" : "Devices",
          "count" : 0,
          "name" : "devices",
          "type" : "device"
        },
        "groups" : {
          "title" : "Groups",
          "count" : 0,
          "name" : "groups",
          "type" : "group"
        }
      }
    }
     } ],
     "data" : {
     "easemob/qixin" : "a2e433a0-ab1a-11e2-a134-85fca932f094"
     },
     "timestamp" : 1366614166487,
     "duration" : 174
    }

## 2.4 获取指定Org下的App列表 ##


- GET /management/organizations/${orgName}/applications
- 描述:获取应用列表
- 参数:
- 返回:
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMt4IYuoKpyEeKAVDvUzId7bAAAAT5QTmKK7SK9-DA3eqvCX9ISX7xN2rJHsoQ" "http://api.easemob.com/management/organizations/easemob/applications"
    
response
    
    {
    "action" : "get organization application",
    "data" : {
    "easemob/test1" : "92a86160-a7f9-11e2-9b9f-05f910c95d9e",
    "easemob/sandbox" : "63b15ec0-a7f9-11e2-891d-4b59fb74c9dd",
    "easemob/qixin" : "a2e433a0-ab1a-11e2-a134-85fca932f094"
    },
    "timestamp" : 1366614166925,
    "duration" : 3
    }

 
## 2.5 获取指定Org下的用户列表（即获取该Org的管理员用户列表） ##

- GET /management/organizations/${orgName}/users
- 描述:
- 参数:
- 返回: orgName, authentication token
- 示例：
- 

    curl -X GET -i -H "Authorization: Bearer YWMt7Yo7wLL0EeKOmhfmlOylrwAAAT6IEHV89wf4rvv3R3_ZZW7NJ43N-nygsnY" "http://163.177.200.107:8080/management/organizations/easemob/users?querypar…"

    {
    "action" : "get organization users",
    "data" : [ {
    "applicationId" : "00000000-0000-0000-0000-000000000001",
    "username" : "jliu",
    "name" : "jliu",
    "email" : "jervisliu@gmail.com",
    "activated" : true,
    "disabled" : false,
    "properties" : { },
    "uuid" : "635ef90a-a7f9-11e2-ad38-59f55259f326",
    "adminUser" : true,
    "displayEmailAddress" : "jliu <jervisliu@gmail.com>",
    "htmldisplayEmailAddress" : "jliu &lt;<a href=\"mailto:jervisliu@gmail.com\"
    >jervisliu@gmail.com</a>&gt;"
     } ],
     "timestamp" : ,
     "duration" : 10
    }

 
# 3. App 管理 #
## 3.1. App改名 ##

- PUT /${organizationName}/${applicationName}
- 描述:应用改名. 注：applicationName是app的唯一标识名，是不能更改的。我们使用的应用名是app的一个叫"title"的属性.改名实际上是修改title属性
- 参数:
- 返回:
- 示例：
   
    curl -X PUT  -H "Authorization: Bearer YWMt7Yo7wLL0EeKOmhfmlOylrwAAAT6IEHV89wf4rvv3R3_ZZW7NJ43N-nygsnY" "http://api.easemob.com/easemob/qixin" -d '{"title":"myappnamenew"}'

Response:

    {
      "action" : "put",
      "application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
      "params" : { },
      "uri" : "http://localhost:8080/easemob/qixin",
      "entities" : [ {
    "uuid" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
    "type" : "application",
    "name" : "easemob/qixin",
    "created" : 1366614166517,
    "modified" : 1367479037536,
    "title" : "myappnamenew",
    "accesstokenttl" : null,
    "applicationName" : "qixin",
    "organizationName" : "easemob",
    "metadata" : {
      "collections" : [ "activities", "assets", "devices", "events", "folders",
    "groups", "roles", "users" ]
    }
      } ],
      "timestamp" : 1367479037525,
      "duration" : 69,
      "organization" : "easemob",
      "applicationName" : "qixin"
    }

## 3.2 获取指定App详情 ##


- GET /management/organizations/${orgName}/applications/${appName}
- 描述:
- 参数:
- 返回: orgName, appName, authentication token
- 示例：


    curl -X GET -H "Authorization: Bearer YWMt7Yo7wLL0EeKOmhfmlOylrwAAAT6IEHV89wf4rvv3R3_ZZW7NJ43N-nygsnY" " http://api.easemob.com/management/organizations/easemob/applications/qixin"

response:

    {
    "action" : "get",
    "application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
    "params" : { },
    "uri" : "http://localhost:8080/easemob/qixin",
    "entities" : [ {
    "uuid" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
    "type" : "application",
    "name" : "easemob/qixin",
    "created" : 1366614166517,
    "modified" : 1367479037536,
    "title" : "myappnamenew",
    "accesstokenttl" : null,
    "applicationName" : "qixin",
    "organizationName" : "easemob",
    "metadata" : {
      "collections" : {
    "assets" : {
      "title" : "Assets",
      "count" : 0,
      "name" : "assets",
      "type" : "asset"
    },
    "users" : {
      "title" : "Users",
      "count" : 0,
      "name" : "users",
      "type" : "user"
    },
    "events" : {
      "title" : "Events",
      "count" : 0,
      "name" : "events",
      "type" : "event"
    },
    "roles" : {
      "title" : "Roles",
      "count" : 3,
      "name" : "roles",
      "type" : "role"
    },
    "folders" : {
      "title" : "Folders",
      "count" : 0,
      "name" : "folders",
      "type" : "folder"
    },
    "activities" : {
      "title" : "Activities",
      "count" : 0,
      "name" : "activities",
      "type" : "activity"
    },
    "devices" : {
      "title" : "Devices",
      "count" : 0,
      "name" : "devices",
      "type" : "device"
    },
    "groups" : {
      "title" : "Groups",
      "count" : 0,
      "name" : "groups",
      "type" : "group"
    }
      }
    }
      } ],
      "timestamp" : 1367479362833,
      "duration" : 7,
      "organization" : "easemob",
      "applicationName" : "qixin"
    }


# 4. 用户（Users）管理 #

## 4.1 app用户登录并获取授权token ##

    
- POST /${orgName}/${appName}/token
- 描述: 登录并授权，获得一个token。
- 参数:
- 返回: 授权结果(json),其中access_token为授权后的token
- 示例：


    curl -X POST "http://api.easemob.com/easemob/qixin/token" -d '{"grant_type":"password","username":"jliu1","password":"jliu1"}'

 

如果这个用户之前已经注册了, 并且这里提供的密码正确的话, 会返回

    {

        "access_token":"YWMtNda4DFzyEeOrOy_LuVzHjAAAAULiG1IrN8opggpytUDFmJkiocawbINICYk",

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

2. 用户的具体信息, app可以根据这个信息来构造一个user对象, 真实的环境中, 这个还会包含用户的头像/性别/email等等, 取决于注册的时候提供了哪些信息

 
## 4.2 创建app的用户 ##


- POST /${orgName}/${appName}/users
- 描述:创建一个新的app user
- 权限: application access
- 参数: "email","username","name" 匀不能已存在
- 示例：


    curl -X POST -i "http://api.easemob.com/easemob/qixin/users" -d '{"username":"jliu0003","password":"jliu0002"}'

response:

    {
      "action" : "post",
      "application" : "a2e433a0-ab1a-11e2-a134-85fca932f094",
      "params" : { },
      "path" : "/users",
      "uri" : "http://api.easemob.com/easemob/qixin/users",
      "entities" : [ {
    "uuid" : "7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
    "type" : "user",
    "name" : "jliu0003",
    "created" : 1368377620796,
    "modified" : 1368377620796,
    "username" : "jliu0003",
    "email" : "jliu0003@easemob.com",
    "activated" : true,
    "picture" : "http://www.gravatar.com/avatar/84c7ecaead2b6a1f8ff468f435db4ce6
    ",
    "metadata" : {
      "path" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
      "sets" : {
    "rolenames" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/rolenames",
    "permissions" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/permissions
    "
      },
      "collections" : {
    "activities" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/activities",
    
    "devices" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/devices",
    "feed" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/feed",
    "groups" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/groups",
    "roles" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/roles",
    "following" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/following",
    "followers" : "/users/7f90f7ca-bb24-11e2-b2d0-6d8e359945e4/followers"
      }
    }
      } ],
      "timestamp" : 1368377620793,
      "duration" : 125,
      "organization" : "easemob",
      "applicationName" : "qixin"
    }
    
 

注：这里, username 和password 是必须提供的, 除此之外, 还可以增加任意别的属性, 例如, 在用户注册页面让用户填写性别和email地址的话, 那么post的数据就会是

    curl -X POST -i "http://api.easemob.com/easemob/qixin/users" -d '{"username":"jliu0003","password":"jliu0002", "sex":"male", "email":"stliu@apache.org"}'

注：创建用户是不需要授权的。所以不需要传入token。

## 4.3 获取app的用户总数量 ##

- GET /${orgName}/${appName}/users
- 描述:
- 参数:
- 返回: orgName, authentication token
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/counters?counter=application.collectio…"



## 4.4 获取app的用户列表 ##

- GET /${orgName}/${appName}/users
- 描述:
- 参数:
- 返回: orgName, authentication token
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/users"

## 4.5 获取app的指定用户详情 ##


- GET /${orgName}/${appName}/users/${userName}
- 描述:
- 参数:
- 返回: orgName, authentication token
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/users/jliu1"

## 4.6 查找用户 ##


- GET /${orgName}/${appName}/users
- 描述:
- 参数:
- 返回: 
- 示例：

例1： 根据用户手机号查找用户

    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/users?ql=select * where mobile='13800138000'"

例2： 根据用户昵称的部分查找用户

    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/users?ql=select * where nick contains '刘'"

## 4.7 查询app的现有用户总数 ##
 
- GET /${orgName}/${appName}/counters
- 描述:
- 参数:counter=application.collection.users
- 返回: 
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/counters?counter=application.collection.users"

## 4.8 删除app的指定用户 ##
 
- DELETE /${orgName}/${appName}/users/${userName}
- 描述:
- 参数:
- 返回: orgName, authentication token
- 示例：

    curl -X DELETE -i -H "Authorization: Bearer YWMtP_8IisA-EeK-a5cNq4Jt3QAAAT7fI10IbPuKdRxUTjA9CNiZMnQIgk0LEUE" "http://api.easemob.com/easemob/qixin/users/ligangying"

## 4.9 更新App的用户信息 ##

- GET /${orgName}/${appName}/users
- 描述: 输入参数中必须有username属性（username是user的primary key)
- 参数:
- 返回:
- 示例：



    curl -X PUT -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" "http://api.easemob.com/easemob/qixin/users/jliu" -d '{   "username" : "jliu", "usertype" : "customer"}'



上面的这个例子是为叫"jliu"的用户增加一个叫usertype的属性，其值为customer。比如以前的user属性设计里并没有用户类型这个属性，现在想区分每个用户的类型，比如这个用户可以是”普通客人”或“客服“或”设计师“。那么只需要调用以上接口即可为jliu用户增加这个属性。如果用户已经有了usertype这个属性，那么以上接口调用会更新usertype的属性值。

 
## 4.10 自定义用户属性 ##

用户属性可以根据app的需要自行定义，以满足不同app的需求。比如一个交友应用可以定义身高，体重，个人相册等属性。一个企业应用可以定义座机，传真，部门等属性。因为EaseMob Baas的后台是一个No SQL的DB, 所以对用户属性没有限制。


比如下面的例子是一个交友应用的用户数据：

    {
    "uuid" : "7f90f7ca-bb24-11e2-b2d0-6d8e359945e4",
    "type" : "user",
    "name" : "jliu0003",
    "created" : 1368377620796,
    "modified" : 1368377620796,
    "username" : "jliu0003",
    "email" : "jliu0003@easemob.com",
    "activated" : true,
    "avator" : "http://www.gravatar.com/avatar/84c7ecaead2b6a1f8ff468f435db4ce6"

    "nick" : "jervis liu",

     "age" : 30,

     "album" : ["http://www.gravatar.com/avatar/84c7ecaead2b6a1f8ff468f435db4ce6"]

     }


## 4.11 生成随机用户id ##
很多app都需要为用户产生一个账号，比如陌陌号，qq号。这个账号一般是一个数字（便于记忆，便于告诉他人）。这个账号一般不能用自增长的数字，而需要是一个随机数字，以避免他人猜到账号，或者根据账号数字猜测app的总用户数

- GET 
- 描述: 
- 参数:
- 返回:
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR_9OcNq-GwPsKwj_TruuxZfFSC2eIQ" -H "appkey: linjushuo" "http://223.202.120.59:7874/id/users"

## 4.12 根据用户手机号登陆（通过手机号查找用户） ##
很多app都是通过用户号登陆。但在实际系统中，我们并不能用手机号作为用户账号，因为这样会导致用户手机号码泄露。通过手机号登陆的实际操作过程是先通过用户手机号查询用户账号，然后再通过用户账号登陆。

- GET 
- 描述: 
- 参数:
- 返回:
- 示例：


    curl -X GET -i -H "Authorization: Bearer YWMt39RfMMOqEeKYE_GW7tu81AAAAT71lGijyjG4VUIC2AwZGzUjVbPp_4qRD5k" "http://api.easemob.com/easemob/qixin/users?ql=select * where mobile='13800138000'"

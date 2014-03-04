---
title: Easemob 组织机构管理
description: Easemob组织机构管理
category: push
layout: docs
---
#  概述 #


 
#  组织机构管理 #
## 创建部门 ##

POST /management/organizations
描述: 创建一个新org，同时在该org下创建一个新用户
参数:
返回: 如果创建成功，返回200
示例：创建一个名为:"weiquan"的org, 并同时为这个org创建一个管理员。管理员的用户名为"weiquan"， 其注册邮件地址是admin@vokeji.com (用于找回密码)，密码为"weiquan123456"


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

## 2.2 删除部门  ##

     
POST /management/token
描述: 登录并授权，获得一个token。
参数:
返回: 授权结果(json),其中access_token为授权后的token
示例：

    curl -X POST "http://api.easemob.com/management/token" -d '{"grant_type":"password","username":"jervisliu@gmail.com","password":"yan7312"}'

response:

    {"access_token":"YWMt4IYuoKpyEeKAVDvUzId7bAAAAT5QTmKK7SK9-DA3eqvCX9ISX7xN2rJHsoQ","expires_in":604800,"user":{"username":"jliu","email":"jervisliu@gmail.com","organizations":{"easemob":{"users":{"jliu":{"applicationId":"00000000-0000-0000-0000-000000000001","username":"jliu","name":"jliu","email":"jervisliu@gmail.com","activated":true,"disabled":false,"properties":{},"adminUser":true,"displayEmailAddress":"jliu <jervisliu@gmail.com>","htmldisplayEmailAddress":"jliu &lt;<a href=\"mailto:jervisliu@gmail.com\">jervisliu@gmail.com</a>&gt;","uuid":"635ef90a-a7f9-11e2-ad38-59f55259f326"}},"name":"easemob","applications":{"easemob/test1":"92a86160-a7f9-11e2-9b9f-05f910c95d9e","easemob/sandbox":"63b15ec0-a7f9-11e2-891d-4b59fb74c9dd"},"uuid":"637c1dfa-a7f9-11e2-a1f9-8b35fa40759c"}},"adminUser":true,"activated":true,"name":"jliu","applicationId":"00000000-0000-0000-0000-000000000001","uuid":"635ef90a-a7f9-11e2-ad38-59f55259f326","properties":{},"htmldisplayEmailAddress":"jliu &lt;<a href=\"mailto:jervisliu@gmail.com\">jervisliu@gmail.com</a>&gt;","displayEmailAddress":"jliu <jervisliu@gmail.com>","disabled":false}}

asemob/qixin" -d '{"title":"myappnamenew"}'

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


## 2.3 获取完整的组织架构树  ##

## 2.4 修改员工部门属性  ##
GET /${orgName}/${appName}/users 
描述: 输入参数中必须有username属性（username是user的primary key) 
参数: 
返回: 
示例： 
    curl -X PUT -i -H "Authorization: Bearer YWMtxc6K0L1aEeKf9LWFzT9xEAAAAT7MNR9OcNq-GwPsKwjTruuxZfFSC2eIQ" "http://api.easemob.com/easemob/qixin/users/jliu" -d '{ "username" : "jliu", "usertype" : "customer"}'

上面的这个例子是为叫"jliu"的用户增加一个叫usertype的属性，其值为customer。比如以前的user属性设计里并没有用户类型这个属性，现在想区分每个用户的类型，比如这个用户可以是”普通客人”或“客服“或”设计师“。那么只需要调用以上接口即可为jliu用户增加这个属性。如果用户已经有了usertype这个属性，那么以上接口调用会更新usertype的属性值。
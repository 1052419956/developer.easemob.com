---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---
首先, 需要明确的是, role, permission 都是api service里面的系统定义的entity, 也就是说你可以把他们当成一般的entity来操作

也就是意味着, 可以用

GET /:org/:app/roles 来查看当前app中的roles

GET /:org/:app/permissions 来查看当前app中定义的permissions

同样的也可以post等增加和修改已有的.

并且这些都是动态随时可以调整的, 用户每次的request请求都会经过role/permission检验, 所以一个例子是可以弄一个网页, 来让公司的管理员来自己定义role, permission, 并且让公司管理员来决定给那些员工哪些权限等等的 ( http://apigee.com/docs/usergrid/content/roles-and-permissions 这里有些图片 )


每个App默认都有三个role

* Guest: The guest role for unauthenticated users
* Default: The default role for authenticated users
* Administrator: An empty role  

(尝试一下 GET /roles)

可以看到每个role的name, 所以, 如同所有的entity一样, 也可以使用 /roles/default 来定位到 default role

也可以访问 /roles/default/permissions 来查看这个role的所有permissions

    {
      "action": "get",
      "application": "a2e433a0-ab1a-11e2-a134-85fca932f094",
      "params": {
        "_": [
          "1369886584450"
        ]
      },
      "uri": "http://api.easemob.com/easemob/qixin",
      "entities": [],
      "data": [
        "get,put,post,delete:/**"
      ],
      "timestamp": 1369886586648,
      "duration": 4,
      "organization": "easemob",
      "applicationName": "qixin"
    }
     
     
创建一个新的role:

    post /roles {"name":"test-employee","roleName":"test-employee","title":"test-employee"} 
    
使用delete来删除一个role (和其它的entity一样)        

    delete /roles/test-employee

创建了这个role之后, 让我们来看看它的permission

    /roles/test-employee/permissions
    
可以看到返回结果是空的, 这个role还没有任何的permission    
    
给这个role添加permission

       post /roles/test-employee/permissions { "permission" : "get,put,post,delete:/users/me/groups" }
       
注意, 这里的值也可以是个数组, 类似

    post /roles/test-employee/permissions { "permission" : ["get,put,post,delete:/users/me/groups","get:/**"] }       
       
       
可以看到返回结果 

    /roles/test-employee/permissions
     
    {
      "action": "post",
      "application": "a2e433a0-ab1a-11e2-a134-85fca932f094",
      "params": {},
      "uri": "http://api.easemob.com/easemob/qixin",
      "entities": [],
      "data": [
        "get,put,post,delete:/users/me/groups"
      ],
      "timestamp": 1369887061207,
      "duration": 9,
      "organization": "easemob",
      "applicationName": "qixin"
    }       
    

从一个role中删除一个permission还有点问题好像当前


给user设置一个role (也可以给一个group设置一个role, 然后把user添加到一个group当中)

    POST /{org_id}/{app_id}/roles/{role_id}/users/{uuid|username}
    
    or
    
    POST /{org_id}/{app_id}/users/{uuid|username}/roles/{role_id}
    
例如  `post /roles/test-employee/users/take` 相当于给user take赋予了 test-employee的role

这时候在访问 get /users/take/rolenames

    /users/take/rolenames
     
    {
      "action": "get",
      "application": "a2e433a0-ab1a-11e2-a134-85fca932f094",
      "params": {
        "_": [
          "1369887953963"
        ]
      },
      "uri": "http://api.easemob.com/easemob/qixin",
      "entities": [],
      "data": {
        "test-employee": {
          "type": "role",
          "name": "test-employee",
          "roleName": null,
          "title": "test-employee",
          "inactivity": 0
        }
      },
      "timestamp": 1369887954146,
      "duration": 3,
      "organization": "easemob",
      "applicationName": "qixin"
    }    

一个user可以有多个role的, 例如再执行一次 `post /roles/admin/users/take`, 然后调用 `get /users/take/rolenames`

    /users/take/rolenames
     
    {
      "action": "get",
      "application": "a2e433a0-ab1a-11e2-a134-85fca932f094",
      "params": {
        "_": [
          "1369888014456"
        ]
      },
      "uri": "http://api.easemob.com/easemob/qixin",
      "entities": [],
      "data": {
        "admin": {
          "type": "role",
          "name": "admin",
          "roleName": null,
          "title": "Administrator",
          "inactivity": 0
        },
        "test-employee": {
          "type": "role",
          "name": "test-employee",
          "roleName": null,
          "title": "test-employee",
          "inactivity": 0
        }
      },
      "timestamp": 1369888014708,
      "duration": 6,
      "organization": "easemob",
      "applicationName": "qixin"
    }
     

同样的, 也可以通过 /roles/${role_name}/users or groups 来查看某一个role下面有哪些user或者group


    delete /roles/${role_name}/users/${username}
    
    撤销一个user所拥有的某项role
    
Permission examples

Here are some examples to illustrate how permissions are specified:

A permission to create a user:

    post:/users/*

A permission that allows someone to look at a specific user:

    get:/users/john.doe

A permission that allows the current user to see his activity feed:

    get:/users/${user}/feed/*

The ${user} in the entity path refers to a variable that represents the current user’s UUID.

A permission allowing linked entities to be read:
    
    get:/users/${user}/**

The /** in the entity path is a wildcard that matches everything under that path. This means that the full specification matches multiple resource paths, including, but not limited to, the following:

    /users/${user}/feed
    /users/${user}/feed/item1/a/b/c

A permission that allows the current user to add himself or another user to a group:
    
    post:/groups/${user}/users/**
    


           
---
title: 易用户
description: EM用户体系
category: emuser
layout: docs
---

 
#  组织机构管理 #

## 组织机构的数据结构

      { 
          "path" : "这个是组织机构的primarykey. 必须是英文字母数字下划线横线的组合，规则如下："^[a-z0-9_-]{3,15}$",
          "fullname" : "组织机构名称，可以为中文，可以修改。",
          "parent" : "父节点的path"
      }

## 创建部门 ##

### POST /${orgName}/${appName}/groups

* 描述: 创建一个新的部门
* 权限: app用户登录
* Url参数: 无
* Request body: 
  
      { 
          "path" : "这个是组织机构的primarykey. 必须是英文字母数字下划线横线的组合，规则如下："^[a-z0-9_-]{3,15}$",
          "fullname" : "组织机构名称，可以为中文，可以修改。",
          "parent" : "父节点的path。如果没有父节点，则用/表示"
      }

* 错误代码：
     
     401 Unauthorized： 认证失败。返回的response body为json数据：
     
        {
         "error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }

    400 Bad Request: 如果要创建的部门path已经存在。返回的response body为json数据：       
        
        Entity group requires that property named name be unique, value of 1 exists
        
* curl示例：

        curl -X POST -H "Authorization: Bearer YWMtI-n89nqGEeOOXyXq3bWeNwAAAUOj8rinBuWhDVWIeFPgEotHz6AOCflJ0AA" -i "http://api.easemob.com/ljs/ljs/groups" -d '{ "path" : "865320001"，"fullname" : "移动开发部",  "parent" : "/"}'
  
返回的json数据的entities包含了新创建的部门.


     {
        "action" : "post",
        "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
        "params" : { },
        "path" : "/groups",
        "uri" : "http://api.easemob.com/ljs/ljs/communities",
        "entities" : [ {
        "uuid" : "73645a1a-7b30-11e3-b708-f7da4a11895a",
        "type" : "group",
        "path" : "865320001",
        "created" : 1389493377585,
        "modified" : 1389493377585,
        "parent" : "No 5, laoshan road",
        "fullname" : "qingdao",
        "duration" : 21,
        "organization" : "ljs",
        "applicationName" : "ljs"
    }


## 删除部门  ##

### DELETE /${orgName}/${appName}/groups/${部门path}

* 描述:删除一个部门
* 权限: app用户登录
* Url参数: 无
* Request body: 无
* 错误代码：

    401 Unauthorized： 认证失败。返回的response body为json数据：

        {
         "error":"expired_token",
         "timestamp":1389492673539,
         "duration":0,
         "exception":"org.usergrid.rest.exceptions.SecurityException",
         "error_description":"Unable to authenticate due to expired access token"
        }
        
    404 Not Found: 要删除的b部门不存在。返回的response body为json数据：       
    
        {"error":"service_resource_not_found",
        "timestamp":1389494177532,
        "duration":1,
        "exception":"org.usergrid.services.exceptions.ServiceResourceNotFoundException",
        "error_description":"Service resource not found"
        }     
   
* curl示例：

        curl -X DELETE -H "Authorization: Bearer YWMtI-n89nqGEeOOXyXq3bWeNwAAAUOj8rinBuWhDVWIeFPgEotHz6AOCflJ0AA" -i "http://api.easemob.com/ljs/ljs/groups/865320001" 
  
返回的json数据的entities包含了被删除的部门基本信息.


    {
        "action" : "delete",
        "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
        "params" : { },
        "path" : "/865320001",
        "uri" : "http://api.easemob.com/ljs/ljs/865320001",
        "entities" : [ {
            "uuid" : "94c80fd4-7b31-11e3-928c-4552c8e41bcf",
            "fullname" : "移动事业部",
            "created" : 1389493863100,
            "modified" : 1389493863100,
            "path" : "865320000"
        } ],
        "timestamp" : 1389494163387,
        "duration" : 59,
        "organization" : "ljs",
        "applicationName" : "ljs"
    }


## 修改部门    
### PUT /${orgName}/${appName}/groups/${部门path}

* 描述:更新部门信息
* 权限: app用户登录
* Url参数: 无
* Request body: 
  
    {
    "parent" : "/移动事业部" //这里把部门的父节点改为"/移动事业部"。注意，不需要修改的属性可以不出现在request中。
    }
    
* 错误代码：
  
    401 Unauthorized： 认证失败。返回的response body为json数据：
    
        {
             "error":"expired_token",
             "timestamp":1389492673539,
             "duration":0,
             "exception":"org.usergrid.rest.exceptions.SecurityException",
             "error_description":"Unable to authenticate due to expired access token"
        }

* curl示例：

        curl -X PUT -H "Authorization: Bearer YWMtI-n89nqGEeOOXyXq3bWeNwAAAUOj8rinBuWhDVWIeFPgEotHz6AOCflJ0AA" -i "http://api.easemob.com/ljs/ljs/groups/865320000" -d '{ "parent" : "/集团总公司" }'

返回的json数据的entities包含了更新后的部门信息.


    {
        "action" : "put",
        "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
        "params" : { },
        "path" : "/groups",
        "uri" : "http://api.easemob.com/ljs/ljs/groups",
        "entities" : [ {
            "uuid" : "73645a1a-7b30-11e3-b708-f7da4a11895a",
            "type" : "group",
            "fullname" : "2",
            "created" : 1389493377585,
            "modified" : 1389493948631,
            "parent" : "/集团总公司",
        } ],
        "timestamp" : 1389493948624,
        "duration" : 16,
        "organization" : "ljs",
        "applicationName" : "ljs"
    }

注：如果要修改的部门不存在，则会创建一个新部门

## 获取部门列表  ##
### GET /${orgName}/${appName}/groups
* 描述:获得部门列表
* 权限: app用户登录
* Url参数: 
* Request body: 无
* 错误代码：
* curl示例：

        curl -X GET -i "http://api.easemob.com/ljs/ljs/groups"

返回的json数据的entities包含了社区信息.


    {
        "action" : "get",
        "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
        "params" : { },
        "path" : "/groups",
        "uri" : "http://api.easemob.com/ljs/ljs/communities",
        "entities" : [ {
          "uuid" : "1b6140ba-7b2d-11e3-b312-07fe2d3e042b",
          "type" : "group",
          "name" : "1",
          "created" : 1389491941435,
          "modified" : 1389493838001,
          "parent" : "/集团总公司",
          "fullname" : "研发部",

        }, {
          "uuid" : "73645a1a-7b30-11e3-b708-f7da4a11895a",
          "type" : "group",
          "name" : "2",
          "created" : 1389493377585,
          "modified" : 1389493948631,
          "parent" : "/集团总公司",
          "fullname" : "人事部",
        } ],
        "timestamp" : 1389494635714,
        "duration" : 13,
        "organization" : "ljs",
        "applicationName" : "ljs",
        "count" : 2
    }
        
## 获得指定部门详情

### GET /${orgName}/${appName}/groups/${部门path}
* 描述:获得指定部门
* 权限: app用户登录
* Url参数: 无
* Request body: 无
* 错误代码：        

        404 Not Found: 指定的部门不存在。返回的response body为json数据：       
        {"error":"service_resource_not_found",
        "timestamp":1389494177532,
        "duration":1,
        "exception":"org.usergrid.services.exceptions.ServiceResourceNotFoundException",
        "error_description":"Service resource not found"
        }  
               
* curl示例：

        curl -X GET -i "http://api.easemob.com/ljs/ljs/groups/2"

返回的json数据的entities包含了部门信息.


    {
        "action" : "get",
        "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
        "params" : { },
        "path" : "/groups",
        "uri" : "http://api.easemob.com/ljs/ljs/groups",
        "entities" : [ {
          "uuid" : "73645a1a-7b30-11e3-b708-f7da4a11895a",
          "type" : "group",
          "name" : "2",
          "created" : 1389493377585,
          "modified" : 1389493948631,
          "parent" : "/集团总公司",
          "fullname" : "人事部",
        } ],
        "timestamp" : 1389494871246,
        "duration" : 7,
        "organization" : "ljs",
        "applicationName" : "ljs"
    }
       
## 修改员工部门属性  ##
### PUT /${orgName}/${appName}/users/${userName} ###
* 描述:修改用户的部门信息
* 权限: app用户登录
* Url参数: 无
* Request body: 
  
        {
          "department":"/集团总公司/研发部" 
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

        curl -X PUT -i -H "Authorization: Bearer YWMtI-n89nqGEeOOXyXq3bWeNwAAAUOj8rinBuWhDVWIeFPgEotHz6AOCflJ0AA" "http://api.easemob.com/ljs/ljs/users/admin" -d '{"usertype":"customer"}'
  
返回的json数据的entities包含了更新后的user.


    {
        "action" : "put",
        "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
        "params" : { },
        "path" : "/users",
        "uri" : "http://api.easemob.com/ljs/ljs/users",
        "entities" : [ {
          "uuid" : "d023ed4a-7b37-11e3-94a0-2769650b88ed",
          "type" : "user",
          "created" : 1389496539668,
          "modified" : 1389497741139,
          "username" : "admin",
          "activated" : true,
          "department" : "/集团总公司/研发部"
        } ],
        "timestamp" : 1389497741130,
        "duration" : 18,
        "organization" : "ljs",
        "applicationName" : "ljs"
    }


## 查询部门所属员工  ##
### GET /${orgName}/${appName}/groups/${部门path}/has/users
* 描述:获得部门所属员工
* 权限: app用户登录
* Url参数: 
* Request body: 无
* 错误代码：
* curl示例：

        curl -X GET -i "http://api.easemob.com/ljs/ljs/groups/123456/has/users"

返回的json数据的entities包含了该部门下的所有员工.


    {
    "action" : "get",
    "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
    "params" : { },
    "path" : "/users",
    "uri" : "http://api.easemob.com/ljs/ljs/users",
    "entities" : [ {
      "uuid" : "d023ed4a-7b37-11e3-94a0-2769650b88ed",
      "type" : "user",
      "created" : 1389496539668,
      "modified" : 1389497741139,
      "username" : "admin",
      "activated" : true,
      "usertype" : "customer"
    } ],
    "timestamp" : 1389497868539,
    "duration" : 6,
    "organization" : "ljs",
    "applicationName" : "ljs"
    }
        
       
     

## 按部门推送 ##

1. 在推送服务器上创建一个部门topic
1. 查询指定部门下的员工列表
2. 在部门topic上注册该部门的所有员工
4. 向部门topic上推送通知
5. 如果部门员工列表有变化，在topic上增加或删除用户
---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---
## Event

event 在 API Service当中主要是用来统计信息的(例如做app的数据分析), 例如一个功能被使用了多少次, 被哪些用户使用的多, 被哪个group的用户使用的多, 甚至每个event还可以添加一个location的属性, 这样就可以知道一些基于地理位置的信息, 例如那里的用户喜欢哪种功能.

Event可以和user / group关联起来, 来做精准的分析, 也可以给每个event指定一个category, 这样查询起来就能够按照类别来查询了.

Event还是用来存储计数器的主要手段.

### 创建Event

    POST /:org/:app/events {request body}
    
其中, request body可以是下面这样的内容

    {
      "timestamp":0,
      "category" : "advertising",
      "counters" : {
        "ad_clicks" : 5
      }
    }    

需要注意的是, "timestamp"属性是必须的, 但是如果这个属性的值是0的话, 系统会给它自动设置为当前值

上面的执行结果类似:

    {
      "action": "post",
      "application": "09621090-affb-11e2-ac08-773f1211b6de",
      "params": {},
      "path": "/events",
      "uri": "http://localhost:8080/test-organization/test1-app/events",
      "entities": [
        {
          "uuid": "7de5230a-d4b7-1f8c-8d5a-01bc0167a5d5",
          "type": "event",
          "created": 1368044791454,
          "modified": 1368044791454,
          "timestamp": 1368044791454,
          "category": "advertising",
          "counters": {
            "ad_clicks": 5
          },
          "message": null,
          "metadata": {
            "path": "/events/7de5230a-d4b7-1f8c-8d5a-01bc0167a5d5"
          }
        }
      ],
      "timestamp": 1368044791448,
      "duration": 39,
      "organization": "test-organization",
      "applicationName": "test1-app"
    }

对于event的request body, 系统还内置了如下的属性:

* user : 需要关联的user的UUID
* group : 需要关联的user group的UUID
* message : String, 这个event的描述信息

除了这些之外, 你也可以增加任何你想要自定义的属性 
    


## 应用数据统计

应用数据的统计是通过 _counter_ 来实现的, 具体的API就是

/{org_name}/{app_name}/counters

此API支持如下的参数:

1. start_time
    
    要获取数据的起始时间, long类型

2. end_time

    要获取数据的截止时间, long类型
    
3. resolution

    数据的时间间隔类型, 有all, minute, five_minute, half_hour, hour, six_hour, half_day, day, week, month
    
    这个时间间隔会对上面给出的start_time, end_time进行自动的修正
    
4. pad

    不知道, 但是必须有这个参数, 其值为'true'
    
5. counter

    要查询的counter

### Counter

当前系统中所支持的counter

* app.users.activity

    用户发出的request中包括了正确的token(即有用户调用)的时候, 此counter自动加1

* application.collection.restaurants

    restaurants 是用户自定义的集合, 也就是说可以通过这种形势来获取到每个用户数据的总数

* application.collection.roles

    当前应用中roles的总数

* application.collection.user:members

    N/A

* application.collection.users

    当前应用中user的总数

* application.entities

    当前应用中user的总数
    
* application.request.upload

    此应用通过request的方式一共上传了多少byte的数据

* application.request.download

    系统通过response的方式一共返回给了此应用多少byte的数据

* application.request.time

    此应用的request一共消耗了系统多少cpu时间(毫秒)

* application.requests.get

    此应用调用HTTP GET的次数

* application.requests.post

    此应用调用HTTP POST的次数

* application.requests.put

    此应用调用HTTP PUT的次数

下面的实例可供参考

"http://localhost:8080/test-organization/test1-app/counters?start_time=1366848000000&end_time=1209600001&resolution=day&counter=application.entities&counter=application.request.download&counter=application.request.time&counter=application.request.upload&pad=true"


    {
      "action" : "get",
      "application" : "09621090-affb-11e2-ac08-773f1211b6de",
      "params" : {
        "pad" : [ "true" ],
        "end_time" : [ "1209600001" ],
        "counter" : [ "application.entities", "application.request.download", "application.request.time", "application.request.upload" ],
        "start_time" : [ "1366848000000" ],
        "resolution" : [ "day" ]
      },
      "uri" : "http://localhost:8080/test-organization/test1-app",
      "entities" : [ ],
      "timestamp" : 1367423375607,
      "duration" : 10,
      "organization" : "test-organization",
      "applicationName" : "test1-app",
      "count" : 0,
      "counters" : [ {
        "name" : "application.entities",
        "values" : [ {
          "timestamp" : 1366848000000,
          "value" : 0
        }, {
          "timestamp" : 1366934400000,
          "value" : 0
        }, {
          "timestamp" : 1367020800000,
          "value" : 0
        }, {
          "timestamp" : 1367107200000,
          "value" : 4
        }, {
          "timestamp" : 1367193600000,
          "value" : 0
        }, {
          "timestamp" : 1367280000000,
          "value" : 2
        }, {
          "timestamp" : 1367366400000,
          "value" : 105
        } ]
      }, {
        "name" : "application.request.download",
        "values" : [ {
          "timestamp" : 1366848000000,
          "value" : 0
        }, {
          "timestamp" : 1366934400000,
          "value" : 0
        }, {
          "timestamp" : 1367020800000,
          "value" : 0
        }, {
          "timestamp" : 1367107200000,
          "value" : 21482
        }, {
          "timestamp" : 1367193600000,
          "value" : 35334
        }, {
          "timestamp" : 1367280000000,
          "value" : 133942
        }, {
          "timestamp" : 1367366400000,
          "value" : 437878
        } ]
      }, {
        "name" : "application.request.time",
        "values" : [ {
          "timestamp" : 1366848000000,
          "value" : 0
        }, {
          "timestamp" : 1366934400000,
          "value" : 0
        }, {
          "timestamp" : 1367020800000,
          "value" : 0
        }, {
          "timestamp" : 1367107200000,
          "value" : 28760
        }, {
          "timestamp" : 1367193600000,
          "value" : 4182
        }, {
          "timestamp" : 1367280000000,
          "value" : 104346
        }, {
          "timestamp" : 1367366400000,
          "value" : 366653
        } ]
      }, {
        "name" : "application.request.upload",
        "values" : [ {
          "timestamp" : 1366848000000,
          "value" : 0
        }, {
          "timestamp" : 1366934400000,
          "value" : 0
        }, {
          "timestamp" : 1367020800000,
          "value" : 0
        }, {
          "timestamp" : 1367107200000,
          "value" : 102
        }, {
          "timestamp" : 1367193600000,
          "value" : 0
        }, {
          "timestamp" : 1367280000000,
          "value" : 758
        }, {
          "timestamp" : 1367366400000,
          "value" : 10825
        } ]
      } ]
      
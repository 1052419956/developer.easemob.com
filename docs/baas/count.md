---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: baas
layout: docs
---
### 统计一周内每天的新增用户数量
GET /${org_name}/${app_name}/counters
* 描述:获得小区列表
* 权限: app授权
* Url参数: start_time=1388505600000&end_time=1390060800000&resolution=day&counter=application.collection.users&pad=true
* Request body: 无
* 错误代码：
* curl示例：

	curl -X GET -i -H "Authorization: Bearer YWMtC5jqgoAWEeOCFZWmJT9oxwAAAUPIZmzZbMU5tYkckVOPSI1LGWoA7PAQ9x8" "http://api.easemob.com/ljs/ljs/counters?start_time=1388505600000&end_time=1390060800000&resolution=day&counter=application.collection.users&pad=true"

返回的json数据的entities包含了统计数据.


	{
	  "action" : "get",
	  "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
	  "params" : {
	    "pad" : [ "true" ],
	    "end_time" : [ "1390060800000" ],
	    "counter" : [ "application.collection.users" ],
	    "start_time" : [ "1388505600000" ],
	    "resolution" : [ "day" ]
	  },
	  "uri" : "http://api.easemob.com/ljs/ljs",
	  "entities" : [ ],
	  "timestamp" : 1390031809955,
	  "duration" : 8,
	  "organization" : "ljs",
	  "applicationName" : "ljs",
	  "count" : 0,
	  "counters" : [ {
	    "name" : "application.collection.users",
	    "values" : [ {
	      "timestamp" : 1388448000000,
	      "value" : 0
	    }, {
	      "timestamp" : 1388534400000,
	      "value" : 0
	    }, {
	      "timestamp" : 1388620800000,
	      "value" : 0
	    }, {
	      "timestamp" : 1388707200000,
	      "value" : 0
	    }, {
	      "timestamp" : 1388793600000,
	      "value" : 0
	    }, {
	      "timestamp" : 1388880000000,
	      "value" : 0
	    }, {
	      "timestamp" : 1388966400000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389052800000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389139200000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389225600000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389312000000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389398400000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389484800000,
	      "value" : 2
	    }, {
	      "timestamp" : 1389571200000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389657600000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389744000000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389830400000,
	      "value" : 0
	    }, {
	      "timestamp" : 1389916800000,
	      "value" : 1
	    }, {
	      "timestamp" : 1390003200000,
	      "value" : 0
	    } ]
	  } ]
        }
        
这里需要注意/修改的是几个查询参数:

* start_time

	查询范围的开始时间

* end_time

	查询范围的结束时间

* resolution

	* 按天来统计数据的话就是 "day"
	* 按周来统计数据的话就是 "week"
	* 按月来统计数据的话就是 "month"
	
	
上面说的时间都是一个long类型的

> Returns the number of milliseconds since January 1, 1970, 00:00:00 GMT

在Java中可以通过这个方法获取

	java.util.Date#getTime	
	
所以, 如果想要查询本周内的每天的新增加用户量, 可以这么来做

1. 先获取当前的时间, 得到一个 *end_time*	
2. 在计算七天前的时间, 得到一个 *start_time*
3. 然后执行上面的查询


### 统计一周/一个月内每天的总用户数量
暂时同统计一周内每天的新增用户数量

### 统计一周/一个月内每天的活跃用户数量
暂时同统计一周内每天的新增用户数量


### 统计当前总用户数量
GET /${org_name}/${app_name}/counters
* 描述:获得当前总用户数量
* 权限: app授权
* Url参数: counter=application.collection.users
* Request body: 无
* 错误代码：
* curl示例：

       curl -X GET -i -H "Authorization: Bearer YWMtC5jqgoAWEeOCFZWmJT9oxwAAAUPIZmzZbMU5tYkckVOPSI1LGWoA7PAQ9x8" "http://api.easemob.com/ljs/ljs/counters?counter=application.collection.users"

返回的json数据的entities包含了统计数据.

	{
	  "action" : "get",
	  "application" : "339d73d0-7a86-11e3-8907-b7afdbf7c043",
	  "params" : {
	    "counter" : [ "application.collection.users" ]
	  },
	  "uri" : "http://api.easemob.com/ljs/ljs",
	  "entities" : [ ],
	  "timestamp" : 1390032656215,
	  "duration" : 7,
	  "organization" : "ljs",
	  "applicationName" : "ljs",
	  "count" : 0,
	  "counters" : [ {
	    "name" : "application.collection.users",
	    "values" : [ {
	      "timestamp" : 1,
	      "value" : 3
	    } ]
	  } ]
        }

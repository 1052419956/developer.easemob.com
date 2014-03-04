---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---
## 通用的查询条件

当执行GET的时候, 可以指定的查询参数

* filter

    一个查询可以有多个filter, 例如 `/:org/:app/users?filter="name='todd'"&filter="id>=10"&filter="id <= 20"`


## queue

### POST /:org/:app/queues/:queue_name {"id":1}

note: post的时候必须的有data, 否则会报错, 而data可以是任何东西, 上面的{"id":1} 仅仅是示例, 没必要有id属性

note2: queue是可以嵌套的, 也就是支持这样的语法

/:org/:app/queues/:queue_name1/:queue_name2/:queue_name3

并且, 并不要求:queue_name1在:queue_name2之前被创建好



### GET /:org/:app/queues/:queue_name 

返回结果类似 

    {
      "path": "/test4",
      "queue": "9ecae721-b2c9-3208-924c-d03908e75294",
      "messages": [
        {
          "id": 1,
          "timestamp": 1368037837063,
          "uuid": "60ade170-b80d-11e2-9685-f375fc8b3371"
        }
      ],
      "last": "60ade170-b80d-11e2-9685-f375fc8b3371",
      "consumer": "9ecae721-b2c9-3208-924c-d03908e75294"
    }
    
从queue中取消息的时候, 消息是按照先进先出的顺序排列的, 并且, 如果一个消息被从queue中取出了, 那么就会被从queue中删除掉, 也就是一个消息只能被取出一次.

查询参数:

* limit -- 限制每次取出的message数量
* start
* consumer -- 上面说了, 当一个消息被取出的时候, 这个消息会从queue中被删除, 但是如果指定了consumer的话, 那么这个消息会一直留在queue中, 并且可以一直被这个consumer获取
* last
* time
* pos -- 可选值: start, end, last, consumer

    * start 

* update
* synchronized
* timeout
* filter

    任何查询都可以

### GET /:org/:app/queues/:queue_name/properties

可以用来获取这个queue的基本信息, 返回内容如下:

    /queues/test5/properties
     
    {
      "created": 1368039729095,
      "modified": 1368039968979,
      "newest": "57667230-b812-11e2-a98f-f19a8cce16b5",
      "oldest": "c86b0d70-b811-11e2-ad74-fbcea2a0fc92"
    } 

需要注意的是, 在执行这个的时候, 需要确认:queue_name是存在的, 否则会报错

### PUT /:org/:app/queues/:queue_name/properties

可以通过PUT的方法来给这个queue增加自定义的属性

例如, 可以给它增加一个name的属性

    put /queues/test5/properties {"name":"test5-queue"}
    
返回结果如下:

    {
      "name": "test5-queue",
      "path": "/test5"
    }

再次执行GET的话, 就会得到

    {
      "created": 1368039729095,
      "modified": 1368040489047,
      "name": "test5-queue",
      "newest": "57667230-b812-11e2-a98f-f19a8cce16b5",
      "oldest": "c86b0d70-b811-11e2-ad74-fbcea2a0fc92",
      "path": "/test5"
    }
     
### PUT /:org/:app/queues/:queue_name/subscribers/:subscriber_name

一个queue可以有多个订阅者(这个订阅者其实也是一个queue), 这样, 当一个message被发送到这个queue的时候, 此message会自动的被发送到此queue的各个订阅者queue中

使用这个PUT (或者POST也一样)可以为:queue_name增加一个订阅者

或者也可以通过这样的方法为一个queue添加订阅者

* put /:org/:app/queues/:queue_name/subscribers {"subscriber" : "s1"}     
* put /:org/:app/queues/:queue_name/subscribers {"subscribers" : ["s1","s2","s3"]}  

注意, 一个订阅者可以订阅多个queue, 这样发送到每个queue的消息都会被拷贝到这个订阅者的queue当中


### GET /:org/:app/queues/:queue_name/subscribers

查看一个queue有那些订阅者

### DELETE /:org/:app/queues/:queue_name/subscribers/:subscriber_name

取消订阅

### GET /:org/:app/queues/:queue_name/subscriptions

查看一个queue(这是一个订阅者queue)都订阅了哪些queue


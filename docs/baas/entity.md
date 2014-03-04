---
title: Easemob推送
description: Easemob推送, 在1秒内推送上万条数据, 并且支持tag等多维度推送
category: push
layout: docs
---
API Service是一个基于REST协议的云服务平台.

REST协议主要定义以下四种方法:

* GET -- 获取一个资源
* POST -- 创建一个资源
* PUT -- 修改
* DELETE -- 删除

在API Service所提供的数据服务中, 有两个基本的概念:

* Entity
* Collection

其中, API Service中的任何数据都是Entity, 而相同类型的entity被保存在这个类型的collection当中.

每个Entity都有一些系统自动创建的属性, 除了这些之外, 你可以在entity中添加任意自定义的属性

Entity的系统属性包括:

* uuid

    系统会为新创建的entity自动分配一个全局唯一的UUID, 这个UUID通常用来定位此entity, 注意, 这个是不能修改的

* type

    此entity的类型, 见下面collection部分的内容

* created

    timestamp

* modified

    timestamp

* metadata

    此entity的元数据, 系统默认包含的是_path_, 例如`"path": "/cats/1a7c2177-67cb-11e1-8223-12313d14bde7"`
    
###  创建entity或者collection

创建一个新的entity或者collection很简单, 可以直接使用POST方法来创建, 并且, 不需要在创建entity之前先创建一个collection, 如果在创建entity的时候, 指定的collection还不存在的话, 那么它是会被自动创建的

    POST /:org/:app/:collection_name {request body}
    
其中, collection_name可以是任意字符串, 而request body则是任意合法的JSON数据, 或者JSON 数组, 所以不仅仅局限于键值对, 一个entity也可以包含子entities.

例如 

    curl -X POST "http://api.easemob.com/test-organization/test1-app/cat" -d '{"name":"nico"}'
    
    curl -X POST "https://api.easemob.com/test-organization/test1-app/cat" -d '[ {"name":"mopsy"},{"name": "topsy"}]
    
返回值为

    {
      "action": "post",
      "application": "09621090-affb-11e2-ac08-773f1211b6de",
      "params": {},
      "path": "/cats",
      "uri": "http://localhost:8080/test-organization/test1-app/cats",
      "entities": [
        {
          "uuid": "c63208fa-b8b7-11e2-9c8d-bdb44a06819f",
          "type": "cat",
          "name": "nico",
          "created": 1368111021823,
          "modified": 1368111021823,
          "metadata": {
            "path": "/cats/c63208fa-b8b7-11e2-9c8d-bdb44a06819f"
          }
        }
      ],
      "timestamp": 1368111021751,
      "duration": 113,
      "organization": "test-organization",
      "applicationName": "test1-app"
    }
         
在返回值中我们可以看到比较有意思的一点, 我们是在 /cat 这个collection创建的entity, 而返回值确是 _/cats_, 这是因为API Service会自动的把collection的名字变成复数, 但是, 继续使用单数也是可以的.

对于任何用户创建的entity, 你都可以提供一个_name_属性, 然后使用这个name的值来定位这个entity, 而不是使用此entity的UUID (注意, 这个name必须在其collection中是唯一的)


### 修改一个Entity

使用PUT方法来修改一个entity, 类似于POST方法, 不同的是, 如果request body中包含的属性, 如果已经存在于entity当中, 则覆盖原值, 如果不存在则添加进去

这里有个例外, 就是json中的子entity, 如果一个子entity的属性在PUT上来的数据中不存在的话, 那么, 它是会被删除的

举个例子, 首先, 我们post下面的json来创建一个entity

    {
    name : "stliu",
    age : 29,
    teams : {
            hibernate : "opensource",
            easemob : "commercial"
        }
    }

然后我们PUT一个新的json到上面的entity

    {
    age : 45,
    }

这个时候, 系统中存储的json会被更新为

    {
    name : "stliu",
    age : 45,
    teams : {
            hibernate : "opensource",
            easemob : "commercial"
        }
    }

下面让我们再PUT一次, 这次更新teams这个子entity

    {
    age : 50,
    team : {
        hibernate : "commercial"
        }
    }

现在系统中的JSON会被更新为

    {
    name : "stliu",
    age : 50,
    teams : {
            hibernate : "commercial"
        }
    }
    
可以看到原来的 teams.easemob被删除掉了    

### 删除一个Entity    
    
    DELETE /:org/:app/:collection/:entity_uuid
    DELETE /:org/:app/:collection/:entity_name
         


### 获取一个Entity

    GET /:org/:app/:collection/:entity_uuid
    GET /:org/:app/:collection/:entity_name
    
注:User是一个系统定义的entity, 它是使用_username_属性来定位的, 而不是_name_属性

在GET的时候, 还可以使用查询条件来一次获取多个entity, 使用方法类似

    GET /:org/:app/:collection?ql={query string}

注, 如果没有查询条件,那么系统默认会返回10个, 可以通过limit查询参数来修改, 例如

    GET /:org/:app/test1s?limit=20
        
注, PUT的时候也可以使用查询条件来一次修改多个entity

## Connection

connection现在最容易理解的例子可能就是weibo了, 一个用户A关注(following)了用户B, 那么A就是B的关注者(follower)了.

在API Service中, 我们可以很容易的做到这一点

    POST /:org/:app/users/:username_a/following/users/:username_b
    
注: :username也可以使用user的uuid代替

通过这个请求, 我们就建立了user A和user B的关系了, 并且API Service会自动创建反向的follower关系

建立了这个关系之后, 我们就可以通过下面的请求来获取到user A都关注了哪些用户

    GET /:org/:app/users/:username_a/following        
    
并且, 通过下面的请求, 我们可以知道user B都有那些关注者

    GET /:org/:app/users/:username_b/followers   

API Service中提供的connection功能还远远不止与此, 实际上, 除了系统中提供的following/followers的关系外, 你可以自己创建任意的关系

一个entity可以和另外一个entity关联起来, 不管这两个entity是否在同一个collection当中(即是否是同一种类型), 从而建立其两个entity之间的关系

例如

    POST http://api.easemob.com/my-org/my-app/users/john.doe/likes/food/pizza
    
这个request就在user john.doe和pizza之间建立了"like"的关系
    
---
title: 环信
---

* TOP
{:toc}

## 环信服务器端API介绍

环信的服务器端接口都是通过[REST服务](http://zh.wikipedia.org/zh-cn/REST)方式提供的, 主要是因为REST接口基于最简单的HTTP协议, 在各个编程语言中, 都对HTTP提供了良好的支持, 从而也能够很方便的调用REST服务.

### REST基本介绍

首先需要明确的是, 调用REST服务, 就是发送HTTP请求, 只不过大家常用的可能是 _HTTP GET_ 和 _HTTP POST_请求, 但是在REST 里面还经常用到 _HTTP PUT_ 和 _HTTP DELETE_, 在REST中, 把这四种操作称之为*动词*, 可以(不是特别准确)的想象成增删改查.

而动词所操作的对象, 在REST中, 被称之为_资源_, 也就是_URL_, 而这些也都是标准的HTTP协议的内容.

实际上, 当我们在浏览器中打开一个网站的时候, 例如, 打开`www.easemob.com`, 浏览器实际上发送给网站服务器的, 就是一个HTTP GET的请求.

需要注意的是, 环信的REST API都是基于json的, 所以在构造HTTP 请求的时候, 需要在HTTP HEADER中指明:

* Accept: application/json
* Content-Type: application/json

第一个header属性_Accept_ 表明了, 这个client需要环信的REST服务返回json格式的数据,

第二个header属性_Content-Type_表明了, 这个client发送(POST / PUT的时候)的数据, 是json格式的, 很多时候, 遇到的调用环信REST服务报错, 是因为客户端发送了http form格式而不是json格式的数据导致的.


###  HTTP 1.0 / HTTP 1.1

当前, 主要使用的是两个HTTP协议的版本, 主流的浏览器都已经默认使用HTTP 1.1协议了, 很多HTTP client的类库也都已经使用了HTTP 1.1的版本, 我们建议您在开发程序的时候, 检查一下所使用的类库, 如果支持HTTP 1.1的话, 尽量设置成使用HTTP 1.1的.

HTTP 1.0 和 1.1 这两个版本的协议, 有一个最主要的不同是在1.1版本中, HTTP请求的Keepalive属性默认是True的.

当一个HTTP 请求的keepalive属性为True的时候, 服务器接到这个请求, 在处理完成返回的时候, 会保留TCP连接, 所以, 当同一个client在短时间内发出下一个请求的时候, 就可以重用这个TCP链接, 从而节约调用时间, 提高客户端的性能.

当keepalive属性为False(这是HTTP 1.0的默认行为)的时候, 服务器在处理完一个请求之后, 就会直接关闭TCP 连接, 所以, 如果这个client再次发送请求, 就得重新创建一个新的TCP连接了, 而创建TCP连接需要经历三次握手, 从而导致客户端的代码效率降低.



### REST client

因为HTTP已经是一个最被广泛使用的协议了, 所以各个编程语言都对起有良好的支持, 无论是使用哪种语言, 都可以很方便的找到相应的方法来调用环信的REST服务.

#### JAVA

在java中, 我们推荐使用[Jersey](https://jersey.java.net)来调用环信的REST服务, Jersy 2.x的版本实现了JAX-RS 2.0 的规范, 使用起来很方便, 并且提供了异步的支持, 但是Jersey 2.x需要JDK 1.7的支持, 所以如果你的服务器端程序还没有办法使用JDK 1.7, 那么需要使用 Jersey 1.x的版本.

也有很多人直接使用[Apache Http Client](http://hc.apache.org), 我们并不推荐直接使用这个库, 主要是因为这个库相对比较底层, 需要自己处理的东西很多, API也相对繁琐.

#### Python

对于python程序, 我们推荐使用[request](http://docs.python-requests.org/en/latest/)这个类库来调用环信的REST服务.


## 环信服务器基本架构

当用户在[环信管理后台](https://console.easemob.com)中注册的时候, 需要填写一个"企业ID", 这是因为环信是一个支持多租户的云服务平台, 并且环信是支持"企业"(或者理解成团队)-"App"两级结构的.

即, 在环信平台中, 每个企业ID之间的数据都是严格相互隔离的, 而每个企业ID内部的每个App之间的数据, 也都是严格相互隔离的.

可以想象这样的模型, 就是一个公司中有三个部门, 每个部门负责一个App, 那么这个公司可以注册一个环信的账号, 然后在这个账号的名下创建三个(环信中的)App, 每个环信中的app对应一个部门的app.

这样, 最开始注册的时候的账号, 是这个企业在环信中的企业管理员账号, 企业管理员可以创建新的app, 并且指派app管理员账号, 在访问权限上, 企业管理员拥有最高的权限, 可以看到自己的企业ID下所有app中的所有的数据, 而app管理员则只能看到自己app中的数据, 看不到其它app中的数据.

同时, 上面也说过了, 同一个企业id下面的app之间, 数据也都是相互隔离的, 所以完全可以在两个app中创建相同用户名的用户.


另, 如果只是个人开发者开发一个app的话, 那么企业id可以随便填写, 只需要不和环信现有的企业id重复即可.

最后, 因为环信提供的是REST服务, 并且上面说过, REST本质就是通过 GET / POST / PUT /DELETE 来操作资源(URL), 所以, 实际上可以看到在环信的REST API中, 也体现了这种思想.

假设一个企业id为 _easemob-demo_, 然后这个企业下面有个app名字叫做 _chatdemoui_, 那么环信的REST API就是下面的样子

* 获取这个app下的所有用户
    
        GET https://a1.easemob.com/easemob-demo/chatdemoui/users
    
* 获取这个app下的用户stliu的详情

        GET https://a1.easemob.com/easemob-demo/chatdemoui/users/stliu
    
* 在这个app下创建一个新的用户

        POST https://a1.easemob.com/easemob-demo/chatdemoui/users {"username":"stliu1", "password":"123456"}
    
    需要注意的是这里post的数据需要是json格式的, 并且设置Content-Type 为 application/json, 不能使用HTTP Form的形式来提交的.
    
* 删除一个用户    

        DELETE https://a1.easemob.com/easemob-demo/chatdemoui/users/stliu
        
从上面的URL的规则中, 也能够看出"企业"--"app"--"用户"的层层递进的关系.        
    

### 名词解释

* org_name
    
    是开发者在环信开发者后台注册账号时填写的公司Id。
    
* app_name

    是开发者在环信开发者后台创建app时填写的app名。

* org_admin
    是开发者在环信开发者后台注册的账号。企业管理员账号可以在自己账号下创建多个app。org管理员拥有对该公司账号下所有app的操作权限。
    
* app_admin
    
    每个app都可以创建app管理员（可选操作）。app管理员拥有app级别的操作权限。
    
* appkey

    在环信的管理后台可以看到自己app的appkey, 这个app可以的规则实际上是 {org_name}#{app_name}
    
### 安全

环信的REST服务完全是基于HTTPS的安全协议, 从协议层面保证了在调用的时候, 不会被别人窃取到.

环信的后台服务API在安全上, 是基于 *OAuth 2.0*标准的和RBAC(基于角色的访问控制)的权限模型的

在调用环信的后台服务之前, 需要先登陆获取token, 而根据请求发起人的角色不同, 获取token的方式也不同

### 示例代码

环信提供了如何调用REST服务的示例代码, 可以在[这里](https://github.com/easemob/emchat-server-examples)找到.

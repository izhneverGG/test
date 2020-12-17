RESTful（英文：**Representational State Transfer，表述性状态转移**）

##### 1. 什么是REST

​	REST是基于HTTP实现的一种API接口**设计风格**。

​	它首次出现在2000年Roy Fielding的博士论文中，Roy Fielding是HTTP规范的主要编写者之一。 他在论文中提到："我这篇文章的写作目的，就是想在符合架构原理的前提下，理解和评估以网络为基础的应用软件的架构设计，得到一个功能强、性能好、适宜通信的架构。REST指的是一组架构约束条件和原则。" 如果一个架构符合REST的约束条件和原则，我们就称它为RESTful架构。

​	REST本身没有创造新的技术或其他东西，而是用**现在web中所有的特征，去更好地使用现在web标准中一些准则和约束**，所以现在所描述的REST是基于HTTP的，但是不意味着和HTTP绑定。

##### 2. 理解RESTful

###### 2.1 资源

​	任何事物，只要有被引用到的必要，它就是一个资源。资源可以是实体(例如手机号码)，也可以只是一个抽象概念(例如价值) 。下面是一些资源的例子：

- 某用户的手机号码

- 某用户的个人信息

- 最多用户订购的GPRS套餐

- 两个产品之间的依赖关系

- 某用户可以办理的优惠套餐

- 某手机号码的潜在价值

  要让一个资源可以被识别，需要有个唯一标识，在Web中这个唯一标识就是URI(Uniform Resource Identifier)。而表述性状态转移，就是对此资源的的表述。

###### 2.2 统一资源接口

​	RESTful遵循统一接口原则，统一接口包含了一组**受限的预定义操作，不论什么样的资源，都是通过使用相同的接口方式进行资源的访问**。接口应该使用标准的HTTP方法如GET，PUT和POST，并遵循这些方法的语义。

​	如果按照HTTP方法的语义来暴露资源，那么接口将会拥有安全性和幂等性的特性，例如GET是安全的， 无论请求多少次，都不会改变服务器状态（安全性）。而GET、PUT和DELETE请求都是幂等的，无论对资源操作多少次， 结果总是一样的，后面的请求并不会产生比第一次更多的影响（幂等性）。

下面列出了GET，DELETE，PUT和POST的安全性与幂等性:

###### GET

- 安全且幂等
- 获取表示


###### POST

- 重复会创建多个资源，不安全且不幂等
- 创建子资源


###### PUT

- 改变状态，重复无影响，不安全但幂等
- 通过替换的方式更新资源

###### DELETE

- 删除资源，重复无影响，不安全但幂等
- 删除资源

###### 状态码

- 200 （OK）- [GET] 已在响应中发出, [POST]如果现有资源已被更改, [PUT]通过替换的方式更新资源, [DELETE]资源已被删除
- 301 （Moved Permanently）- 资源的URI已更改
- 303 （See Other）- 其他，如负载均衡
- 400 （bad request）- 指代坏请求
- 404 （not found）- 资源不存在
- 500 （internal server error）- 通用错误响应

- 503 （Service Unavailable）- 服务端当前无法处理请求

​	还有其他拓展方法，比如RFC的PATCH，WebDAV的LOCK，UPLOCK，使用时需考虑接口调用方是否支持。

3. ###### RESTful接口

   RESTful接口可以简单理解为**面向资源设计API**，设计一个Restful接口可以参考以下步骤：

- API的专用域名

  ```java
  api.example.com
  ```

  或者放在主域名后面。

  ```java
  example.com/api
  ```

- 加入版本控制

  ```java
  example.com/api/v1
  ```

- 在RESTful中，每一个API都代表着一种资源，所以API中只有名词，没有动词。

  ```java
  example.com/api/v1/user
  ```

  比如下面常见的接口是错误的。

  ```java
  example.com/api/v1/addUser
  ```

- 使用/表示资源的层级

  ```java
  example.com/api/v1/user/book
  ```

- 使用?过滤资源

  ```java
  example.com/api/v1/user/book?uid=1&bid=1
  ```

- 使用,或;表示同级资源

  ```java
  example.com/api/v1/user/book?bid=1,2
  ```

- 使用HTTP动词表示资源操作类型

  ```java
  GET		example.com/api/v1/users					  //获取所有用户
  GET		example.com/api/v1/user/book?uid=1&bid=1	  //获取id=1，bid=1的book信息
  POST 	example.com/api/v1/user						  // 新建一个用户
  PUT    	example.com/api/v1/user						  // 更新一个用户（提供全部信息）
  PATCH	example.com/api/v1/user						  //更新一个用户部分信息（提供部分信息）
  DELETE  example.com/api/v1/user/1				  //删除uid=1的用户
  ```

- 返回状态码符合HTTP规范

- 服务器返回数据格式尽量使用JSON（Accept、Content-Type=application/json）

以上例子是比较简单的，资源都可以看做是实体对象，不妨把资源再抽象一点。

比如，有个接口是按年度时间段，查询首页柱状图统计数据，可以这样设计接口。

```java
GET api/v1/index/bar-graph?ndKs=2017&ndJs=2021
```

有个案件审批的接口呢？如果只是修改单条数据状态，可以是PATCH，如果不仅修改状态，还有新建数据，可以用PUT。

```java
PATCH api/v1/aj/action/shen-pi?ajbh=xxx
```

获取组织机构树的接口。

```java
GET artery/organ/children?selectType=corp&selectScope=csCorp&multiple=false&id=
```

知乎的一些接口：

```java
GET www.zhihu.com/api/v4/answer_later/count	    //新回答统计

GET www.zhihu.com/api/v4/home/sidebar			//获取首页侧边栏

GET www.zhihu.com/api/v4/questions/24952874/concerned_followers?limit=7&offset=0	//文章编号24952874的回答

POST www.zhihu.com/api/v4/questions/433380294/answers?include=visible_only_to_author.........  //回答问题
   
PUT	www.zhihu.com/api/v4/questions/433380294/answers?include=visible_only_to_author.........  //修改回答
    
DELETE www.zhihu.com/api/v4/answers/440113990	//删除回答
```

以上是狭义的理解——“资源的CRUD”。
REST有个核心概念，"超媒体即应用状态引擎（hypermedia as the engine of application state）"，要求在表述格式里边加入链接来引导客户端，但是一般用不到，有兴趣可以看一看。


参考：

[RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)

[Hypertext Transfer Protocol -- HTTP/1.1](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

[PATCH Method for HTTP](https://tools.ietf.org/html/rfc5789)

[RESTful 架构详解](https://www.runoob.com/w3cnote/restful-architecture.html)


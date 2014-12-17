
空搜索
=============

* [空搜索](empty-search.md)
* [查询DSL](query-dsl.md)
* [查询和过滤器](queries-and-filters.md)
* [更重要的查询和过滤器](most-important-queries-and-filters.md)
* [带过滤器的查询](combining-queries-with-filters.md)
* [验证查询](validating-queries.md)

让我们开始使用最简单的搜索API——空搜索，这将返回索引中的所有文档。

```
GET /_search
{}  <1>
```

> 1. 这个一个空请求体。


和字符串搜索，你也可以搜索一个、多个或者`_all`索引，以及一个、多个或者所有的类型：

```
GET /index_2014*/type1,type2/_search
{}
```
你可以使用`from`和`size`参数进行分页：
```
GET /_search
{
  "from": 30,
  "size": 10
}
```

带请求体的GET请求？
---------------

一些特定的HTTP库（比如JavaScript）并不允许GET请求带请求体。实际上，很多用户对GET请求允许带一个请求体感到非常惊讶。


真相是，在[RFC 7231](http://tools.ietf.org/html/rfc7231#page-24)（RFC处理HTTP语义和内容）并不确定当GET请求带了一个请求体时将会发生什么！结果是，一些HTTP服务器允许这样，而另外一些（尤其是缓存代理）不允许。

比起POST方式，Elasticsearch的作者更钟爱使用GET方法进行搜索，因为这样更好地描述了获取信息这个动作。然而，带请求体的GET请求并不被普遍支持，因而搜索API也允许使用POST请求：

```shell
POST /_search
{
  "from": 30,
  "size": 10
}
```
同样的规则适合其他需要请求体的GET API。

我们将在《[深度搜索](../depth-in-aggregations.md)》中介绍聚合，现在我们把重点放在查询上。

不像神秘的查询字符串的方法，请求体搜索允许我们通过查询特定领域语言，或者查询DSL编写查询。

--------------
[« 请求体查询](full-body-search.md)     [查询DSL »](query-dsl.md)  

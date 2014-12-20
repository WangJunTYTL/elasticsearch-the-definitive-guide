验证查询
=============

* [空搜索](empty-search.md)
* [查询DSL](query-dsl.md)
* [查询和过滤器](queries-and-filters.md)
* [更重要的查询和过滤器](most-important-queries-and-filters.md)
* [带过滤器的查询](combining-queries-with-filters.md)
* [验证查询](validating-queries.md)

查询可以变得相当复杂，特别是当结合不同的分析器和字段映射，会变得难以理解。查询验证API可以用来校验查询的有效性。


```shell
GET /gb/tweet/_validate/query
{
   "query": {
      "tweet" : {
         "match" : "really powerful"
      }
   }
}
```

上面的验证请求的返回内容告诉我们查询是无效的：

```json
{
  "valid" :         false,
  "_shards" : {
    "total" :       1,
    "successful" :  1,
    "failed" :      0
  }
}
```

理解错误
-------------

要找出查询无效的原因，可以在查询字符串后面加上`explain`参数：

```shell
GET /gb/tweet/_validate/query?explain <1>
{
   "query": {
      "tweet" : {
         "match" : "really powerful"
      }
   }
}
```

> 1. `explain`标识会提供更多关于查询无效的原因。

--------------

显然，我们混淆了查询的类型（`query`)和字段的名称(`tweet`)：

```json
{
  "valid" :     false,
  "_shards" :   { ... },
  "explanations" : [ {
    "index" :   "gb",
    "valid" :   false,
    "error" :   "org.elasticsearch.index.query.QueryParsingException:
                 [gb] No query registered for [tweet]"
  } ]
}
```

理解查询
------------

使用`explain`参数还有另外的优点，会返回便于用户理解的查询失败的原因，对我们准确理解Elasticsearch中断查询的原因有很大的帮助：

```shell
GET /_validate/query?explain
{
   "query": {
      "match" : {
         "tweet" : "really powerful"
      }
   }
}
```

我们查询的每一个索引都会返回一个`explanation`字段，因为每一个索引都有不同的映射和分析器：

```json
{
  "valid" :         true,
  "_shards" :       { ... },
  "explanations" : [ {
    "index" :       "us",
    "valid" :       true,
    "explanation" : "tweet:really tweet:powerful"
  }, {
    "index" :       "gb",
    "valid" :       true,
    "explanation" : "tweet:realli tweet:power"
  } ]
}
```
通过`explanation`字段，你可以看到`match`查询是如何把`really powerful`针对`tweet`字段分析成两个单独的词。


与此同时，你可以看到对于`us`索引，两个术语是`really`和`powerful`；而对于`gb`索引，则是`realli`和`power`。这是因为我们对`gb`索引的`tweet`字段使用了`english`分析器，另外一个则是默认的分析器。

--------------------

[«  带过滤器的查询](combining-queries-with-filters.md)     [排序和相关性 »](../sorting-and-relevance/README.MD)

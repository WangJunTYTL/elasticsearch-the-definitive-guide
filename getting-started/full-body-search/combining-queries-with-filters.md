带过滤器的查询
=============

* [空搜索](empty-search.md)
* [查询DSL](query-dsl.md)
* [查询和过滤器](queries-and-filters.md)
* [更重要的查询和过滤器](most-important-queries-and-filters.md)
* [带过滤器的查询](combining-queries-with-filters.md)
* [验证查询](validating-queries.md)

查询可以用于查询上下文，过滤器能被用于过滤上下文。通过Elasticsearch API，你可以看到参数中既包含`query`又包含`filter` 。这意味着会出现一个包含查询或过滤器条件的单一参数。也就是说，他们建立一个外部的上下文作为查询或者过滤器的上下文。

复合查询条件可以封装其他查询条件，符合过滤器条件可以封装其他过滤器条件。然而，在查询里边封装一个过滤器也是非常有用的，较少的情况，还会使用全文查询作为过滤器。

为了达到这个目的，有专门封装过滤器的查询条件，反之亦然。允许我们从一个上下文切换到另外的上下文。如何通过高效的方式组合查询和过滤器条件达到目的，非常重要。

过滤查询
----------

比方说我们有这样一个查询：

```json
{ "match": { "email": "business opportunity" }}
```

我们想把它结合下面的`term`过滤器, 匹配在我们的`inbox`的文档：

```json
{ "term": { "folder": "inbox" }}
```

`search`API只能接受一个`query`参数，所以我们必须将查询和过滤器封装成另外一个查询，称之为`filtered`查询：

```json
{
    "filtered": {
        "query":  { "match": { "email": "business opportunity" }},
        "filter": { "term":  { "folder": "inbox" }}
    }
}
```

现在我们能把这个查询传入`search`API的`query`参数：

```shell
GET /_search
{
    "query": {
        "filtered": {
            "query":  { "match": { "email": "business opportunity" }},
            "filter": { "term": { "folder": "inbox" }}
        }
    }
}
```

仅仅是过滤器
----------


如果你需要在查询上下文使用没有查询的过滤器（比如，匹配所有在`inbox`的邮件），你可以提交下面的查询：

```shell
GET /_search
{
    "query": {
        "filtered": {
            "filter":   { "term": { "folder": "inbox" }}
        }
    }
}
```

如果没有指定查询，默认使用`match_all`查询，所以上面的查询相当于下面的查询：

```shell
GET /_search
{
    "query": {
        "filtered": {
            "query":    { "match_all": {}},
            "filter":   { "term": { "folder": "inbox" }}
        }
    }
}
```

将查询作为过滤器
-------------

偶尔你可以能希望在过滤器上下文使用一个查询。这可以通过`query`过滤器实现，仅仅是封装了一个查询。下面的例子展示了一个排除垃圾邮件的方法：

```shell
GET /_search
{
    "query": {
        "filtered": {
            "filter":   {
                "bool": {
                    "must":     { "term":  { "folder": "inbox" }},
                    "must_not": {
                        "query": {  <1>
                            "match": { "email": "urgent business proposal" }
                        }
                    }
                }
            }
        }
    }
}
```

> 1. 注意，`query`过滤器允许我们在`bool`过滤器里边使用`match`查询。

------

> 提示
-----
你可能需要把查询作为一个过滤器使用，但是因为完整性的缘故，我们提供了这个功能。你唯一需要用到它是在你需要在过滤器上下文做全文匹配的时候。

-----------------------
[« 更重要得查询和过滤器](most-important-queries-and-filters.md)     [验证查询 »](validating-queries.md)

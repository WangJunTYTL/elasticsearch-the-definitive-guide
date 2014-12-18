更重要的查询和过滤器
=================

* [空搜索](empty-search.md)
* [查询DSL](query-dsl.md)
* [查询和过滤器](queries-and-filters.md)
* [更重要的查询和过滤器](most-important-queries-and-filters.md)
* [带过滤器的查询](combining-queries-with-filters.md)
* [验证查询](validating-queries.md)

而elasticsearch带来许多的查询和过滤器，你将只使用一些经常。我们讨论的更为详细的搜索深度但接下来我们给你一个快速的介绍最重要的查询和过滤器。

Elasticsearch有很多查询和过滤器，你经常使用的只是一小部分。我们将在《[深度搜索](../../search-in-depth/README.MD)》中详细讨论更多的细节，现在我们来快速了解一些更加重要的查询和过滤器。

`term`过滤器
---------

`term`过滤器被用来对精确值进行过滤，包括数值、日期、布尔值、或者`not_analyzed`的字符串：


```json
{ "term": { "age":    26           }}
{ "term": { "date":   "2014-09-01" }}
{ "term": { "public": true         }}
{ "term": { "tag":    "full_text"  }}
```

`terms`过滤器
--------------

`terms`过滤器和`term`过滤器相同，但是允许指定更多的值进行匹配。只要字段包含任何指定的值，都会被匹配上：

```json
{ "terms": { "tag": [ "search", "full_text", "nosql" ] }}
```

`range`过滤器
-------

`range`过滤器允许你找到在指定范围内的数字或日期：


```json
{
    "range": {
        "age": {
            "gte":  20,
            "lt":   30
        }
    }
}
```

操作符接受如下几种：

`gt`
> 大于

`gte`
> 大于等于

`lt`
> 小于

`lte`
> 小于等于


`exists`和`missing`过滤器
-------------

`exists`和`missing`过滤器被用来查找指定字段有一个或多个值（存在）或没有任何值（丢失）。这和SQL语句的`IS_NULL`(丢失）和`NOT IS_NULL`(存在）相同：

```json
{
    "exists":   {
        "field":    "title"
    }
}
```

这些过滤器经常被用来在一个字段存在时应用一个条件，不存在时应用另外的条件。

`bool`过滤器
------------

`bool`过滤器用来使用布尔逻辑综合多个过滤器条件。它接收下面三个参数：

* `must`

> 这些条件必须都满足，和`and`类似。

* must_not

> 这些条件必须不满足，和`not`类似。

* should

> 至少有一个条件要满足，类似于`or`。

每一个参数都能接受一个单一的过滤器或者过滤器数组：

```json
{
    "bool": {
        "must":     { "term": { "folder": "inbox" }},
        "must_not": { "term": { "tag":    "spam"  }},
        "should": [
                    { "term": { "starred": true   }},
                    { "term": { "unread":  true   }}
        ]
    }
}
```

`match_all`查询
---------------

`match_all`查询所有的文档。它是默认的查询，没有指定任何的查询：

```json
{ "match_all": {}}
```

这个查询经常用来和一个过滤器想结合。例如，接收所有收件箱里边的邮件。所有的文档被认为是同等重要的，接收到的`_score`值都为1。

`match`查询
--------

`match`查询应该被认为是一种标准查询，你可以在任何时候对全文字段或者对几乎所有的精准值字段进行查询。

如果你对全文字段进行`match`查询，在执行查询之前会使用正确的分析器对查询字符串进行分析。

```json
{ "match": { "tweet": "About Search" }}
```

如果你对一个精确值字段（比如一个日期、一个布尔值、或者一个`not_analyzed`的字符串字段）进行`match`查询，它就会完全按这个确切的值进行查询：

```json
{ "match": { "age":    26           }}
{ "match": { "date":   "2014-09-01" }}
{ "match": { "public": true         }}
{ "match": { "tag":    "full_text"  }}
```

> 提示
-----
对于精确值查询，你可能想使用一个过滤器而不是查询，因为过滤器将会被缓存。
语法。
不像我们在《[搜索精简版](../search-lite.md)》查询字符串搜索，`match`查询并不使用类似于+user_id:2 +tweet:search这样的查询语法。它仅仅是查找指定的这些词。这意味着把搜索字段暴露给用户是安全的；你可以控制哪些字段能被查询，也不容易带来语法错误。


`multi_match`查询
----------------

`multi_match`查询允许对多个字段进行同样的匹配查询：

```
{
    "multi_match": {
        "query":    "full text search",
        "fields":   [ "title", "body" ]
    }
}
```

`bool`查询
----------

`bool`查询和`bool`过滤器类似，被用来综合多个查询条件。然而，也还是一些区别。过滤器给出yes/no的回答，而查询则会计算出相关度。`bool`查询将从每一个匹配的`must`或者`should`条件匹配综合得出出`_score`。这个查询接受如下的参数：

* `must`

> 条件必须和包含的文档相匹配。

`must_not`

* 条件必须和包含的文档不匹配。

`should`
> 如果条件匹配，它们会增加`_score`的分值；否则，它们没有任何影响。它们被用来对每个文档提炼相关性得分。


下面的查询用来查找`title`字段匹配查询字符串`how to make millions`且没有被标记为垃圾的文档。任何被标星或者创建于2014年以后的文档，得分高于本来应得的。满足两个条件的得分更高。

```json
{
    "bool": {
        "must":     { "match": { "title": "how to make millions" }},
        "must_not": { "match": { "tag":   "spam" }},
        "should": [
            { "match": { "tag": "starred" }},
            { "range": { "date": { "gte": "2014-01-01" }}}
        ]
    }
}
```

> 提示
----
如果没有`must`条件，至少需要有一个`should`条件进行匹配。而如果有至少一个`must`条件，则不需要有`should`条件。

[« 查询和过滤器](queries-and-filters.md)     [带过滤器的查询 »](combining-queries-with-filters.md)


DSL
==============

* [空搜索](empty-search.md)
* [查询DSL](query-dsl.md)
* [查询和过滤器](queries-and-filters.md)
* [更重要的查询和过滤器](most-important-queries-and-filters.md)
* [带过滤器的查询](combining-queries-with-filters.md)
* [验证查询](validating-queries.md)

DSL是一种灵活，富有表现力的搜索语言。Elasticsearch使用通过简单的JSON接口展现了Lucene的绝大部分威力。这是你在生产环境下应该使用的查询方式，它使得查询更加灵活、精确、便于阅读、易于调试。

要使用查询DSL，需要给查询参数加入一个`query`关键字：

```
GET /_search
{
    "query": YOUR_QUERY_HERE
}
```

空查询`{}`等同于使用`match_all`这个查询条件，顾名思义，表示匹配所有文档：

```shell
GET /_search
{
    "query": {
        "match_all": {}
    }
}
```


查询语句的结构
-------------

一个经典的查询语句结构如下：

```json
{
    QUERY_NAME: {
        ARGUMENT: VALUE,
        ARGUMENT: VALUE,...
    }
}
```

如果引用某个特定的字段，则使用下面的结构：

```json
{
    QUERY_NAME: {
        FIELD_NAME: {
            ARGUMENT: VALUE,
            ARGUMENT: VALUE,...
        }
    }
}
```

例如，你可以通过下面的查询语句找到在`tweet`字段提到`elasticsearch`的推文：

```json
{
    "match": {
        "tweet": "elasticsearch"
    }
}
```

完整的搜索请求如下：

```shell
GET /_search
{
    "query": {
        "match": {
            "tweet": "elasticsearch"
        }
    }
}
```

多条件组合
----------

查询条件可以像堆积木一样组合起来，形成复杂的查询。条件如下：

* `叶子条件`（如`match`语句）被用来比较一个字段（或几个字段）和查询字符串。

* `复合条件`是用来组合其他查询条件的。比如，一个`bool`条件允许结合`must`、`must_not`、或者`should`使用。

```
{
    "bool": {
        "must":     { "match": { "tweet": "elasticsearch" }},
        "must_not": { "match": { "name":  "mary" }},
        "should":   { "match": { "tweet": "full text" }}
    }
}
```

需要注意的是，一个复合条件可以结合任何其他查询条件，包括其他符合条件。这意味着，复句可以互相嵌套，允许非常复杂的逻辑表达式。这意味着复合条件可以相互嵌套，允许非常复杂的逻辑表达式。


作为一个例子，下面的查询查找包含`business opportunity`的电子邮件，要么被标星，要么在“inbox”目录且未标记为垃圾邮件：

```json
{
    "bool": {
        "must": { "match":      { "email": "business opportunity" }},
        "should": [
             { "match":         { "starred": true }},
             { "bool": {
                   "must":      { "folder": "inbox" }},
                   "must_not":  { "spam": true }}
             }}
        ],
        "minimum_should_match": 1
    }
}
```

别担心这个例子的细节，我们将在后面做详细分析。最重要的事情是一个复合查询子句可以将多个条件（无论是叶子条件还是符合条件）组成成一个单一查询。

----------------

[« 空搜索](empty-search.md)     [查询和过滤 »](queries-and-filters.md)

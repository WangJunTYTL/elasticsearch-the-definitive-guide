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

术语过滤
---------

术语过滤器被用来对精确值进行过滤，包括数值、日期、布尔值、或者`not_analyzed`的字符串：


```json
{ "term": { "age":    26           }}
{ "term": { "date":   "2014-09-01" }}
{ "term": { "public": true         }}
{ "term": { "tag":    "full_text"  }}
```

多术语过滤器
--------------

多术语过滤器和术语过滤器相同，但是允许指定更多的值进行匹配。只要字段包含任何指定的值，都会被匹配上：

```json
{ "terms": { "tag": [ "search", "full_text", "nosql" ] }}
```

范围过滤
-------

范围过滤器允许你找到在指定范围内的数字或日期：


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


存在和丢失过滤器
-------------

存在和丢失过滤器被用来查找指定字段有一个或多个值（存在）或没有任何值（丢失）。这和SQL语句的`IS_NULL`(丢失）和`NOT IS_NULL`(存在）相同：

```json
{
    "exists":   {
        "field":    "title"
    }
}
```


These filters are frequently used to apply a condition only if a field is present, and to apply a different condition if it is missing.

bool filteredit

The bool filter is used to combine multiple filter clauses using Boolean logic. It accepts three parameters:

must
These clauses must match, like and.
must_not
These clauses must not match, like not.
should
At least one of these clauses must match, like or.
Each of these parameters can accept a single filter clause or an array of filter clauses:

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
VIEW IN SENSE
match_all queryedit

The match_all query simply matches all documents. It is the default query that is used if no query has been specified:

{ "match_all": {}}
VIEW IN SENSE
This query is frequently used in combination with a filter—for instance, to retrieve all emails in the inbox folder. All documents are considered to be equally relevant, so they all receive a neutral _score of 1.

match queryedit

The match query should be the standard query that you reach for whenever you want to query for a full-text or exact value in almost any field.

If you run a match query against a full-text field, it will analyze the query string by using the correct analyzer for that field before executing the search:

{ "match": { "tweet": "About Search" }}
VIEW IN SENSE
If you use it on a field containing an exact value, such as a number, a date, a Boolean, or a not_analyzed string field, then it will search for that exact value:

{ "match": { "age":    26           }}
{ "match": { "date":   "2014-09-01" }}
{ "match": { "public": true         }}
{ "match": { "tag":    "full_text"  }}
VIEW IN SENSE
Tip
For exact-value searches, you probably want to use a filter instead of a query, as a filter will be cached.

Unlike the query-string search that we showed in Search Lite, the match query does not use a query syntax like +user_id:2 +tweet:search. It just looks for the words that are specified. This means that it is safe to expose to your users via a search field; you control what fields they can query, and it is not prone to throwing syntax errors.

multi_match queryedit

The multi_match query allows to run the same match query on multiple fields:

{
    "multi_match": {
        "query":    "full text search",
        "fields":   [ "title", "body" ]
    }
}
VIEW IN SENSE
bool queryedit

The bool query, like the bool filter, is used to combine multiple query clauses. However, there are some differences. Remember that while filters give binary yes/no answers, queries calculate a relevance score instead. The bool query combines the _score from each must or should clause that matches. This query accepts the following parameters:

must
Clauses that must match for the document to be included.
must_not
Clauses that must not match for the document to be included.
should
If these clauses match, they increase the _score; otherwise, they have no effect. They are simply used to refine the relevance score for each document.
The following query finds documents whose title field matches the query string how to make millions and that are not marked as spam. If any documents are starred or are from 2014 onward, they will rank higher than they would have otherwise. Documents that match both conditions will rank even higher:

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
VIEW IN SENSE
Tip
If there are no must clauses, at least one should clause has to match. However, if there is at least one must clause, no should clauses are required to match.

«  queries and filters     combining queries with filters  »

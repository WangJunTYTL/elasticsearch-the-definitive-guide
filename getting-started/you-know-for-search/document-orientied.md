
面向文档
===============

* [安装Elasticsearch](installing-elasticsearch.md)
* [运行Elasticsearch](running-elasticsearch.md)
* [和Elasticsearch交流](talking-to-elasticsearch.md)
* [面向文档](document-oriented.md)
* [发现你的足迹](finding-your-feet.md)
* [索引员工文档](indexing-employee-documents.md)
* [检索一个文档](retrieving-a-document.md)
* [搜索精简版](search-lite.md)
* [使用查询DSL搜索](search-with-query-dsl.md)
* [复杂搜索](more-complicated-searches.md)
* [全文搜索](full-text-search.md)
* [短语搜索](phrase-search.md)
* [高亮搜索](highlighting-our-searches.md)
* [分析](analytics.md)
* [教程结论](tutorial-conclusion.md)
* [分布式特性](distributed-nature.md)
* [接下来](next-steps.md)

应用程序中的对象很少只是简单的键值对列表。更多的情况下，它们有复杂的数据结构，包含日期、geo位置、其他对象，或其数组的值。

迟早你要将这些对象存储到数据库。尝试使用关系型数据库的行和列来做这件事，需要把丰富的对象要转化成一个大的电子表格，这是在压寨你的富有：你必须把对象扁平化，以对应上数据表的架构（通常每列一个字段），而你每次获取它们的时候还需要重新组织成对象。

Elasticsearch是面向文档的，意味着它存储整个对象或者文档。它不仅存储它们，还会对每个文档简历索引，供搜索使用。在Elasticsearch，你的文档可以被索引、搜索、排序、以及过滤，而不是几行柱状数据。这是完全不同的一种对待数据的方式，也是Elasticsearch能够进行复杂全文搜索的原因。

JSON
--------

Elasticsearch使用[JSON](http://en.wikipedia.org/wiki/Json)（全称Javascript Object Notation）作为文档的序列化格式。大部分的编程语言都支持JSON序列化，已经变成NoSQL运动的标准格式。它简单、简洁、易于阅读。

来看一下这个表示一个`user`对象的JSON文档：

```json
{
    "email":      "john@smith.com",
    "first_name": "John",
    "last_name":  "Smith",
    "info": {
        "bio":         "Eco-warrior and defender of the weak",
        "age":         25,
        "interests": [ "dolphins", "whales" ]
    },
    "join_date": "2014/05/01"
}
```

虽然原始的`user`对象很复杂，但是这个对象的结构和意义被JSON版本保留了。在Elasticsearch中，把对象转化成JSON进行索引，比起处理成扁平数据表结构要更简单。


> 备注
-------
虽然所有的语言都有将任何数据或者对象转化为JSON的模块，但它们的细节各不相同，因而你需要找出JSON序列化或还原（marshlling）的模块。[Elasticsearch官方客户端API](http://www.elasticsearch.org/guide/)已经把为你把JSON序列化的工作自动处理好了。

---------------------

[« 和Elasticsearch交流](talking-to-elasticsearch.md)      [发现你的足迹 »](finding-your-feet.md)


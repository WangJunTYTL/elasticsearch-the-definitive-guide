
使用DSL搜索
================

* [安装Elasticsearch](installing-elasticsearch.md)
* [运行Elasticsearch](running-elasticsearch.md)
* [和Elasticsearch交流](talking-to-elasticsearch.md)
* [面向文档](document-oriented.md)
* [找找感觉](finding-your-feet.md)
* [索引文档](indexing-employee-documents.md)
* [检索文档](retrieving-a-document.md)
* [简单搜索](search-lite.md)
* [使用DSL搜索](search-with-query-dsl.md)
* [复杂点的搜索](more-complicated-searches.md)
* [全文搜索](full-text-search.md)
* [短语搜索](phrase-search.md)
* [高亮搜索结果](highlighting-our-searches.md)
* [分析](analytics.md)
* [教程总结](tutorial-conclusion.md)
* [分布式特性](distributed-nature.md)
* [接下来](next-steps.md)

查询字符串供命令行临时使用，但是有它本身的一些限制（参见《[搜索精简版](../search/search-lite.md)》一节）。Elasticsearch提供一个丰富、灵活的查询语言，叫做查询DSL，允许我们构建更加复杂、健壮的查询。

DSL(domain-specific language)规定使用JSON请求体。我们重新将前面查找所有`Smith`的查询表述成下面的格式：

```
GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "last_name" : "Smith"
        }
    }
}
```

这将会返回和之前的查询同样的结果。你可以看到一些事情已经改变了。一方面，我们不在使用查询字符串参数，而是使用请求体。这个请求体是通过JSON构建，并且使用了`match`查询（查询方法之一，我们会在后后面的章节中学习）。

--------------

[« 搜索精简版](search-lite.md)      [复杂搜索 »](more-complicated-searches.md)

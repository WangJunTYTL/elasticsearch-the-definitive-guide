
短语搜索
===============

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

对一个字段进行单个词语的搜素固然不错，但是有的情况下你还是想查询和一系列单词或者短语完全匹配的结果。比如，我们想通过短语“rock climbing”查询找到包含"rock"和"climbing"两个单词彼此相邻的员工记录。

要实现这个目的，我们使用"query"查询的一个轻量级变种——称之为"match_phrase"的查询：

```shell
GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
```

毫无疑问，只返回`John Smith`的文档：

```
{
   ...
   "hits": {
      "total":      1,
      "max_score":  0.23013961,
      "hits": [
         {
            ...
            "_score":         0.23013961,
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            }
         }
      ]
   }
}
```


----------------------
[« 全文搜索](full-text-search.md)      [高亮搜索结果 »](highlighting-our-searches.md)


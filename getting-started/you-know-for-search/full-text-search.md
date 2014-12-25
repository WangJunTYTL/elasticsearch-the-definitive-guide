
全文搜索
==============

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


到目前为止的搜索都还很简单：单一名称、通过年龄过滤。我们来尝试更高级一些的全文搜索，这经常是困扰传统数据库的一个问题。

我们将搜索所有喜欢攀岩(`rock climbing`)的员工。


```shell
GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
```

你可以看到我们使用和之前一样的`match`查询来搜索`about`字段包含"rock climbing"的文档。我们得到两个匹配的文档：

```
{
   ...
   "hits": {
      "total":      2,
      "max_score":  0.16273327,
      "hits": [
         {
            ...
            "_score":         0.16273327,   <1>
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            }
         },
         {
            ...
            "_score":         0.016878016,  <2>
            "_source": {
               "first_name":  "Jane",
               "last_name":   "Smith",
               "age":         32,
               "about":       "I like to collect rock albums",
               "interests": [ "music" ]
            }
         }
      ]
   }
}
```
 
> [1] [2] 相关性得分

-------


默认情况下，Elasticsearch按照相关性得分对匹配结果进行排序，也就是说，通过每个文档和搜索有多匹配进行排序。第一个得分高很明显：`John Smith`的`about`字段明显包含了`rock climbing`。


但是，为什么Jane Smith也被返回来了呢？这是因为在他的`about`字段中提到了`rock`。因为只有一个“rock"被提到，没有包含"climbing"，她的`_score`值也低于John的。


这是一个很好的例子，展示了Elasticsearch如何对全文字段进行搜索，并优先返回最相关的结果。相关性是Elasticsearch中很重要的概念，而传统关系型数据库只有匹配或者不匹配的概念，相关性完全和它们无缘。

-----------------------

[« 复杂点的搜索](more-complicated-searches.md)      [短语搜索 »](phrase-search.md)


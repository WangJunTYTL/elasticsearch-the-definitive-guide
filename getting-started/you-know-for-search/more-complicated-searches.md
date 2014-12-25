

复杂点的搜索
=========

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

让我们进行一些更加复杂的搜索。我们任然想查找`last name`是`Smith`的员工，但是年龄要大于30岁。我们的查询将使用过滤器（帮助我们使结构化查询执行更有效率），对查询进行小小改动：


```shell
GET /megacorp/employee/_search
{
    "query" : {
        "filtered" : {
            "filter" : {
                "range" : {
                    "age" : { "gt" : 30 }  <1>
                }
            },
            "query" : {
                "match" : {
                    "last_name" : "smith" <2>
                }
            }
        }
    }
}
```

> [1] 这部分查询是一个`range`过滤器，用来查找所有年龄大于30岁的员工。
> [2] 这部分查询和之前是一样的`match`查询。

----------------------

现在不用太担心这些语法；我们将在后续的章节中进一步学习。只是认识到我们添加了一个过滤器进行范围查询，并且重新使用了之前的`match`查询。现在我们的结果只展示了一个员工，他的年龄刚好是32岁（大于30岁），名字叫`Jane Smith`：


```
{
   ...
   "hits": {
      "total":      1,
      "max_score":  0.30685282,
      "hits": [
         {
            ...
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

----------------

[« 查询DSL搜索](search-with-query-dsl.md)      [全文搜索 »](full-text-search.md)


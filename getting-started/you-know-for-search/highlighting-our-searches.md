
高亮搜索结果
========

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


很多程序对结果进行高亮显示，这样用户可以知道为什么这个文档被匹配上。在Elasticsearch要实现高亮显示结果是非常简单的。

让我们重新运行前面的查询，但是增加一个`highlight`的参数：

```shell
GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}
```


当我们运行这个查询，`hit`字段还是会和之前一样返回来，但是我们从返回结果中找到一个新的字段，叫做`highlight`。它把`about`字段中匹配上的单词使用HTML代码`<em></em>`标识出来了：

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
               "about":       "I love to go rock climbing", <1>
               "interests": [ "sports", "music" ]
            },
            "highlight": {
               "about": [
                  "I love to go <em>rock</em> <em>climbing</em>" 
               ]
            }
         }
      ]
   }
}
```

> [1] 高亮片段的原文

-------------------

你可以在《[高亮参考文档](../)》一节中详细了解关于高亮的内容。

-----------------------

[« 短语搜索](phrase-search.md)     [分析 »](analytics.md)


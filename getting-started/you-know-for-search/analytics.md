
分析
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


最后，我们进入到最后一些业务需求：允许管理员对员工目录进行分析。Elasticsearch有聚合的功能，允许你对数据进行精细地分析。这类似于SQL语句里的`GROUP BY`，但是要更加强大。

例如，我们查找我们的员工最感兴趣的事情：

```shell
GET /megacorp/employee/_search
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "interests" }
    }
  }
}
```

现在先忽略语法，只看结果：

```
{
   ...
   "hits": { ... },
   "aggregations": {
      "all_interests": {
         "buckets": [
            {
               "key":       "music",
               "doc_count": 2
            },
            {
               "key":       "forestry",
               "doc_count": 1
            },
            {
               "key":       "sports",
               "doc_count": 1
            }
         ]
      }
   }
}
```

我们可以看到有两个员工对`music`感兴趣，一个喜欢`forestry`，另外一个喜欢`sports`。这个聚合不是预先计算好的，它们是在查找匹配当前查询的文档时产生的。如果我们想知道叫`Smith`的人的兴趣所在，我们可以把对应的`query`加入进来：

```shell
GET /megacorp/employee/_search
{
  "query": {
    "match": {
      "last_name": "smith"
    }
  },
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests"
      }
    }
  }
}
```

这个`all_interests`聚合已经发生变化了，只包含了匹配查询的文档：

```
  ...
  "all_interests": {
     "buckets": [
        {
           "key": "music",
           "doc_count": 2
        },
        {
           "key": "sports",
           "doc_count": 1
        }
     ]
  }
```

聚合也允许分级汇总。比如，我们要查找有相同兴趣的人的平均年龄：

```
GET /megacorp/employee/_search
{
    "aggs" : {
        "all_interests" : {
            "terms" : { "field" : "interests" },
            "aggs" : {
                "avg_age" : {
                    "avg" : { "field" : "age" }
                }
            }
        }
    }
}
```

这个聚合返回的结果有一些复杂，不过还是很容易理解：

```
  ...
  "all_interests": {
     "buckets": [
        {
           "key": "music",
           "doc_count": 2,
           "avg_age": {
              "value": 28.5
           }
        },
        {
           "key": "forestry",
           "doc_count": 1,
           "avg_age": {
              "value": 35
           }
        },
        {
           "key": "sports",
           "doc_count": 1,
           "avg_age": {
              "value": 25
           }
        }
     ]
  }
```

输出的内容和我们的第一个聚合基本一致。我们仍然又一个兴趣列表及其数量，但是每个兴趣有一个额外的`avg_age`字段，显示了有这个共同兴趣的员工的平均年龄。

即使你不懂得上述语法，你也很容易了解到通过这个功能可以实现复杂的聚合和分组。没有限制，什么样的数据都可以提取！

-------------

[« 高亮搜索结果](highlighting-our-searches.md)      [教程总结 »](tutorial-conclusion.md)

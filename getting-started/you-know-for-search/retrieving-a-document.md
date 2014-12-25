
检索文档
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

现在我们已经有一些数据存在Elasticsearch，我们可以开始处理程序的业务需求。第一个需求是获取单个的员工数据。

对Elasticsearch来说这是一件很简单的事情。我们简单执行一条HTTP GET请求并且制定文档的地址（索引、类型、ID）即可。使用这三项信息，我们可以得到原始的JSON文档：

```
GET /megacorp/employee/1
```

响应包含一些文档的元数据，以及包含`John Smith`原始JSON文档的`_source`字段。

```json
{
  "_index" :   "megacorp",
  "_type" :    "employee",
  "_id" :      "1",
  "_version" : 1,
  "found" :    true,
  "_source" :  {
      "first_name" :  "John",
      "last_name" :   "Smith",
      "age" :         25,
      "about" :       "I love to go rock climbing",
      "interests":  [ "sports", "music" ]
  }
}
```

> 提示
-----
我们将HTTP的方法从`PUT`换成`GET`，用来检索文档。同理，使用`DELETE`方法则会删除文档，而使用`HEAD`方法可以检查文档是否存在。要对已有的文档进行更新，则可以再次使用`PUT`方法。

-------------------

[« 索引员工文档]     [搜素精简版 »](search-lite.md)

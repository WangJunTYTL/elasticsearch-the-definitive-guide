
搜索精简版
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
* [高亮搜索](highlighting-our-searches.md)
* [分析](analytics.md)
* [教程结论](tutorial-conclusion.md)
* [分布式特性](distributed-nature.md)
* [接下来](next-steps.md)

`GET`相当简单——你拿到了你要求的文档。我们来试试一些更先进的事情，比如一个简单的搜索！


我们尝试的第一次搜索尽可能简单。我们将使用如下的请求搜索所有的员工：

```
GET /megacorp/employee/_search
```

你可以看到我们仍然使用`megacorp`索引和`employee`类型，但没有指定文档的ID，而是用`_search`取而代之。返回的结果中在`hits`数组中包含全部3个文档。默认情况下，搜索会返回签10个结果。

```
{
   "took":      6,
   "timed_out": false,
   "_shards": { ... },
   "hits": {
      "total":      3,
      "max_score":  1,
      "hits": [
         {
            "_index":         "megacorp",
            "_type":          "employee",
            "_id":            "3",
            "_score":         1,
            "_source": {
               "first_name":  "Douglas",
               "last_name":   "Fir",
               "age":         35,
               "about":       "I like to build cabinets",
               "interests": [ "forestry" ]
            }
         },
         {
            "_index":         "megacorp",
            "_type":          "employee",
            "_id":            "1",
            "_score":         1,
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            }
         },
         {
            "_index":         "megacorp",
            "_type":          "employee",
            "_id":            "2",
            "_score":         1,
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
> 备忘
------
> 响应不仅告诉我们哪些文档被匹配上，还包含了文档本身：搜索结果已经告诉我们所有需要展示的信息。

下一步，让我们尝试搜索`last name`包含“Smith”的员工。要达到这个目的，我们将使用一种容易通过命令行实现的轻量级搜索方法。这个方法因为我们通过网址的查询参数实现，通常被成为查询字符串搜索：

```
GET /megacorp/employee/_search?q=last_name:Smith
```
我们同样在网址中使用`_search`方法，在后面增加了一个查询参数`q`。返回的结果显示所有叫`Smith`的员工：

```
{
   ...
   "hits": {
      "total":      2,
      "max_score":  0.30685282,
      "hits": [
         {
            ...
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

--------------------

[« 检索文档](retrieving-a-document.md)      [通过查询DSL搜索 »](search-with-query-dsl.md)

	

索引文档
==========

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

首先要做是存储员工数据。这需要将员工的文档采用这样的形式：一个文档对应一个员工。Elasticsearch里边存储数据的动作被称为索引，在索引一个文档之前，我们需要先决定在什么地方存储它。

对Elasticsearch而言，文档属于类型，这些类型构成索引。你可以将它和传统关系型数据库做一些粗略的对比：

> 关系型数据库      ⇒ 数据库  	⇒ 数据表 	⇒ 行	⇒ 列
Elasticsearch  	⇒ 索引   	⇒ Types  	⇒ 文档	⇒ 字段

一个Elasticsearch集群可以包含多个索引（数据库），每个索引包含多个类型（表）。这些类型包含多个文档（行），每个文档包含多个字段（列）。

> 索引 VS  索引 VS 索引
------------------------

> 你可以注意在Elasticsearch的上下文里，索引被赋予了多种意思。有必要对它们做一些澄清：


> ### 索引（名词）
>> 之前解释过，索引类似于传统关系型数据库里边的数据库。它是存储相关文档的地方。索引(`index`)的复数是`indices`或者`indexes`。


> ### 索引（动词）
----------------
>> 索引一个文档，就是将文档存储到索引（名词），这样它才能被检索和查询。这像SQL表达式中的`INSERT`关键词。如果文档已经存在，新的文档将替换旧的。

> ### 反向索引
-------------
>> 关系型数据库给指定的列添加索引（比如B-tree索引）以提升数据检索的速度。Elasticsearch和Lucene使用叫做反向索引的结构，以达到同样的目的。

>> 一般的，文档里的每一个字段是被索引的（有一个反向索引），因此可以被搜索。没有反向索引的字段则不能被搜索。我们将在《[反向索引](../getting-started/inverted-index.md)》更加深入地讨论反向索引。

所以对于我们的员工目录，我们要做到如下几点：

* 给每个员工索引一个文档，包含员工的详细情况。
* 文档的类型是`employee`。
* 这个类型存在于索引`megacorp`里。
* 这个索引属于我们的Elasticsearch集群。


这看起来需要很多步骤，但实际上很容易实现。我们可以将所有的动作通过一个简单的命令予以实现：

```shell
PUT /megacorp/employee/1
{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
```


请注意路径`/megacorp/employee/1`包含三方面的信息：

### `megacorp`
> 索引名称


### `employee`
> 类型名称

### `1`
特定的员工ID

请求体（JSON文档）包含该员工的所有信息。他的名字是`John Smith`，年龄`25`岁，喜欢`rock climbing`(攀岩)。

简单！这不需要运行任何管理员任务，比如创建索引或者指定每个字段包含的数据类型。我们可以仅仅是直接索引一个文档。Elasticsearch已经对所有事情都设定了默认值，所有必须的管理任务已经在后台按照默认值进行处理了。

在进一步行动之前，我们再给目录多添加几条员工信息：

```
PUT /megacorp/employee/2
{
    "first_name" :  "Jane",
    "last_name" :   "Smith",
    "age" :         32,
    "about" :       "I like to collect rock albums",
    "interests":  [ "music" ]
}


PUT /megacorp/employee/3
{
    "first_name" :  "Douglas",
    "last_name" :   "Fir",
    "age" :         35,
    "about":        "I like to build cabinets",
    "interests":  [ "forestry" ]
}
```


----------------
[« 找找感觉](finding-your-feet.md)      [检索文档 »](retrieving-a-document.md)

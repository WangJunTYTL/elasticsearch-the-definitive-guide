

找找感觉
============

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


为了让你对Elasticsearch能做什么以及它的易用性有一些感觉，我们开始通过一些简单的教程来了解一些基本概念，比如索引、搜索、聚合。

我们将逐步介绍一些新的术语和概念，如果你没有全部理解也没有关系，因为我们在这本书的剩余部分将从更深的层次上来介绍这些概念。

所以，先来快速了解一下Elasticsearch都能干哪些事情吧：

让我们建立一个员工目录
-------------------

我们偶然为`Megacop`工作，作为HR提倡的"We love our drones!"的一部分内容，我们要完成创建员工目录的任务。这个目录用来促进同事交流，提供实时、协同、动态的合作，因此有一些新的业务需求：

* 数据支持包含多个标签、数值、以及全文。
* 能获取任何员工的全部信息。
* 允许结构化查询，比如查找年龄大于30的员工。
* 允许简单的全文搜索以及更加复杂的短语搜索。
* 允许高亮显示匹配结果中的搜索片段。
* 支持建立数据分析报表。


------------------

[« 面向文档](document-oriented.md)      [索引员工文档 »](indexing-employee-documents.md)


搜索—搜索工具
===============

* [空搜索](the-empty-search.md)
* [多索引，多类型](multi-index-multitype.md)
* [分页](pagination.md)
* [搜索精简版](search-lite.md)

到目前为止，我们已经学会了如何将elasticsearch作为简单的类NoSQL的分布式文件存储来使用。我们可以把JSON文档放到elasticsearch里存取，通过ID检索。但elasticsearch真正的能力在于能够从混乱中找回秩序-把大数据转化为大信息。

这是我们使用结构化JSON文档的原因，而不是乱七八糟得数据。Elasticsearch不仅仅存储数据，而且对文档的内容进行索引，使其可搜索。

文档的每一个字段都被索引且可搜索。不过远不止如此，在进行一个单独的查询时，Elasticsearch能使用所有的索引，瞬息之间返回结果。这是使用传统数据库不敢想象的。

一个搜索可以是下面的任意一种：

* 基于具体字段（如性别或年龄），通过一个字段排序（如`join_date`）的结构化查询，通过和SQL语句类似的方式构建
* 全文查询，找出和搜索关键词匹配的全部文档，并返回它们的相关性排序
* 两者的结合


虽然许多搜索会解决的方块，要发挥出Elasticsearch的全部潜力，你需要了解三个方面：

* 映射
> 数据的每个字段如何解释

* 分析
> 全文字段如何被处理，使其可搜索

* 查询DSL
> Elasticsearch使用的灵活、强大的查询语言

这三个方面单独来看都是一个很大的主题，我们将在《[深度搜索](search-in-depth.md)》一节中予以详细地解释。本节章将介绍这三者的基本概念，帮助你对搜索运行机制有一个全面的理解。

我们将从解释简单的搜索API开始。

> 测试数据
---------
本章我们用来测试的数据可以通过这个`gits`找到：[https://gist.github.com/clintongormley/8579281](https://gist.github.com/clintongormley/8579281)。

> 为了更好学习本章，你可以复制粘贴它们到你的终端运行。

> 另外，如果你是使用本书的在线版，可以直接点击[这里](http://www.elasticsearch.com/guide/en/elasticsearch/guide/current/sense_widget.html?snippets/050_Search/Test_data.json)打开。

-----------------------------
[« 多文档模式](../distributed-document-store/multidocument-patterns.md)     [空搜索 »](the-empty-search.md)

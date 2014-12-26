
分布式特性
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

在这一章的开头，我们说过Elasticsearch可以扩展到上百（甚至上千）台服务器，处理PB级别的数据。我们的教程只是给出了一些如何使用Elasticsearch的例子，完全没有提到相应的机制。Elasticsearch具有分布式的特性，而且极具匠心地隐藏了分布式带来的复杂性。

Elasticsearch的分布方面，基本上是透明的。前面的教程根本不需要你对分布式系统、分片、集群的发现，或者其他分布式概念有任何了解。你可以很高兴地在台式机单节点上运行教程，不过如果你准备在包含100个节点的集群跑教程，一切都会按照同样的方式。

Elasticsearch力图隐藏分布式系统的复杂性。这里有一些自动运行的操作：

* 将文档分配到不同的容器或者分片，可以存储在一个或多个节点上。
* 平衡这些节点分片的索引和搜索的负载。
* 复制每个分片提供数据的冗余备份，防止数据因为硬件故障而丢失。
* 可以从急群众的任何节点路由定位到你需要的数据。
* 集群增长时无缝集成新节点；节点丢失后重新部署分片。

这些章节将告诉你集群如何扩展和实现故障转移（《[分布式集群](../../distributed-cluster/README.MD)》），处理文档存储（《[分布式文档存储](../../distributed-document-store/README.MD)》，执行分布式搜索（《[分布式搜索](../../distributed-search/README.MD)》，还有分片是什么以及如何工作（《[分片](../../life-inside-shard/README.MD)》）。

阅读这些章节不是必须的，不理解这些内部机制也可以使用Elasticsearch，但是它们将是你的Elasticsearch的只是更加完整，提升你的洞察力。你也可以忽略它们，在你需要一些复杂理解时再来查看它们。

----------------------------

[« 教程总结](tutorial-conclusion.md)      [接下来 »](next-steps.md)

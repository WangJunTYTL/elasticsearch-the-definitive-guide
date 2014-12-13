文档的分布式存储
==============

* [路由](routing-a-document-to-a-shard.md)
* [主分片和复制分片如何交互](how-primary-and-replica-shards-interact.md)
* [创建、索引、删除文档](creating-indexing-and-deleting-a-document.md)
* [访问文档](retrieving-a-document.md)
* [部分更新文档](partial-updates-to-a-document.md)
* [多文档模式](multidocument-patterns.md)

在前面的章节中，我们了解了所有对数据进行索引和查找的方法，但我们忽略了许多相关的技术细节。比如数据是如何在集群分布和获取。这种隔离是故意的；你不需要知道Elasticsearch是数据是如何分布的。反正是可行的。

在这一章中，我们将深入内部了解技术细节，帮您理解数据是如何存储在一个分布式系统。

> 内容警告
---------
本章的内容只适合高级用户，主要用来照顾你的兴趣。要使用Elasticsearch，并不需要理解和记住所有的细节。

> 阅读这些内容了解内部如何运转，将来如果需要的话也知道从什么地方查找，但是千万不要被这些细节所淹没。


«  cheaper in bulk     [路由 »](routing-a-document-to-a-shard.md)

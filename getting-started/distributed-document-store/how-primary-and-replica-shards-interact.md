
主分片和复制分片如何交互
====================


* [路由](routing-a-document-to-a-shard.md)
* [主分片和复制分片如何交互](how-primary-and-replica-shards-interact.md)
* [创建、索引、删除文档](creating-indexing-and-deleting-a-document.md)
* [访问文档](retrieving-a-document.md)
* [部分更新文档](partial-updates-to-a-document.md)
* [多文档模式](multidocument-patterns.md)

为了便于理解，让我们想象有一个三节点的集群，它包含一个叫`blogs`的索引，有两个主分片。每个主分片分别有两个复制分片。因为同一个复制分片不会被分配到同一个节点，所有我们的集群会看起来如图8所示：“三节点和一个索引的集群”。


![三节点和一个索引的集群](elas_0401.png)
图8. 三节点和一个索引的集群

我们可以向任何一个节点发送请求。每一个节点都能处理所有的请求。每个节点知道每个文档所在的位置，能将请求指向正确的节点。在后续的例子中，我们将把所有的请求发给节点1，称之为`请求节点`。


> 提示
-------
发送请求的时候，最好使用轮询的方式访问所有急群中的节点，用以实现负载均衡。

[« 路由](routing-a-document-to-a-shard.md)      [创建、索引、删除文档 »](creating-indexing-and-deleting-a-document.md)

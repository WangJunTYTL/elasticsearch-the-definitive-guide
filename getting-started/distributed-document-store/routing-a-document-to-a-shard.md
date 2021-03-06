路由
===

* [路由](routing-a-document-to-a-shard.md)
* [主分片和复制分片如何交互](how-primary-and-replica-shards-interact.md)
* [创建、索引、删除文档](creating-indexing-and-deleting-a-document.md)
* [访问文档](retrieving-a-document.md)
* [部分更新文档](partial-updates-to-a-document.md)
* [多文档模式](multidocument-patterns.md)

文档在建索引的时候会被存储到一个主分片。那么Elasticsearch是如何知道一个文档属于哪个分片？当我们创建一个文档，它如何决定这个文档该存到分片1还是分片2呢？

这个处理过程不能是随机的，因为我们后面还需要把文件取出来。实际上，这是通过下面这个简单的公式算出来的

```shell
    shard = hash(routing) % number_of_primary_shards
```

`routing`的值时一个任意的字符串，默认就是文档的`_id`，但可以被设置为一个指定的值。这个`routing`字符串通过一个哈希函数产生一个数字，然后对索引的主分片数量求余。余数会是`0`到`number_of_primary_shards - 1`中的一个值，直接对应到文档所在分片的号码。

这也解释了，为什么主分片的数量只能在索引创建的时候设置，之后不能改动：如果主分片数量被改动，所有之前路由的值将无效，文档也找不到了。

> 笔记
--------
有时候用户认为固定主分片的数量使得之后很难扩展。实际上，有技术可以很容易地实现这样的扩展。我们会在《[针对扩展的设计](designing-for-scall.md)》这一节将会详细讨论。


所有文档的API（`get`，`index`，`delete`，`bulk`，`update`，以及`mget`）都可以接受一个`routing`的参数，通过它来指定文档到分片的路由策略。自定义路由值可被用来确保所有相关文档（比如属于同一个用户的文档）被存储在同一个分片。我们将在《[针对扩展的设计](designing-for-scall.md)》一节中详细讨论这样做的理由。

----------------------------
[« 文档的分布式存储](distributed-document-store)     [主分片和复制分片如何交互 »](how-primary-and-replica-shards-interact.md)

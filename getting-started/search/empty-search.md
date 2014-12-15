空搜索
=============

* [空搜索](the-empty-search.md)
* [多索引，多类型](multi-index-multitype.md)
* [分页](pagination.md)
* [搜索精简版](search-lite.md)

最简单得搜索API就是空搜索，即不指定任何查询条件、返回集群里边所有索引的所有数据：

```shell
GET /_search
```
返回的内容如下（经过整理精简）：

```json
{
   "hits" : {
      "total" :       14,
      "hits" : [
        {
          "_index":   "us",
          "_type":    "tweet",
          "_id":      "7",
          "_score":   1,
          "_source": {
             "date":    "2014-09-17",
             "name":    "John Smith",
             "tweet":   "The Query DSL is really powerful and flexible",
             "user_id": 2
          }
       },
       ... 9 RESULTS REMOVED ...
      ],
      "max_score" :   1
   },
   "took" :           4,
   "_shards" : {
      "failed" :      0,
      "successful" :  10,
      "total" :       10
   },
   "timed_out" :      false
}
```

`hits`
-------------

返回结果中最重要的的部分是`hits`，包含所有和查询匹配的文档个数，`hits`数组包含最前面的10个匹配的文档。

`hits`数组中每个元素包含`_index`、`_type`和`_id`字段，以及`_source`字段。这意味着我们直接从搜索结果获取到文档。而其他搜索引擎，往往只返回文档ID，要求你通过另外一个单独的步骤读取文档本身。

每个元素还包含一个`_score`字段。这是相关性得分，用来衡量文档与查询匹配程度的。默认情况下，返回的结果按照`_score`降序排列。在这种情况下，我们没有指定任何查询，所以所有的文件都同样重要，因此返回结果的所有`_score`都为1。

`max_score`则是所有匹配结果中最大的`_score`值。

`took`
-------

`took`值告诉我们整个查询请求花费了多少毫秒。

`shards`
-------------

`_shard`元素告诉我们有多少分片参与了查询，其中有多少查询成功，有多少失败了。我们通常不会想到分片失败，但它可能发生。如果我们遭受重大灾难，导致失去一个主分片和与之相同的复制分片，就不会有任何的分片可以提供搜索。在这种情况下，Elasticsearch会报告分片失败了，但继续从返回其他分片的结果。



`timeout`
--------------

`timed_out`值告诉我们查询是否超时。默认情况下搜索查询是不会超时的。如果你认为响应时间比完整结果要重要，你可以将`timeout`指定为10或者10毫秒，或者1s：

```shell
GET /_search?timeout=10ms
```

Elasticsearch会将它在超时前从各个分片收集到的任何结果返回。

> 警告
-------------------------
应当指出的是，`timeout`并不会中止查询；它只是告诉协调节点返回到目前为止收集的结果，并关闭连接。在这种背景下，即使结果已经返回，但其他的分片可能仍在处理查询。

> 使用超时是因为它对于你的SLA（服务水平协议）很重要，而不是因为你想中断长时间运行的查询.

[« 搜索——基本工具](README.MD)     [多索引，多类型](multi-index-multitype.md »)

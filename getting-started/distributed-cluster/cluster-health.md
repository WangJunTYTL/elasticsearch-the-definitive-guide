
集群健康
=========

* [空集群](an-empty-cluster.md)
* [集群健康](cluster-health.md)
* [添加索引](add-an-index.md)
* [故障转移](add-failover.md)
* [水平扩展](scale-horizontally.md)
* [应对失败](coping-with-failure)

Elasticsearch集群可以进行很多统计，但最最重要的一项还是集群的健康——显示为绿色、黄色或者红色的状态。

```shell
	GET /_cluster/health
```
对于一个没有索引的空集群，可能会返回如下的内容：

```json
{
   "cluster_name":          "elasticsearch",
   "status":                "green", 
   "timed_out":             false,
   "number_of_nodes":       1,
   "number_of_data_nodes":  1,
   "active_primary_shards": 0,
   "active_shards":         0,
   "relocating_shards":     0,
   "initializing_shards":   0,
   "unassigned_shards":     0
}
```

`status`字段是我们最感兴趣的一项，代表集群运转的整体表现，其三种颜色代表的意义如下：

`green`
> 所有分片正常。

`yellow`
> 主分片正常，不是所有复制分片都正常。

`red`
> 不是所有主分片都正常。

接下来的章节，我们将解释什么是主分片，什么是复制分片，以及上述颜色实际产生的影响。


--------------------------------------------------------------------------------

[« 空集群](an-empty-cluster.md)                [添加索引 »](add-an-index.md)


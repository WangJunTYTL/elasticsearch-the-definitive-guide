聚合
========

[高级概念](high-level-concepts.md)
[测试驱动的聚合](aggregation-test-drive.md)
[可用的桶和度量](available-buckets-and-metrics.md)
[创建矩形图](building-bar-charts.md)
[看时间](looking-at-time.md)
[范围聚合](scoping-aggregations.md)
[过滤查询和聚合](filtering-queries-and-aggregations.md)
[过滤多个桶](sorting-multivalue-buckets.md)
[近似聚集](approximate-aggregations.md)
[重要术语](significant-terms.md)
[控制内存和延迟](controlling-memory-use-and-latency.md)
[最后的想法](closing-thoughts.md)

直到现在，这本书一直致力于搜索。我们用搜索查询与之匹配的文档。我们正在大海捞针。

通过聚合，我们聚焦到数据的概况。不再针对单个的文档，我们要分析和总结数据的总况：

* 大海里有多少根针
* 针的平均长度是多少
* 被制造商损坏的针，平均长度是多少
* 海里每个月增加多少针

聚合可以解决更多微秒的问题：
* 你最满意的制造商都有谁？
* 是否有任何不寻常或者反常的针吗？

聚合允许我们对数据问一些复杂的问题。虽然它和搜索完全不同，但它们使用相同的数据结构。这意味着聚合和搜索一样执行快速，近乎实时。

这个非常适合报表和仪表盘。你可以很快将数据可视化，快速响应，而不需要对你的数据进行汇总（一个糟糕的Hadoop任务需要运行一周）。

最后，聚合操作也是通过搜索请求，这意味着你可以通过一个请求，对同一数据进行搜索/过滤和分析。因为聚合是在搜索的上下文进行计算，所以，不仅仅可以得到四星级酒店的数量，你可以获取到满足你搜索提交的四星级酒店数量。

聚合如此重要，很多公司已经建立大型Elasticsearch集群用来分析。


[« 语音匹配](../../phonetic-matching.md)      [高级概念 »](high-level concepts.md)

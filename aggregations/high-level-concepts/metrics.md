度量
======

[桶](buckets.md)
[度量](metrics.md)
[组合两者](combining-the-two.md)

桶将文件区分成有用的子集，但最终我们想要的通过度量计算每个桶中的文件。桶是达到目的的一个手段：它提供了一种方法来组织文档,使得你可以计算出有趣的指标。

大部分度量都是简单的数学运算（比如`min`、`mean`、`max`和`sum`）计算文档的值。在实际应用中，度量允许你计算数量，如平均工资，或最大的销售价格，或第九十五百分位数的查询延迟(or the 95th percentile for query latency)。


[« 捅](buckets.md)     [组合两者 »](combining-the-two.md)

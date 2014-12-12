高级概念
========


[桶](buckets.md)
[度量](metrics.md)
[两者结合](combining-the-two.md)

像DSL，聚合有组合语法：独立的功能单元可以组合和相互匹配，满足你的个性化需求。这意味着只需要学习很少的概念，但是可以形成很多种组合。

要掌握聚合，你需要理解好下面两个概念：

* 桶

> 符合标准的文档集合。

* 度量

> 对桶中的文件计算的统计结果。


每个聚合就是一个或多个桶以及0个或多个度量的组合。初略用SQL表示就是：

```sql
SELECT COUNT(color) 
FROM table
GROUP BY color 
```

`COUNT(color)`相当于一个`度量`。

`GROUP BY color`相当于一个`桶`。

桶和SQL中的group概念很像，而度量则类似于`COUNT()`、`SUM()`、`MAX()`等等。

让我们深入研究这两个概念，看看到底意味着什么。

--------------------------------
[« 聚合](README.MD)     [桶 »](buckets.md) 

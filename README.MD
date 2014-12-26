ElasticSearch权威指南
===================================

* 英文标题：Elasticsearch: The Definitive Guide
* 英文github地址：[https://github.com/elasticsearch/elasticsearch-definitive-guide/](https://github.com/elasticsearch/elasticsearch-definitive-guide/)
* 英文书地址：[http://www.elasticsearch.com/guide/en/elasticsearch/guide/current/index.html](http://www.elasticsearch.com/guide/en/elasticsearch/guide/current/index.html)

[序言](preface.md)
---------
* 谁应该读这本书
* 为什么写此书
* ElasticSearch版本
* 如何阅读这本书
* 导读
* 在线资源
* 规范
* 使用范例
* 致谢

[入门](getting-started/README.MD)
------------------------------
* [你知道的，为了搜索...](getting-started/you-know-for-search/README.MD)
* [分布式集群](getting-started/distributed-cluster/README.MD)
* [数据](getting-started/data-in-data-out/README.MD)
* [分布式文档存储](getting-started/distributed-document-store/README.MD)
* [搜索-基本工具](getting-started/search/README.MD)
* [映射和分析](getting-started/mapping-analysis/README.MD)
* [请求体搜索](getting-started/full-body-search/README.MD)
* [排序和相关性](getting-started/sorting-and-relevance/README.MD)
* 分布式搜索
* 索引管理
* 分片

高级搜索
--------
* 结构化搜索
* 全文搜索
* 多字段搜索
* 近似匹配
* 部分匹配
* 控制相关

自然语言搜索
------------
* 开始使用语言
* 分词
* 规范标记
* 减少干扰词
* reducing words to their root form
* 停用词:性能和精准度
* 同义词
* 错别字

[聚合](aggregations/README.MD)
-------
* [高级概念](aggregations/high-level-concepts/README.MD)
* [测试驱动聚合](aggregations/aggregation-test-drive/README.MD)
* 可用的容量和指标
* 创建条形图
* 关于时间
* 范围聚合
* 查询和聚合的过滤
* 多值字段排序
* 近似聚合
* 重要条款
* 控制内存使用和延迟
* 结束联想

地理坐标
---------
* 经纬度
* 经纬度哈希值
* 经纬度聚合
* 经纬度形状

数据建模
----------
* 理清关系
* 嵌套对象
* 父子关系
* 考虑规模

管理、监控和部署
----------------
* 监控
* 部署到生产线
* 运维

映射和分析
===========

* [精确值和全文](exact-values-versus-full-text.md)
* [倒排索引](inverted-index.md)
* [分析和分析器](analysis-and-analyzers.md)
* [映射](mapping.md)
* [复杂核心类型](complex-core-field-types.md)

在使用我们索引里的数据是，我们发现了一些奇怪的地方。在我们的索引里存了12条推文（tweet），其中有一条包含日期2014-09-15，但是一起来看一下下面这些查询命中的条数：

```shell
GET /_search?q=2014              # 12 results
GET /_search?q=2014-09-15        # 12 results !
GET /_search?q=date:2014-09-15   # 1  result
GET /_search?q=date:2014         # 0  results !
```

为什么使用_all字段查询完整日期，能返回所有的tweet，但是只用年份查询`date`字段却没有返回任何结果呢？通过字段`_all`和`date`查询的结果差异为什么这么大呢？

相比，这是因为数据在`_all`字段和`date`字段索引的方式不一样导致的。所以让我们通过`gb`索引的映射（或者结构定义）来看看到底Elasticsearch如何解释我们的文档结构：

```shell
GET /gb/_mapping/tweet
```
返回结果如下：

```json
{
   "gb": {
      "mappings": {
         "tweet": {
            "properties": {
               "date": {
                  "type": "date",
                  "format": "dateOptionalTime"
               },
               "name": {
                  "type": "string"
               },
               "tweet": {
                  "type": "string"
               },
               "user_id": {
                  "type": "long"
               }
            }
         }
      }
   }
}
```

Elasticsearch已经基于对字段类型的推测，动态生成了一个映射。返回的结果表明`date`字段已经被识别为`date`类型，而`_all`字段因为是一个默认字段，并没有显示出来，但是我们知道它是string类型的。

`date`和`string`类型的字段是通过不同的方式被索引的，这也就导致他们搜索结果的不一致。这不足为奇。你可能希望每种核心数据类型（字符串、数字、布尔型、日期）索引方式存在细微差别。实际上也是如此，它们有着细微的差别。

但目前为止最大的不同在于精确值字段（包括`tring`字段）和全文字段。这种区别至关重要，它是搜索引擎区别其他数据库的重要标志。

--------------------------------

[精确值和全文 »](exact-values-versus-full-text.md) 

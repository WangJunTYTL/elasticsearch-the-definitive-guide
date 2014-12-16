搜索精简版
=========

* [空搜索](the-empty-search.md)
* [多索引，多类型](multi-index-multitype.md)
* [分页](pagination.md)
* [搜索精简版](search-lite.md)

有两种形式的搜索API：精简的查询字符串版——所有的参数都通过查询字符串表达；完整请求体版本——通过JSON格式的请求体查询，这种查询体被称为DSL，有着丰富的搜索语言。


在命令行中运行字符串搜索是很管用的。比如，查找`tweet`类型中`tweet`字段包含`elastaticsearch`的文档：

```shell
GET /_all/tweet/_search?q=tweet:elasticsearch
```
下一个查询查找`name`字段为`john`，`tweet`字段为`mary`的文档，真正的查询字符串为：

`+name:john +tweet:mary`

但是，查询字符串参数需要被%编码，因而显得比真正的更神秘：

```shell
GET /_search?q=%2Bname%3Ajohn+%2Btweet%3Amary
```

“+”作为前缀表示这个条件必须满足。而“-”作为前缀则表示这个条件必须不被匹配。如果条件中没有“+”或者“-”则表明条件是可选的——越和条件匹配，则该文档的关联性越大。


`_all`字段
----------
这个简单的查询返回所有包含`mary`的文档：

```shell
GET /_search?q=mary
```

在上一个例子中，我们查询了`tweet`和`name`字段的词，然而，查询结果提到的`mary`在三个不同的字段：

* 一个全名为Mary的用户
* Mary发表的6条推文
* 一条@mary的推文

Elasticsearch·是如何通过3个不同的字段查找结果的呢？

当你索引一个文档，Elasticsearch将所有的字符串字段连接起来形成一个大的字符串，并将它放到`_all`字段索引起来。比如，当我们搜索这个文档：

```json
{
    "tweet":    "However did I manage before Elasticsearch?",
    "date":     "2014-09-14",
    "name":     "Mary Jones",
    "user_id":  1
}
```
这相当于我们添加了一个额外的`_all`字段，它的值如下：

> "However did I manage before Elasticsearch? 2014-09-14 Mary Jones 1"

这个查询字符串使用`_all`字段，除非被指定了别的名字。

> 提示
-------
当你开始使用一个新应用的时候，`_all`字段是很有用的功能。之后，当你能对搜索结果有更多控制时，你会更多地查询指定的字段。当`_all`字段对你无用的时候，你可以禁掉它，参见《[元数据：`_all`字段](metadata-_all-field.md)》。

更复杂的查询
-------------

接下来使用如下的条件查询推文：

* `name`字段包含`mary`或者`john`
* `date`字段大于`2014-09-19`
* `_all`字段包含`aggregations`或者`geo`



`+name:(mary john) +date:>2014-09-10 +(aggregations geo)`

经过正确编码的查询字符串，看起来可读性要差一点：

`?q=%2Bname%3A(mary+john)+%2Bdate%3A%3E2014-09-10+%2B(aggregations+geo)`

通过前面的例子你可以发现，精简版的查询字符串搜索出奇的精彩。它的查询语法，允许我们间接地表达非常复杂的查询。我们在《查询字符串语法》一节中对其给予了详细地介绍。这使它非常适合在命令行或者开发阶段使用。

然而，你也可以看到，它的简介让它很神秘且很难调试。而且它很脆弱，一个简单的语法问题（比如将`-`、`:`、`/`或者`"`放错位置），就会导致错误，而不会返回结果。

最后，查询字符串允许任何用户对索引中的任何字段运行缓慢、重量级的查询，容易暴露私人信息，甚至使集群瘫痪。

> 提示
------
基于这些原因，我们不推荐直接将字符串查询交给你的用户，除非他们肯定能管理好数据和集群。

相反，在生产环境，我们通常更加依赖全功能的请求体搜索API。它们能处理更多的事情。在没有了解之前，我们需要先了解Elasticsearch是如何索引我们的数据。


--------------
[« 分页](pagination.md)      [映射和分析 »](../mapping-and-analysis/README.MD)


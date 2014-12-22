排序
=======


* [排序](sorting.md)
* [字符串排序和多字段](string-sorting-and-multifields.md)
* [什么是相关性](what-is-relevance.md)
* [字段数据](fielddata.md)


为了使用相关性排序，我们需要将相关性表现为一个值。Elasticsearch将相关性分数用浮点型数值表示，在搜索结果中使用`_score`表示，默认排序是按`_score`降序。

有的情况下，相关性得分并没有什么意义。比如，下面的查询仅仅是返回`user_id`为1的推文：

```shell
GET /_search
{
    "query" : {
        "filtered" : {
            "filter" : {
                "term" : {
                    "user_id" : 1
                }
            }
        }
    }
}
```

过滤器没有影响`_score`，隐藏的`match_all`查询仅仅是把所有文档的`_score`值设为中立的1。也就是说，所有文档被认为相关性完全相同。

通过字段值排序
-------------

在这种情况下，最近发表的推文会更重要，更有相关性。我们可以通过`sort`参数予以实现：

```shell
GET /_search
{
    "query" : {
        "filtered" : {
            "filter" : { "term" : { "user_id" : 1 }}
        }
    },
    "sort": { "date": { "order": "desc" }}
}
```

你会注意到两种结果的不同：

```json
"hits" : {
    "total" :           6,
    "max_score" :       null,  <1>
    "hits" : [ {
        "_index" :      "us",
        "_type" :       "tweet",
        "_id" :         "14",
        "_score" :      null, <2>
        "_source" :     {
             "date":    "2014-09-24",
             ...
        },
        "sort" :        [ 1411516800000 ] <3>
    },
    ...
}
```
 
> [1][2] 并没有计算`_score`，因为不是通过它来进行排序。
>
> [3] 通过毫秒表示的`date`字段的值被用作排序的值。

---------------


首先，我们有一个`sort`元素出现在结果中，包含了用来排序的字段的值。在这种情况，我们通过`date`排序，内部实现则将其转化为毫秒。长整形数字`1411516800000`相当于日期`2014-09-24 00:00:00 UTC`。

其次，`_score`和`max_score`都为`null`。计算`_score`的成本很大，而且仅仅是用它来进行排序；而我们并没有通过相关性排序，使用`_score`排序就没有意义了。如果你无论如何都要计算出`_score`，可以把`track_scores`参数设置为true。


> 提示
------
作为一种快捷方式，你可以直接用字段的名称进行排序：

> ```json
    "sort": "number_of_children"
```
> 将按指定字段的升序排序，而使用`_score`计算相关性时则会按降序排列。

多级排序
-------

我们想把一个对`date`的查询和`_score`起来，先按照`date`排序，然后按照相关性排序，显示所有匹配结果：


```
GET /_search
{
    "query" : {
        "filtered" : {
            "query":   { "match": { "tweet": "manage text search" }},
            "filter" : { "term" : { "user_id" : 2 }}
        }
    },
    "sort": [
        { "date":   { "order": "desc" }},
        { "_score": { "order": "desc" }}
    ]
}
```

顺序很重要。优先按照第一个标准显示结果。只有结果的第一个排序值相同，才会考虑通过第二个排序标准排序。

多级排序并没有涉及到`_score`。你可以通过不同的字段排序，比如通过地理位置或者脚本计算等等。

> 提示
------
查询字符串搜索也支持使用`sort`参数来自定义排序：

```shell
GET /_search?sort=date:desc&sort=_score&q=search
```
多值字段排序
-----------

当对多值字段排序时，记住这些值并没有任何内在排序；一个多值字段只是有一袋子值。那么哪一个会被用来排序呢？

对于数字和日期，你可以对他们使用`min`、`max`、`avg`、或者`sum`这样的排序模式来将多个值合并成一个。比如，你可以通过下面的方式对`dates`字段按最早的日期进行排序：

```json
"sort": {
    "dates": {
        "order": "asc",
        "mode":  "min"
    }
}
```

------------------

[« 排序和相关性](sorting-and-relevance.md)     [字符串排序和多字段 »](string-sorting-and-multifields.md)

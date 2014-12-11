映射
=======

exact values versus full text
inverted index
analysis and analyzers
mapping
complex core field types
In order to be able to treat date fields as dates, numeric fields as numbers, and string fields as full-text or exact-value strings, Elasticsearch needs to know what type of data each field contains. This information is contained in the mapping.

在《[数据流入，数据流出](data-in-data-out.md)》解释过，每个索引里的文档都有一个类型，每个类型都有自己的映射，或者结构定义。映射定义了每个类型里德尔字段，每个字段的数据类型，以及Elasticsearch该如何处理每个字段。映射同样也用来配置每个类型关联的元数据。

我们将在《[类型和映射](getting-started/index-management/mapping.md)》一节中详细讨论映射。这一节的内容希望能帮助你入门。

核心简单字段类型
---------------

Elasticsearch支持下面的简单字段类型：

字符串: `string`
数字: `byte`, `short`, `integer`, `long`
浮点数: `float`, `double`
布尔值: `boolean`
日期: `date`

当你索引的文档包含了一个从未使用过的新字段，Elasticsearch将使用动态映射去推断JSON中可用的基本数据类型，规则如下：

JSON type              |  Field type
-----------------------|------------
布尔值：true或者false    | boolean
数字: 123               | long
浮点数: 123.45          | double
字符串，有效日期：2014-09-15 | date
字符串：foo bar`         | string


> 提示
-------
如果你将数字使用引号括起来("123")，这个将被映射为`string`类型，而不是`long`类型。然而，如果字段已经被映射成`long`，Elasticsearch将会尝试将字符串转化为`long`，如果不能转化，则会抛出异常。

查看索引
---------------

通过`/_mapping`接口可以查各索引里个类型的映射。在这章的开始，我们已经获取了`gb`索引的`tweet`类型的索引：

```shell
GET /gb/_mapping/tweet
```

Elasticsearch根据我们索引的文档动态生成了映射，显示了各字段的映射（叫做属性）。:

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

> 提示
-------------
不正确的映射，比如将`age`字段映射成`string`类型，查询的时候有可能产生混乱的结果。
不要假定你的映射是正确的，赶紧检查去！


定制字段映射
----------

虽然基本字段类型能满足大部分情况，你还是经常会需要为字段定制映射，尤其是`string`类型的字段。定制映射能让你：

* 明确全文字段和精确值字段
* 使用特定语言分析器
* 优化字段的部分匹配
* 定制日期类型
* 还有其他...


最重要的字段属性就是类型，除`string`类型外，很少需要指定`type`外的属性。

```json
{
    "number_of_clicks": {
        "type": "integer"
    }
}
```
通常`string`类型被认为包含全文本，也就是说，他们的值在索引前将被交给分析器分析，在搜索前也会先经过分析器。

两个最重要的映射蛇形是`index`和`analyzer`。


`index`
---------

`index`属性控制字符串如何被索引，它包含下述3种可选值：

`analyzed`
> 先分析字符串然后索引。即用全文索引该字段。

`not_analyzed`
> 索引该字段，可被搜索，但是索引的值完全确定了。不会分析它。

`no`
> 完全不索引这个字段，是不可搜索的。

`string`字段的`index`默认值是`analyzed`。如果我们希望这个字段是一个精确值，我们需要将其设置为`not_analyzed`：

```json
{
    "tag": {
        "type":     "string",
        "index":    "not_analyzed"
    }
}
```

> 提示
------
另外一个简单类型（如`long`、`double`、`date`等）也接受`index`参数，但是可选值只有`no`和`not_analyzed`，它们的值绝不会被分析。


分析器
-------

为了分析`string`类型的字段，可使用`analyzer`属性来指定搜索和索引时分析器。默认的，Elasticsearch使用标准分析器，但是你可以修改它为别的内建分析器，比如`whitespace`、`simple`或者`english`：

```json
{
    "tweet": {
        "type":     "string",
        "analyzer": "english"
    }
}
```


在《[定制分析器]()》一节，我们将高速你如何定义和使用定制的分析器。

更新映射
-----------------

通过`/_mapping`接口，在你刚开始创建索引的时候，可以给一个类型指定映射，也可以随后为一个新类型增加映射（或者未一个已经存在的类型更新映射）。


> 提示
-------------
虽然你可以添加到现有的映射，但是你不能修改它。一个字段存在映射的字段，数据有可能已经被索引，如果你修改这个字段的映射，已被索引的数据可能会出错而且不能被搜索。

我们可以更新索引，增加新的字段，但是不能将一个已经存在的字段从`analyzed`修改成`not_analyzed`。

为了演示指定映射的两种方式，我们先删除`gb`这个索引：

```shell
DELETE /gb
```

然后创建索引，指定`tweet`字段将使用`english`分析器：

```shell
PUT /gb 
{
  "mappings": {
    "tweet" : {
      "properties" : {
        "tweet" : {
          "type" :    "string",
          "analyzer": "english"
        },
        "date" : {
          "type" :   "date"
        },
        "name" : {
          "type" :   "string"
        },
        "user_id" : {
          "type" :   "long"
        }
      }
    }
  }
}
```


这将创建指定映射的索引。

随后，我们决定使用`_mapping`接口添加一个`not_analyzed`的`tag`字段到`tweet`的映射：

```shell
PUT /gb/_mapping/tweet
{
  "properties" : {
    "tag" : {
      "type" :    "string",
      "index":    "not_analyzed"
    }
  }
}
```

注意我们并没有列举所有存在的字段，它们本身也不能再次被改变。我们的新字段将被合并到存在的映射。

测试映射
-------------

你可以对`string`字段类型通过分析器API来测试映射，比较两个请求的输出：

```shell
GET /gb/_analyze?field=tweet
1) Black-cats

GET /gb/_analyze?field=tag
2) Black-cats 
```
 

1) 2) 这里是我们要给分析器分析的内容

`tweet`字段产生两个词：`black`和`cat`，而`tag`字段产生一个单独词`Black-cats`。换言之，我们的映射设置正确。

[« 分析和分析器](analysis-and-analyzers.md)     [复杂字段类型  »](complex-core-field-types.md)


复杂核心字段类型
================

* [精确值和全文](exact-values-versus-full-text.md)
* [反向索引](inverted-index.md)
* [分析和分析器](analysis-and-analyzers.md)
* [映射](mapping.md)
* [复杂核心类型](complex-core-field-types.md)

除了我们提到的简单标量数据类型，JSON还有`null`、`array`和`object`这几种类型，它们都被Elasticsearch所支持。

多值字段
--------

很有可能我们需要`tag`字段包含不止一个标签，替代一个`string`类型，我们可以索引一个标签数组：

```json
{ "tag": [ "search", "nosql" ]}
```

数组的映射没有特别需要增加的，任何字段可以包含零个、一个或多个值，全文字段也是被分析后生成多个术语。


这就意味着数组的值必须拥有相同的数据类型。你不能把日期和字符串混合着用。如果您创建了一个新字段用来索引数组，Elasticsearch将使用第一个值的数据类型来推断新字段的类型。

> 注意
-----------
当你从Elasticsearch得到一个文档，任何数组的顺序将会和你索引文档时保持一致。`_source`字段返回JSON文档和你索引时完全一样。

> 然而，因为要可搜索，数组被存储为无序的多值字段。在搜索时,你不能指名分会“第一个元素”或“最后一个元素”。相反，你需要把数组认为是一袋子值。

空字段
========

当然，数组可以是空的。这相当于有0个值。事实上，没有办法在Lucene中存储一个null值，所以一个值为null的字段也被认为是一个空字段。

下面3种字段都被认为是空的，不会被索引：

```
"null_value":               null,
"empty_array":              [],
"array_with_null_value":    [ null ]
```

多层次对象
=========

我们最后需要讨论的一种JSON数据类型是`object`——在其他语言被称为`hash`、`hashmap`、`dictionary`或`associative array`。

内部对象通常被用来在一个实体或对象中嵌入另一个。例如，我们在`tweet`的文档中去掉`user_name`和`user_id`这两个字段，把它写成如下的形式：

```json
{
    "tweet":            "Elasticsearch is very flexible",
    "user": {
        "id":           "@johnsmith",
        "gender":       "male",
        "age":          26,
        "name": {
            "full":     "John Smith",
            "first":    "John",
            "last":     "Smith"
        }
    }
}
```

内部对象的映射
---------------------

Elasticsearch将动态推断新的对象字段并映射成`object`类型，并把内部的字段在`properties`下边罗列出来:

```json
{
  "gb": {
    "tweet": {   <1>
      "properties": {
        "tweet":            { "type": "string" },
        "user": {    <2>
          "type":             "object",
          "properties": {
            "id":           { "type": "string" },
            "gender":       { "type": "string" },
            "age":          { "type": "long"   },
            "name":   {    <3>
              "type":         "object",
              "properties": {
                "full":     { "type": "string" },
                "first":    { "type": "string" },
                "last":     { "type": "string" }
              }
            }
          }
        }
      }
    }
  }
}
```

> 1. 根对象
2. 内部对象
3. 内部对象


`user`和`name`字段的映射和`tweet`自身的映射结构相同。实际上，这种类型映射只是一种特殊类型的`object`映射，我们称之为根对象。这和任何其他对象一样，除了一些特殊的顶层元数据字段，如`_source`和`_all`字段。

内部对象如何被索引
----------------------

Lucene并不了解什么是内部对象，一个Lucene文档由一个平键值对列表。为了让Elasticsearch有效地索引内部对象，它将我们的文档转换成这样：

```json
{
    "tweet":            [elasticsearch, flexible, very],
    "user.id":          [@johnsmith],
    "user.gender":      [male],
    "user.age":         [26],
    "user.name.full":   [john, smith],
    "user.name.first":  [john],
    "user.name.last":   [smith]
}
```

内部字段可以用名字来引用（例如，`first`）。为了区分两个具有相同名称的字段，我们可以使用全路径（例如，`user.name.first`）或者类型名称加路径（如`tweet.user.name.first`）。


> 注意
--------
在前面的简单的扁平的文档，没有字段称为`user`，也没有字段成为`user.name`。Lucene只索引标量或简单的值，而不是复杂的数据结构。

内部对象数组
------------

最后，来考虑一下如何对包含内部对象的数组进行索引。假设我们有一个这样的数组：

```json
{
    "followers": [
        { "age": 35, "name": "Mary White"},
        { "age": 26, "name": "Alex Jones"},
        { "age": 19, "name": "Lisa Smith"}
    ]
}
```
如前所述，这个文档将被扁平化处理，变成这样：

```json
{
    "followers.age":    [19, 26, 35],
    "followers.name":   [alex, jones, lisa, smith, mary, white]
}
```

`{age:35}`和`{name: Mary White}`之间的关联丢失了，每一个多值字段只是一袋子零散的值，而不是有序的数组。我们这样问是可以的：“有一个26岁的追随者吗？”

但是如果我们这样问：“有一个26岁名叫Alex Jones的追随者吗？”，是得不到准确的答复的。

被称为嵌套对象的相关内部对象，能回答这样的查询，我们会在《[嵌套对象](nested-objects.md)》一节中予以介绍。

[« 映射](mapping.md)     [全体搜索 »](full-body-search) 


倒排索引
=======

* [精确值和全文](exact-values-versus-full-text.md)
* [倒排索引](inverted-index.md)
* [分析和分析器](analysis-and-analyzers.md)
* [映射](mapping.md)
* [复杂核心类型](complex-core-field-types.md)

Elasticsearch使用倒排索引这种结构来提供快速全文搜索。一个倒排索引包含出现在任何文档的唯一词列表，对每一个词，也包含所有它出现的文档列表，

假设我们有两个文档，分别有一个`content`字段，内容如下：

1. The quick brown fox jumped over the lazy dog
2. Quick brown foxes leap over lazy dogs in summer

为了创建倒排索引，我们先要将`content`字段进行分词（我们称之为词，或者口令），创建一个包含所有唯一词的有序列表。并列举出现过他们的文档。最后得出近似的结果：

Term    | Doc_1 | Doc_2
--------|:-----:|:----:
Quick   |       |  X    
The     |   X   |
brown   |   X   |  X
dog     |   X   |
dogs    |       |  X
fox     |   X   |
foxes   |       |  X
in      |       |  X
jumped  |   X   |
lazy    |   X   |  X
leap    |       |  X
over    |   X   |  X
quick   |   X   |
summer  |       |  X
the     |   X   |

现在，如果我们查询`quick brown`，我们仅仅需要查询每个词分别出现在哪个文档：


Term    | Doc_1 | Doc_2
--------|:-----:|:------:
brown   |   X   |  X
quick   |   X   |
**Total**   |   **2**   |  **1**


两个文档都有匹配，但是第1个文档比第2个文档更加匹配。如果我们提供一个只是计算词匹配次数的单纯算法，那么我们可以说第一个文档比第2个文档更加匹配，更加有关。

不过现在这个倒排索引还有一些小问题：

* `Quick`和`quick`会被分成两个词，但是用户会认为它们是同一个词。
* `fox`和`foxes`非常相似，`dog`和`dogs`也是一样；它们的词根相同。
* `jumped`和`leap`，词根不一样，但是意思相近，它们是同义词。

使用前面的索引，查询`+Quick +fox`将不会匹配任何文档（记住，前面的`+`表示该词必须出现）。`Quick`和`fox`两个词必须同时出现在文档，但是第一个文档包含`quick fox`，而第2个文档包含`Quick foxes`。


我们的用户可能期望两个文档都能匹配查询。我们可以做得更好！

如果我们将词都校正为标准格式，那么我们可以找到的文档可能和用户请求的不是完全匹配，但已经足够匹配了。比如：

* `Quick`变成小写`quick`。
* `foxes`保留词根，变成`fox`。同样，`dogs`变成单数形式`dog`。
* `jumped·和`leap·是同义词，被索引到同一个词`jump`.

现在索引变成这个样子了：

Term    |  Doc_1|  Doc_2
--------|-------|--------
brown   |   X   |  X
dog     |   X   |  X
fox     |   X   |  X
in      |       |  X
jump    |   X   |  X
lazy    |   X   |  X
over    |   X   |  X
quick   |   X   |  X
summer  |       |  X
the     |   X   |  X

不过，我们还没完全实现我们的想法。当搜索`+Quick +fox`的时候仍然不会有任何结果，因为在我们的索引里已经没有Quick这个词。然而，如果我们使用之前针对`content`字段的规则来校正查询字符，就会变成`+quick +fox`，这样两个文档都将被匹配到。


> 提示
-------------
这一点非常重要，通过索引只能查询到词，所以被索引的文本和查询词必须统一为同样的形式。

这个标记和统一的过程被称为`分析`，我们将在下一节讨论到。

[« 精确值和全文](exact-values-versus-full-text.md)     [分析和分析器  »](analysis-and-analyzers.md)

精确值和全文
=============================

* [精确值和全文](exact-values-versus-full-text.md)
* [反向索引](inverted-index.md)
* [分析和分析器](analysis-and-analyzers.md)
* [映射](mapping.md)
* [复杂核心类型](complex-core-field-types.md)

Elasticsearch里的数据可以广义地分为两种类型：精确值和全文。

精确值就是和他们本身完全一样。比如一个日期或者一个用户ID，但是也包括精确字符串，比如一个用户名或者一个邮件地址。Foo和foo是不同的精确值。同样，2014和2014-09-15也不一样。

另一方面，全文指文本型数据，一般使用人类语言，比如tweet或者邮件的内容。

> 提示
----------------
全文经常指非结构化数据——不对，自然语言是高度结构化的。问题是自然语言的规则过于复杂，以至于电脑很难解析正确。比如，看看下面这个句子：

> May is fun but June bores me.
这是指月份还是人名呢？

精确值很容易查询。只有两种情况：和查询匹配，或者不匹配。这种类型的查询很容通过SQL表达：



```sql
WHERE name    = "John Smith"
  AND user_id = 2
  AND date    > "2014-09-15"
```

查询全文数据则要微妙得多。我们不仅仅是问：“这个文档是否和查询匹配”，还会问“这个文档和查询匹配到什么程度？”换言之，文档和查询的相关度有多少。

我们很少要求和全文完全匹配，相反，我们想要在文本字段内搜索。不仅如此，我们更希望搜索意图能被理解：

* 搜索`UK`应该返回提到`United Kingdom`的文档。
* 搜索`jump`（中文意思：跳跃）应该匹配`jumped`、`jumps`、`jumping`，甚至`leap`（中文意思：飞跃）。
* `johnny walker`要匹配上`Johnnie Walker`，`johnnie depp`应该匹配上`Johnny Depp`。
* `fox news hunting`应该返回在福克斯新闻的花边新闻，而`fox hunting news`则回关于猎狐的新闻故事。

为了便于这些全文查询字段，Elasticsearch先分析文本，然后使用分析后的结果构建反向索引。在接下来的两节里我们将讨论反向索引及其分析流程。

[反向索引  »](inverted-index.md)

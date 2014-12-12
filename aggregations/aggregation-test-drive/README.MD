aggregation test-driveedit

adding a metric to the mix
buckets inside buckets
one final modification

我们可以在接下来的几页定义各种聚合及其语法，但最好学习聚合的方法还是通过例子。聚合的语法相当简单，重要地还在于你学会如何看待聚合，并用合适的方法进行构建。

所以让我们现在就开始一个例子。我们将构建一些聚合帮助汽车经销商。我们的数据是关于汽车交易：汽车型号、制造商、售价、何事卖出等等。

我们先提交一批数据以供使用：


```
POST /cars/transactions/_bulk
{ "index": {}}
{ "price" : 10000, "color" : "red", "make" : "honda", "sold" : "2014-10-28" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "honda", "sold" : "2014-11-05" }
{ "index": {}}
{ "price" : 30000, "color" : "green", "make" : "ford", "sold" : "2014-05-18" }
{ "index": {}}
{ "price" : 15000, "color" : "blue", "make" : "toyota", "sold" : "2014-07-02" }
{ "index": {}}
{ "price" : 12000, "color" : "green", "make" : "toyota", "sold" : "2014-08-19" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "honda", "sold" : "2014-11-05" }
{ "index": {}}
{ "price" : 80000, "color" : "red", "make" : "bmw", "sold" : "2014-01-01" }
{ "index": {}}
{ "price" : 25000, "color" : "blue", "make" : "ford", "sold" : "2014-02-12" }
```
现在我们有了一些数据，让我们开始构建第一个聚合。汽车经销商可能想知道哪种颜色的汽车销售最好，这很容易通过一个简单的聚合实现，我们将使用术语桶来实现这个：


```
GET /cars/transactions/_search?search_type=count <1>
{
    "aggs" : { <2>
        "colors" : { <3> 
            "terms" : {
              "field" : "color" <4>
            }
        }
    }
}
```

> 1. 因为我们不关心搜索结果，我们使用`count`这种更快的`search_type`（搜索类型）。
2. 聚合通过最外层的`aggs`参数标明(如果你喜欢，也可以使用`agregations`)。
3. 根据我们的意愿对聚合进行命名，这个例子使用了`colors`。
4. 最后，我们定义了一个`terms`类型的桶。

聚合是在搜索结果的上下文中执行，这意味着它只是一个搜索接口(如`/_search`接口)最外层的参数。聚合可以和查询匹配，不过我们将会在《[范围聚合](scoping-aggregations.md)》一节中详细介绍。

接下来,我们对集合定义一个名字。名字有拟定，返回的数据将会带上你提供的名字，便于应用程序分析结果。

之后我们定义了聚合本身。比如，我们顶一个单独的术语桶，这个术语桶将给它遇到的每一个术语创建一个新桶。当我们告诉它使用`color`这个字段，术语桶将未每种颜色创建一个桶。

我们执行一下这个聚合，结果如下：

```json
{
   ...
   "hits": {
      "hits": [] <1>
   },
   "aggregations": {
      "colors": { <2>
         "buckets": [
            {
               "key": "red", <3>
               "doc_count": 4  <4>
            },
            {
               "key": "blue",
               "doc_count": 2
            },
            {
               "key": "green",
               "doc_count": 2
            }
         ]
      }
   }
}
```


> 1. 没有命中的文档返回，是因为我们使用了`search_type=count`参数。
2. 这里使用了我们给聚合定义的名字`colors`。
3. 每一个桶的`key`为`color`字段里边找到的唯一的术语。通常还包括`doc_count`字段，表示这个术语对应的文档个数。
4. 每个桶的数量表示是这种颜色的文档个数。


返回结果包含一系列的桶，每个桶对应一种独特的颜色(例如，`red`或`green`)。每个桶还包括“调入”特定的桶的文档数量。例如,有4辆红色的汽车。


前面的例子完全实时操作：如果文档可以搜索， 它们就能被聚合。这意味着你可以直接聚合结果，利用一个图形库生成实时的仪表板。只要你卖掉一辆银色轿车，统计图上的银色汽车统计信息将会动态更新。

瞧，你的第一个聚合！

--------------------

[« 组合两者](combining-the-two.md)     [混合度量 »w](adding-a-metric-to-the-mix.md)

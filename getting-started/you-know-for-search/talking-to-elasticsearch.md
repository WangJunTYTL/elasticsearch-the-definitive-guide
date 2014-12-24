
和Elasticsearch交流
==================

* [安装Elasticsearch](installing-elasticsearch.md)
* [运行Elasticsearch](running-elasticsearch.md)
* [和Elasticsearch交流](talking-to-elasticsearch.md)
* [面向文档](document-oriented.md)
* [发现你的足迹](finding-your-feet.md)
* [索引员工文档](indexing-employee-documents.md)
* [检索一个文档](retrieving-a-document.md)
* [搜索精简版](search-lite.md)
* [使用查询DSL搜索](search-with-query-dsl.md)
* [复杂搜索](more-complicated-searches.md)
* [全文搜索](full-text-search.md)
* [短语搜索](phrase-search.md)
* [高亮搜索](highlighting-our-searches.md)
* [分析](analytics.md)
* [教程结论](tutorial-conclusion.md)
* [分布式特性](distributed-nature.md)
* [接下来](next-steps.md)

你怎样和Elasticsearch交流取决于你是否使用Java。

java api
--------

如果你使用Java，Elasticsearch提供了两个内建客户端供你直接在代码中使用：


### 节点客户端

> 节点客户端作为一个没有数据的节点加入一个本地集群。换句话说，它本身不存储任何数据，但是它知道数据都分别存在急群众的哪个节点上，并把请求转发给正确的节点。

### 传输客户端

> 轻量级传输客户端能用来发送数据到远程的集群。它本身不加入集群，只是简单地把请求转发到集群的某一个节点。

两个Java客户端都和集群的9300端口通讯，使用本地Elasticsearch传输协议。急群中的节点相互通讯也是通过9300端口。如果这个端口挂掉了，你的节点就没法形成一个集群。

> 提示
-----
Java客户端必须和Elasticsearch的节点保持同样的版本，否则可能无法相互通讯。

更多关于Java客户端的信息可以在《[指南](http://www.elasticsearch.org/guide/)》的Java API相关章节找到。

json格式的HTTP RESTful API
-------------------------

所有语言都可以使用RESTful API通过9200端口和Elasticsearch通讯，你可以选择最喜欢的web客户端。实际上你应该注意到，可以使用`curl`用命令行的形式和Elasticsearch交流。


> 备注
-----
Elasticsearch提供各种语言（Groovy、Javascript、.NET、PHP、Perl、Python、Ruby）的官方客户端，以及各种社区（提供客户端和集成），这些都可以在《[指南](http://www.elasticsearch.org/guide/)》找到。

Elasticsearch的请求就是包含HTTP请求同样的部分。比如，要计算集群中的文档综述，我们可以这样来使用它：


```shell
       <1>   <2>                   <3>    <4>                        
curl -XGET 'http://localhost:9200/_count?pretty' -d '
{  <5>
    "query": {
        "match_all": {}
    }
}
```

> [1] 可用的HTTP方法：GET、POST、PUT、HEAD或者DELETE。
[2] 集群中任意节点的协议、主机名、以及端口
[3] 请求的路径（path）。
[4] 可选的查询字符串参数（比如，`?pretty`将使返回的JSON字符串将以pretty（漂亮）的形式展示，更加便于阅读。
[5] JSON编码格式的请求体（如果请求需要才使用）

---------------------------

Elasticsearch返回一个HTTP状态码（比如`200 OK`）以及JSON编码的响应体（HEAD请求方法除外）。上面的curl请求返回的JSON响应内容如下：

```json
{
    "count" : 0,
    "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
    }
}
```
上面的内容并没有包含HTTP响应头，因为并没有要求curl展示他们。要看到响应头，使用curl命令时刻加入`-i`参数：

```
curl -i -XGET 'localhost:9200/'
```
这本书剩下的部分，我们在展示curl例子时，将使用一种简单的格式：去掉完全相同的部分，比如主机名和端口，以及curl命令本身。比如一个完整的命令：

```shell
curl -XGET 'localhost:9200/_count?pretty' -d '
{
    "query": {
        "match_all": {}
    }
}'
```
我们将以下面这种精简的格式呈现：

```shell
GET /_count
{
    "query": {
        "match_all": {}
    }
}
```

实际上，在Sense终端（和Marvel一起安装的）上我们使用的就是这种格式。如果使用本书的在线版本，你可以点击“View in Sense"链接在Sense中运行这些示例代码。

--------------

[« 运行Elasticsearch](running-elasticsearch.md)     [面向文档 »](document-oriented.md) 


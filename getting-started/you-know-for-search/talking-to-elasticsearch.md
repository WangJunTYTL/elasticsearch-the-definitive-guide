
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
Elasticsearch提供各种语言（Groovy、Javascript、.NET、PHP、Perl、Python、Ruby）的官方客户端，以及各种社区（提供客户端和集成），这些都可以在《[指南](http://www.elasticsearch.org/guide/》找到。

A request to Elasticsearch consists of the same parts as any HTTP request. For instance, to count the number of documents in the cluster, we could use this:

                                    
curl -XGET 'http://localhost:9200/_count?pretty' -d '
{  
    "query": {
        "match_all": {}
    }
}
'


The appropriate HTTP method or verb: GET, POST, PUT, HEAD, or DELETE



The protocol, hostname, and port of any node in the cluster



The path of the request



Any optional query-string parameters (for example ?pretty will pretty-print the JSON response to make it easier to read)



A JSON-encoded request body (if the request needs one)

Elasticsearch returns an HTTP status code like 200 OK and (except for HEAD requests) a JSON-encoded response body. The preceding curl request would respond with a JSON body like the following:

{
    "count" : 0,
    "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
    }
}
We don’t see the HTTP headers in the response because we didn’t ask curl to display them. To see the headers, use the curl command with the -i switch:

curl -i -XGET 'localhost:9200/'
For the rest of the book, we will show these curl examples using a shorthand format that leaves out all the bits that are the same in every request, like the hostname and port, and the curl command itself. Instead of showing a full request like

curl -XGET 'localhost:9200/_count?pretty' -d '
{
    "query": {
        "match_all": {}
    }
}'
we will show it in this shorthand format:

GET /_count
{
    "query": {
        "match_all": {}
    }
}
VIEW IN SENSE
In fact, this is the same format that is used by the Sense console that we installed with Marvel. If in the online version of this book, you can open and run this code example in Sense by clicking the View in Sense link above.

«  running elasticsearch     document oriented  »


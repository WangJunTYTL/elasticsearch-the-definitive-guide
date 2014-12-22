
安装Elasticsearch
========

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

理解Elasticsearch最简单的方式就是和它一起玩，所以让我们开始吧！

安装Elasticsearch的唯一必备条件是最新版本的Java。你最好从Java官网[www.java.com](http://www.java.com)安装。

你可以从 [http://elasticsearch.org/download](http://elasticsearch.org/download)下载到最新版本的Elasticsearch。

```shell
curl -L -O http://download.elasticsearch.org/PATH/TO/VERSION.zip <1>
unzip elasticsearch-$VERSION.zip
cd  elasticsearch-$VERSION
```

> [1] 从[http://elasticsearch.org/download](http://elasticsearch.org/download)找到最新可用版本填充这个网址。

----------------

> 提示
-----
在生产环境使用Elasticsearch时，你可以用前面这个方法，或者使用[下载页](http://www.elasticsearch.org/downloads)的Debian或者RPM包。你可以使用官方提供的[Puppet模块](https://github.com/elasticsearch/puppet-elasticsearch)或者[Chef cookbook](https://github.com/elasticsearch/cookbook-elasticsearch)。

安装marvel
-------------


[Marvel](http://www.elasticsearch.com/products/marvel)是Elasticsearch的一个管理和监控工具，免费提供给开发时使用。它提供了一种叫`Sense`的交互界面，可以很方面地通过浏览器和Elasticsearch直接交流。

本书在线版本的很多代码示例都包含`View in Sense`的链接。点击链接，会打开一个`Sense`控制台的工作例子。你根本不用安装`Marvel`，却可以轻松体验这些示例代码在你本地Elasticsearch集群运行的场景，使得本书更有交互性。

Marvel是一个插件，要下载并安装它，只要在Elasticsearch目录运行下面的命令即可：

```shell
./bin/plugin -i elasticsearch/marvel/latest
```
如果你不想Marvel监控你本地的集群，可以通过下面的命令禁止数据采集：

```shell
echo 'marvel.agent.enabled: false' >> ./config/elasticsearch.yml
```
------------------

[« 你知道，为了搜索...](you-know-for-search.md)     [运行Elasticsearch »](running-elasticsearch.md) 

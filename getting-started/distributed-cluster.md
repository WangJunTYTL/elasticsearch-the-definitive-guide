life inside a clusteredit

an empty cluster
cluster health
add an index
add failover
scale horizontally
coping with failure
Supplemental Chapter

As mentioned earlier, this is the first of several supplemental chapters about how Elasticsearch operates in a distributed environment. In this chapter, we explain commonly used terminology like cluster, node, and shard, the mechanics of how Elasticsearch scales out, and how it deals with hardware failure.

Although this chapter is not required reading—you can use Elasticsearch for a long time without worrying about shards, replication, and failover—it will help you to understand the processes at work inside Elasticsearch. Feel free to skim through the chapter and to refer to it again later.

Elasticsearch is built to be always available, and to scale with your needs. Scale can come from buying bigger servers (vertical scale, or scaling up) or from buying more servers (horizontal scale, or scaling out).

While Elasticsearch can benefit from more-powerful hardware, vertical scale has its limits. Real scalability comes from horizontal scale—the ability to add more nodes to the cluster and to spread load and reliability between them.

With most databases, scaling horizontally usually requires a major overhaul of your application to take advantage of these extra boxes. In contrast, Elasticsearch is distributed by nature: it knows how to manage multiple nodes to provide scale and high availability. This also means that your application doesn’t need to care about it.

In this chapter, we show how you can set up your cluster, nodes, and shards to scale with your needs and to ensure that your data is safe from hardware failure.
